# 00 — Análise estratégica

> Estado: 17/11/2026. Factos medidos no código e na base de dados de teste.
> Preços e estimativas são **proposta**, não facto.

---

## 1. Sumário executivo

1. **O ativo é grande e real.** 600 rotas de API, 180 páginas, 468 componentes, ~392 mil
   linhas de código, 232 tabelas e 649 políticas de isolamento por cliente (RLS) na base
   instalada. Não é um protótipo: é um CRM omnicanal com agente de IA, faturação própria,
   consola de agência e white-label.
2. **O software é MIT.** Podes hospedar, modificar e revender. Não podes, legalmente,
   "fechar" o código que recebeste. Logo, o fosso **nunca** pode ser o código: tem de ser
   operação, dados, verticalização, formação e marca.
3. **O caminho com mais teto é o AtlasDesk** (subscrição, sem servidor para o cliente),
   com a implementação paga como porta de entrada e o open-source como canal de aquisição.
4. **Boa notícia técnica:** a faturação do produto já cobra **BRL, USD e EUR** via Stripe,
   tem trial configurável e limites por plano (assentos, canais, teto de gasto de IA). O
   AtlasDesk é produto, não projeto de 6 meses.
5. **O maior risco não é técnico, é de posicionamento:** vender "Deskcomm" é vender software
   de terceiros. O produto comercial tem de ser **AtlasDesk**, com o Deskcomm como motor
   declarado (licença MIT), sob pena de a marca nunca ser tua.

---

## 2. O que está a ser analisado (evidência)

| Dimensão | Medição | Fonte |
|---|---|---|
| Rotas de API | 600 | `find app -name route.ts` |
| Páginas | 180 | `find app -name page.tsx` |
| Componentes | 468 | `components/**/*.tsx` |
| Código | ~391.822 linhas (app+lib+components+workers) | `wc -l` |
| Tabelas / funções / políticas RLS | 232 / 677 / 649 | carga do `baseline.sql` |
| Specs técnicas | 48 | `docs/specs/` |
| Idiomas | PT e ES completos; EN e TR e ZH em construção | `lib/i18n/registro.ts` |
| Faturação | Stripe (BRL/USD/EUR) e Asaas (BRL), trial, limites | `lib/cobranca/` |
| Licença | MIT | `LICENSE` |

---

## 3. Inventário funcional (o que o cliente recebe)

### 3.1 Atendimento e canal

- **Inbox unificado** de WhatsApp com assumir, transferir, convidar colega, encerrar e
  histórico por contacto.
- **WAHA** (WhatsApp Web headless) multi-número, com saúde da sessão, reconexão e envio
  de mídia.
- **Janela de 24 h** tratada como boa prática operacional, com throttling, jitter,
  aquecimento de número, rotação de texto e limite diário (anti-banimento).
- **Roteamento por remetente**, etiquetas, respostas rápidas, avisos internos.
- **Chamadas** (`calls`, `wacalls`, `cold-call`) e **videochamada**.
- **Fluxos de atendimento** e um **canal bot** opcional.

### 3.2 CRM e receita

- **Customer 360**: contactos, empresas, vários telefones, deduplicação por nome/telefone.
- **Funil Kanban** multi-pipeline, negócios, etapas, tarefas, agenda e responsáveis.
- **Produtos, propostas, comissões/honorários, faturação e comandas** (módulos verticais).
- **Campanhas** em massa com supressão, limites e métricas; **cadências** de follow-up.
- **Prospecção nativa**: busca Google Maps (Apify ou Scavio), enriquecimento de site e
  redes, importação de planilha até 500 empresas, verificação de WhatsApp, agente criado
  por conversa e fila de abordagem.
- **Radar / índice de atrito**, **análise**, **métricas**, **ouvidoria**, **auditoria**.
- **LGPD**: anonimização, exportação, base legal e retenção.

### 3.3 IA

