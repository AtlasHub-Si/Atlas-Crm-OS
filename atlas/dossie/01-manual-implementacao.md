# 01 — Manual de implementação

> Duas vias: **A) AtlasDesk** (uma instalação, muitos clientes) e **B) instalação
> dedicada** por cliente. A via A é a recomendada para receita recorrente; a B para
> enterprise com exigência de isolamento.
> Referência de código: `v1.114.0`. Comandos conferidos no repositório local.

---

## 1. Pré-requisitos

| Recurso | Mínimo | Recomendado |
|---|---|---|
| VPS | 2 vCPU / 4 GB RAM + 4 GB swap | 4 vCPU / 8 GB RAM |
| Disco | 40 GB SSD | 80 GB SSD |
| Sistema | Ubuntu 22.04/24.04 com Docker | idem |
| Domínio | 1 subdomínio por instância | + subdomínios por cliente (marca própria) |
| Base de dados | Supabase Cloud (grátis serve para piloto) | Supabase Pro |
| Email | SMTP (Brevo, Resend, Gmail Workspace) | domínio verificado com SPF/DKIM |
| WhatsApp | número dedicado por cliente | número novo, aquecido 7–14 dias |

> 4 GB é o **mínimo real** (WAHA + Node + Postgres cliente). Com menos, o sistema entra em
> swap e a sessão do WhatsApp cai.

---

## 2. Via A — AtlasDesk (multi-tenant, uma instalação)

### 2.1 Arquitetura

```
Internet
  │  HTTPS (Caddy/Traefik + TLS automático)
  ▼
deskcomm-app (Next.js)  ──►  Supabase (Postgres + Auth + Storage)
  │                              ▲
  ├── deskcomm-worker (agentes/IA)┘
  ├── deskcomm-scheduler (cron, cadências, filas)
  ├── deskcomm-waha (WhatsApp, 1 sessão por número/cliente)
  └── redis + serverless-redis-http
```

O isolamento entre clientes é feito por **`organization_id` + RLS** (649 políticas). Não é
preciso um contentor por cliente; é preciso **quota** por cliente.

### 2.2 Instalação (caminho oficial, 1 comando)

```bash
# na VPS
git clone --depth 1 https://github.com/melgarafael/DeskcommCRM.git
cd DeskcommCRM
bash hostgator-setup-kit/install.sh
```

O instalador:
1. verifica Docker, portas e recursos (`preflight`);
2. cria/valida a base Supabase (cloud ou local, conforme escolha);
3. aplica o schema (`supabase/baseline.sql`);
4. gera chaves (`AI_CRED_AES_KEY`, `CPF_ENCRYPTION_KEY`, etc.) no `.env`;
5. sobe app, worker, scheduler, WAHA, Redis;
6. imprime as **credenciais iniciais** e o estado de saúde.

Não mostrar essas credenciais em gravações. Guardar em gestor de segredos.

### 2.3 Variáveis que não podem faltar

| Variável | Para quê |
|---|---|
| `NEXT_PUBLIC_SUPABASE_URL` | endereço do projeto Supabase |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | chave pública do browser |
| `SUPABASE_SERVICE_ROLE_KEY` | chave de servidor (nunca no browser) |
| `SUPABASE_DB_URL` | ligação direta para worker/migrations |
| `NEXT_PUBLIC_APP_URL` | URL pública (HTTPS) — usada em webhooks e cobrança |
| `AI_CRED_AES_KEY` | cifra das chaves de IA e cobrança |
| `WAHA_API_BASE_URL` / `WAHA_API_KEY` | ligação ao motor WhatsApp |
| `SMTP_*` | emails de convite, recuperação e cobrança |

### 2.4 Base de dados: Supabase Cloud (caminho rápido, testado)

1. Criar projeto em `supabase.com` (região UE para clientes europeus).
2. Copiar URL, `anon key` e `service_role key`.
3. Aplicar o schema:
   ```bash
   psql "$SUPABASE_DB_URL" -f supabase/baseline.sql
   ```
   Confirmar no fim: **232 tabelas, 649 políticas RLS, 0 erros**.
4. Extensões necessárias no schema `public`: `vector`, `citext`, `pg_trgm`, `pgcrypto`.
5. Criar o primeiro utilizador no Auth e promover a dono:
   ```sql
   -- usar a função de provisionamento da instalação
   select public.fn_create_tenant_with_owner(...);
   ```

**Dois obstáculos já encontrados e resolvidos:**
- `db.<ref>.supabase.co` só tem IPv6 e os contentores não têm rota → usar o pooler
  `aws-1-<região>.pooler.supabase.com:5432`.
- O driver Postgres rejeita o certificado do pooler com `sslmode=require` → acrescentar
  `uselibpqcompat=true` à URL.

> Nota operacional: `psql` do sistema pode não aceitar `uselibpqcompat`; remover o
> parâmetro apenas para comandos manuais, mantê-lo na URL da aplicação.

### 2.5 WhatsApp

1. No CRM: **Canais → WhatsApp → nova sessão**.
2. Ler o QR com o telemóvel do cliente (Aparelhos conectados).
3. Confirmar `WORKING` no health check.
4. Aquecer o número: 7 dias com volume crescente; não disparar campanha no dia 1.
5. Um número por cliente/organização. Nunca partilhar número entre clientes.

### 2.6 IA

1. **IA → Credenciais**: inserir chave do fornecedor (OpenAI, Anthropic, Google, etc.).
2. **IA → Agentes**: criar agente, escolher modelo, definir instruções e skills.
3. **IA → Conhecimento**: carregar documentos (preços, horários, políticas).
4. **IA → Uso**: definir teto mensal por organização (protege a margem).
5. Testar em **IA → Atendimento** antes de ligar ao WhatsApp.