- **Agentes** com personalidade, skills, routers, memória, follow-ups e casos humanos.
- **Conhecimento (RAG)** por organização, credenciais de fornecedor, limites de gasto e
  painel de uso por modelo.
- **Três papéis do agente** (atender, qualificar, transferir) com governança e handoff.
- **Servidor MCP** interno e externo, extensões declarativas.

### 3.4 Plataforma e administração

- **Multi-tenant com RLS** (o dado de um cliente não é visível a outro).
- **Papéis**: `viewer`, `agent`, `ai_operator`, `manager`, `admin`; administrador global;
  operador de agência com delegação auditada e temporária.
- **Consola de agência**: criar e abrir organizações-cliente dentro de uma cota.
- **White-label**: nome, cor derivada com contraste, logo, ícone, CSS por tema.
- **Cobrança do revendedor**: planos, assinaturas, faturas, régua de cobrança, suspensão e
  reativação automáticas.
- **20 módulos opcionais** ligáveis por instalação (cobrança, agências, B2B, propostas,
  cold call, email, reservas, link de pagamento, etc.).

---

## 4. Diferencial vs commodity

| Capacidade | Commodity hoje | Neste produto |
|---|---|---|
| Inbox WhatsApp | Sim (Kommo, Octadesk, Chatwoot) | Sim, com anti-banimento e multi-número |
| CRM + funil | Sim | Sim, com Customer 360 e vários telefones |
| Agente de IA | Sim, mas caro e fechado | Sim, com RAG, skills, MCP e teto de custo |
| **Prospecção nativa + enriquecimento** | Raro no CRM | **Sim — é aqui que está a margem** |
| **Cobrar os teus clientes de dentro do CRM** | Só em plataformas de agência | **Sim, com Stripe/Asaas e suspensão automática** |
| **White-label + consola de agência** | Plano enterprise caro | **Incluído** |
| Dados no teu servidor | Raro | Sim (self-host) |
| Preço | 15–150 €/utilizador/mês | Custo de infraestrutura + operação |

**Conclusão:** o produto não ganha por "ter inbox". Ganha por juntar **aquisição
(prospecção) + conversão (IA no WhatsApp) + cobrança (receita)** num só sítio, e por poder
ser vendido como plataforma de agência. É essa a história a contar.

---

## 5. Como monetizar: os três caminhos

### Caminho A — Implementação + formação (software grátis)

**O que se vende:** instalação, configuração, migração, ligação ao WhatsApp, criação de
agentes, formação e acompanhamento. O software é gratuito (MIT).

| Item | Proposta de preço (EUR, a validar) |
|---|---|
| Instalação base (VPS, domínio, base, WhatsApp, 1 agente) | 1.500–3.500 € |
| Arranque completo PME (funil, campanhas, 3 agentes, formação 4 h) | 3.500–7.500 € |
| Projeto mid-market (integrações, SSO, migração, 2 verticais) | 8.000–25.000 € |
| Formação por sessão (até 8 pessoas) | 400–900 € |
| Acompanhamento mensal (horas + atualizações) | 250–600 €/mês |

**Vantagem:** cash imediato, sem risco de infraestrutura.
**Limite:** é um negócio de horas. Sem recorrência, o teto é o teu calendário. Serve para
financiar e validar, não como destino.

### Caminho B — AtlasDesk (SaaS, sem servidor para o cliente)

**O que se vende:** acesso web, 24/7, atualizações, backups, suporte, WhatsApp incluído na
operação. O cliente não instala nada.

**Arquitetura recomendada:** **uma instalação multi-tenant** (não uma por cliente) com o
módulo de agências ligado. Vantagens: um deploy, um backup, custo marginal baixo. Riscos:
um incidente afeta todos; exige disciplina de quotas e observabilidade.

**Economia unitária (proposta):**