### 2.7 Marca e planos

1. `/admin/marca`: nome, cor, logo, ícone, CSS (claro/escuro).
2. **Cobrança → Planos**: criar planos em EUR com `preço`, `intervalo`, `trial`,
   `max_assentos`, `max_canais`, `teto_ia_usd_cents`.
3. **Cobrança → Conexão**: Stripe (EUR/USD/BRL) ou Asaas (BRL).
4. Testar com chave de teste: registo → checkout → webhook → assinatura ativa.

### 2.8 Provisionar um cliente novo (agência)

1. **Admin global → Organizações → Nova** (ou consola de agência, se ligada).
2. Definir nome, identificador, idioma, plano.
3. Convidar o dono do cliente (email).
4. O cliente lê o QR do WhatsApp dele.
5. Carregar conhecimento e criar o primeiro agente.
6. Verificar: mensagem recebida → agente responde → conversa aparece no inbox.

Tempo-alvo: **30–45 min** por cliente já padronizado.

---

## 3. Via B — instalação dedicada

Para enterprise, setor regulado ou cliente que exige servidor próprio.

- Uma VPS + um Supabase por cliente.
- Vantagem: isolamento físico, backup independente, atualizações controladas.
- Desvantagem: custo fixo por cliente (15–40 €/mês) e mais operação.
- Preço: incluir a infraestrutura no contrato (40–120 €/mês) ou passar a fatura ao cliente.

---

## 4. Checklist de aceitação (go-live)

| # | Verificação | Como |
|---|---|---|
| 1 | `GET /api/v1/health` devolve `ok` e a versão | curl |
| 2 | Login do dono funciona e mostra a organização | browser |
| 3 | WhatsApp `WORKING` e recebe mensagem | enviar do telemóvel |
| 4 | Agente responde e faz handoff | conversa de teste |
| 5 | Convite de colega chega por email | SMTP |
| 6 | Backup gerado e restaurado num ambiente limpo | `backup.sh` / `restore.sh` |
| 7 | Cobrança em modo teste: assinatura criada e webhook recebido | Stripe/Asaas |
| 8 | RGPD/LGPD: política, DPA e retenção configurados | jurídico |
| 9 | Teto de IA definido por organização | IA → Uso |
| 10 | Monitorização e alerta de disco/CPU ativos | VPS |

---

## 5. Operação

### Diária
- Health check automático (`hostgator-setup-kit/healthcheck.sh`).
- Sessões WhatsApp em `FAILED` → reconectar.
- Fila de mensagens presas → ver worker.

### Semanal
- Espaço em disco e crescimento da base.
- Custo de IA por cliente vs teto.
- Revisão de conversas com handoff (qualidade do agente).

### Mensal
- Atualização controlada:
  ```bash
  bash hostgator-setup-kit/preflight-upgrade.sh
  bash hostgator-setup-kit/update.sh
  ```
- Backup completo e **teste de restauro**.
- Relatório de uso ao cliente (mensagens, leads, conversões, custo de IA).

### Diagnóstico
```bash
bash hostgator-setup-kit/diagnostico.sh
docker ps
docker logs --tail 200 deskcomm-worker
```

---

## 6. Custos de referência (proposta, a validar)

| Item | Piloto | Produção (10 clientes) |
|---|---|---|
| VPS | 10–20 €/mês | 40–80 €/mês |
| Supabase | 0 € (free) | 25 €/mês (Pro) |
| IA | 10–30 €/mês | 150–400 €/mês (com tetos) |
| Domínio + email | 2 €/mês | 5 €/mês |
| WhatsApp (WAHA) | incluído | incluído |
| **Total** | **~25–50 €** | **~220–510 €** |

Com 10 clientes a 99–249 €/mês, a margem bruta fica entre 65% e 85%, desde que a IA tenha
teto e o suporte seja por marcação (não 24/7 humano).

---

## 7. Segurança e conformidade

- TLS obrigatório; nunca expor `service_role` no browser.
- Chaves de IA e cobrança cifradas em repouso (`AI_CRED_AES_KEY`).
- MFA para administradores (o produto tem política de MFA por papel).
- Backups cifrados, retenção definida, restauro testado.
- Registo de tratamentos (RGPD art. 30), DPA com subprocessadores (Supabase, VPS, LLM,
  Stripe/Asaas, Apify/Scavio).
- Retenção e anonimização por pedido do titular (módulo LGPD).
- Para a UE: revisar vocabulário LGPD → GDPR nas políticas e consentimentos.

---

## 8. Problemas comuns

| Sintoma | Causa provável | Resolução |
|---|---|---|
| Worker reinicia | URL de base errada/IPv6 | usar pooler + `uselibpqcompat=true` |
| WhatsApp `FAILED` | sessão não emparelhada ou número banido | novo QR; número novo |
| Agente não responde | sem credencial de IA ou teto atingido | IA → Credenciais / Uso |
| Emails não chegam | SMTP/SPF/DKIM | verificar domínio |
| Checkout falha | `NEXT_PUBLIC_APP_URL` errado ou webhook não chega | corrigir URL e reconectar |
| Base sem tabelas | `baseline.sql` não aplicado | aplicar e conferir contagens |
| Disco cheio | logs e mídia do WhatsApp | rotação de logs, limpeza de mídia |

---

## 9. O que este manual não cobre

- Migração de dados de outro CRM (mapeamento varia por cliente).
- Integrações certificadas de faturação portuguesa (não existem no produto; ver doc 00 §8).
- Alta disponibilidade multi-região (a arquitetura atual é de instância única).
- Escala acima de ~50 organizações por instância sem testes de carga.