| Plano | Preço/mês | Inclui | Custo estimado de servir |
|---|---|---|---|
| Solo | 39–59 € | 1 assento, 1 número, 1 agente, teto IA baixo | 4–8 € |
| Equipa | 99–149 € | 3–5 assentos, 1–2 números, 2 agentes | 8–15 € |
| Negócio | 249–399 € | 10 assentos, multi-número, 5 agentes, relatórios | 20–35 € |
| Enterprise / agência | 600–1.500 € | assentos e números à medida, SLA, marca própria | 50–120 € |

Custos dominantes: VPS (15–40 €), Supabase (0–25 €), IA (variável — **nunca vender IA
ilimitada**), e o teu tempo de suporte. A margem bruta saudável é 70–85%.

**Alavancas de preço já no produto:** `max_assentos`, `max_canais`, `teto_ia_usd_cents`,
`trial_dias`, plano padrão no registo. Não é preciso inventar nada.

### Caminho C — Open-source novamente, com melhorias nossas

**O que se devolve à comunidade:** o motor, com as nossas melhorias (captura de leads,
verticalizações, traduções, correções).

**Porque faz sentido:** é o canal de aquisição mais barato que existe. Quem testa o
open-source e não quer gerir servidor compra AtlasDesk; quem quer controlo paga
implementação; quem é agência paga licença de operação.

**O que não se devolve (o fosso):**
- pacotes verticais prontos (clínicas, imobiliárias, contabilidade, advocacia);
- biblioteca de skills e prompts afinados por vertical;
- enriquecimento de leads e listas proprietárias;
- operação (SLA, backups, suporte) e marca.

**Atenção legal:** o projeto é MIT. Ao republicar, mantém o `LICENSE` e os avisos de
copyright, não sugeres endosso do autor original e não usas a marca "Deskcomm" como tua.
O nome comercial é AtlasDesk; "motor DeskcommCRM (MIT)" é crédito, não marca.

### Veredicto

**Modelo híbrido, por esta ordem:** AtlasDesk como receita principal → implementação paga
como entrada e caixa → open-source como funil e prova de competência. Vender só
implementação é vender horas; vender só SaaS sem serviço não converte PME.

---

## 6. O que falta para o AtlasDesk ser produto (lacunas reais)

| Lacuna | Impacto | Esforço |
|---|---|---|
| ~~Cobrança em EUR~~ | **Resolvido**: CHECK aceita BRL/USD/EUR; Stripe cobra na moeda do plano | — |
| Nenhum plano criado na base | Não há nada para vender no checkout | 1 h (ecrã de planos) |
| Registo público e onboarding | Existe `/get-started`, mas falta afinar limites e trial | 1–3 dias |
| Marca própria | White-label existe; falta domínio, emails e contratos | 1 dia |
| Isolamento de recursos por cliente | RLS isola dados; CPU/IA/WhatsApp não têm quota por tenant além do teto de IA | 3–5 dias |
| Observabilidade multi-cliente | Precisa de dashboards por organização e alertas | 2–4 dias |
| Backup e restauro testados | Crítico para SLA | 1–2 dias |
| Páginas legais/DSA/GDPR | O produto fala LGPD; para a UE falta GDPR, DPA e subprocessadores | 2–3 dias (jurídico) |
| Suporte e SLA | Não existe processo | contínuo |

**Regra:** só se vende AtlasDesk com backup testado, DPA e um processo de suporte. Sem
isso, vende-se implementação.

---

## 7. Onde agregar valor (por ordem de retorno)

1. **Dados e aquisição (maior margem).** A prospecção nativa é o que nenhum concorrente
   barato tem. Transforma-a em produto: "50 contactos qualificados do teu setor por mês" é
   mais fácil de vender do que "software".
2. **Pacotes verticais.** Um CRM genérico exige configuração; um pacote "Clínica" com
   funil, agentes, lembretes, no-show e consentimento vende-se sozinho. O repositório já
   tem sementes por vertical em `curso/demonstracoes/` (clínicas, imobiliárias, advocacia,
   contabilidade, seguros, educação, e-commerce).
3. **Skills de IA por setor.** O produto já tem skills, routers e memória. Uma biblioteca
   afinada (triagem, marcação, cobrança, reclamação) é difícil de copiar e fácil de cobrar.
4. **Integrações locais.** Faturação certificada PT (Moloni, InvoiceXpress, Vendus),
   Google Calendar, pagamentos (Stripe/IfthenPay), e-commerce (Shopify, WooCommerce,
   Nuvemshop). Cada integração abre um segmento.
5. **Operação gerida.** Dashboards, relatórios mensais, revisão de qualidade das conversas
   e otimização de campanhas: recorrência de serviços com margem alta.

---

## 8. Combinações com outras soluções

| Combinação | Para quem | Valor |
|---|---|---|
| AtlasDesk + faturação certificada PT | PME portuguesas | Fecha o ciclo venda→fatura |
| AtlasDesk + Google Calendar/Outlook | clínicas, consultores | Marcação sem fricção |
| AtlasDesk + Stripe/IfthenPay | serviços, e-commerce | Pagamento no chat |
| AtlasDesk + Shopify/WooCommerce | lojas | Recuperação de carrinho e pós-venda |
| AtlasDesk + Power BI/Metabase | mid-market | Relatórios próprios |
| AtlasDesk + telefonia (Asterisk/VoIP) | call centers pequenos | Voz + WhatsApp no mesmo histórico |
| AtlasDesk + n8n/Make | agências técnicas | Automações sem tocar no código |
| AtlasDesk + LLM local (Ollama) | setores regulados | Dados que não saem do país |

Não construir tudo. Escolher **duas** integrações por trimestre, guiadas por pedidos reais.

---

## 9. Riscos e mitigação

| Risco | Probabilidade | Mitigação |
|---|---|---|
| WhatsApp bloquear números | Alta | Aquecimento, limites, números dedicados, aviso contratual |
| Custo de IA comer a margem | Alta | Teto por plano (já existe), modelo mais barato por tarefa, cache |
| Fork público copiar o negócio | Média | Fosso em dados/vertical/operação, não em código |
| Dependência de terceiros (WAHA, Supabase, Apify) | Média | Camada de abstração; alternativa oficial Meta API a prazo |
| Incidente multi-tenant afetar todos | Média | Backups por organização, testes de restauro, quotas |
| GDPR/DSA | Média | DPA, registo de tratamentos, subprocessadores, retenção |
| Marca de terceiros | Média | Marca própria, crédito ao projeto original, sem endosso implícito |

---

## 10. Roadmap de 90 dias

| Semana | Entrega | Critério de conclusão |
|---|---|---|
| 1–2 | AtlasDesk: marca, domínio, planos, trial, checkout EUR | Um cliente de teste paga e entra sozinho |
| 2–3 | Backup/restauro documentado e testado | Restauro completo num ambiente limpo |
| 3–4 | Pacote vertical "Clínica" (funil + 3 agentes + agenda) | Demo de 15 min sem tocar em configuração |
| 4–6 | Manual de implementação + manual de utilizador | Um técnico novo instala sem ajuda |
| 6–8 | EN ligado; FR em tradução | Seletor mostra EN; guarda i18n passa |
| 8–10 | Duas integrações (faturação PT + calendário) | Fluxo ponta a ponta demonstrado |
| 10–12 | Primeiros 3 clientes pagos + caso de estudo | Receita recorrente > 300 €/mês |

**KPIs a acompanhar:** custo de servir por cliente, margem bruta, tempo até primeira
mensagem respondida pelo agente, taxa de conversão das leads prospetadas, churn mensal,
receita por assento.

---

## 11. O que não foi verificado

- Preços de concorrentes (Kommo, Octadesk, Intercom, Chatwoot): confirmar nas páginas
  oficiais antes de usar em proposta.
- Envio real de WhatsApp e faturação real (nenhum plano criado, nenhuma sessão emparelhada).
- Capacidade da infraestrutura de teste sob carga real (a instância atual é de validação).
- Enquadramento jurídico de revenda e proteção de dados para a UE (requer advogado).
