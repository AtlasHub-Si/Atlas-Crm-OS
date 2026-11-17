# 05 — Análise da versão integrada no GitHub (`Atlas-Crm-OS`)

> Medição de 17/11/2026. Repositório público, acesso confirmado.
> Conclusão curta: **o fork existe, mas não tem código nosso — só três ficheiros de
> identidade legal e proveniência.**
> **Atualização (17/11/2026, 07:09 UTC): fork sincronizado com o upstream — 0 commits atrás.**

---

## 0. Estado final da sincronização (17/11/2026 07:09 UTC)

| Métrica | Antes | Depois |
|---|---|---|
| Commits à frente | 1 | **2** |
| Commits atrás | 1.198 | **0** |
| Total de commits | 17.620 | 18.819 |
| Merge commit | — | `e0d43d53` — "Merge branch 'melgarafael:main' into main" |
| Executado por | — | `atlashub-digital` (botão **Sync fork → Update branch** no GitHub) |
| Diff face ao upstream | 3 ficheiros, +37 linhas | **3 ficheiros, +37 linhas** (inalterado) |
| `ATLAS.md` / `LEGAL.md` | presentes | **presentes (HTTP 200)** |
| Conflitos | — | **nenhum** |

O cabeçalho Atlas do `README.md` (5 linhas) ficou **por cima** do README upstream, sem
perda de conteúdo: o diff face ao upstream continua a ser exatamente o mesmo de antes.
O commit Atlas `29d4b3e2` foi preservado no histórico.

Resultado: o fork é agora um **fork de acompanhamento** a sério — recebe o upstream e
mantém apenas a camada de identidade/proveniência. As 1.198 atualizações trazidas incluem
correções de WhatsApp/WAHA, baseline do Supabase, telefonia SIP e módulos.

**Próxima sincronização:** repetir o botão (ou `git fetch upstream && git merge`) quando o
aviso voltar a mostrar commits atrás; registar aqui data e SHA.

---

## 1. Onde está e como foi encontrado

| Item | Valor |
|---|---|
| Repositório | `AtlasHub-Si/Atlas-Crm-OS` (público) |
| Fork de | `melgarafael/DeskcommCRM` (`main`) |
| Organização acessível | `AtlasHub-Si` (7 repositórios) |
| Conta autenticada no GitHub | `atlashub-digital` (29 repositórios; **nenhum** Deskcomm) |
| Ramo principal | `main` |
| Ramos existentes | `main`, `docs/atlas-legal-provenance-v1-20261113` |
| Último commit | `29d4b3e2` — 13/11/2026 19:31 UTC |
| Autor | Sergio Monteiro `<sergiomonteiro@atlashub.si>` |
| Commit base | `d4c1cbd5` — 12/11/2026 (upstream) |

O repositório não aparece por nome porque está na organização **`AtlasHub-Si`**, não em
`atlashub-digital`. Foi localizado pela lista de forks do projeto original.

---

## 2. O que foi realmente acrescentado

Comparação `base d4c1cbd5 → HEAD`: **1 commit, 3 ficheiros, +37 linhas, 0 remoções.**

| Ficheiro | Estado | Linhas | Conteúdo |
|---|---|---|---|
| `ATLAS.md` | novo | +20 | propósito, estado, proveniência, licença MIT, contactos |
| `LEGAL.md` | novo | +12 | entidades legais UK (Atlas Global Tech Ltd, ATLASHUB DIGITAL LTD), contactos |
| `README.md` | alterado | +5 | cabeçalho a declarar "Atlas integration track" |

### O que dizem, em substância

- `ATLAS.md`: "Atlas CRM/Workforce-OS business-unit candidate, derived from DeskcommCRM";
  estado "business application in evaluation"; não é runtime do Atlas Harness nem SKU
  comercial em produção; licença MIT mantida; crédito ao autor original.
- `LEGAL.md`: identifica as empresas do grupo e contactos; declara explicitamente que as
  identidades **não transferem copyright** do software original.
- `README.md`: acrescenta um aviso de que é uma integração Atlas, não uma alegação de
  autoria.

---

## 3. O que NÃO foi acrescentado (e era expectável)

| Área | Estado no fork |
|---|---|
| Código de aplicação (`app/`, `lib/`, `components/`, `workers/`) | **Idêntico ao upstream** |
| Marca / white-label (nome, cor, logo, CSS) | **Nada** |
| Captura de leads / prospecção / integração LeadFlow | **Nada** |
| Idiomas (PT-PT, EN, FR) | **Nada** |
| Faturação / planos / EUR | **Nada** |
| Módulos verticais | **Nada** |
| Migrações de base de dados próprias | **Nada** |
| CI/CD próprio, deploy, infra | **Nada** |
| Testes novos | **Nada** |

Ou seja: o que existe no GitHub é um **selo de proveniência**, não uma versão integrada
do produto. A integração funcional que fizemos (WAHA, LeadFlow, Supabase `Atlas_CRM`)
vive **localmente** em `/home/nex/OpenBot/Shared/`, não foi publicada no GitHub.

---

## 4. Divergência face ao upstream (medição anterior à sincronização — histórico)

| Métrica | Valor |
|---|---|
| Commits à frente | **1** |
| Commits atrás | **1.198** |
| Versão local em produção de teste | v1.114.0 (mais recente que o fork) |
| Ficheiros diferentes do upstream atual | 1.686 (quase todos por atraso, não por edição nossa) |

O upstream tem uma cadência muito alta (mais de mil commits em poucos dias). Um fork
parado 1.198 commits perde correções de segurança, compatibilidade com WhatsApp e
migrações. **Não serve como base de produto sem um processo de sincronização.**

---

## 5. Leitura estratégica

1. **O fork não é um ativo técnico.** É documentação. Não há nada lá que possamos
   "aproveitar" além do enquadramento legal — que, aliás, está correto e deve manter-se.
2. **A licença MIT obriga a manter o `LICENSE` e os avisos de copyright.** O `LEGAL.md`
   atual já faz a ressalva certa; não remover nem reescrever o README original de forma a
   apagar o autor.
3. **O nome `Atlas-Crm-OS` pode ficar** como nome de repositório interno, mas o produto
   comercial é **AtlasDesk** (provisório). Não confundir os dois.
4. **A versão que interessa está à frente**, não atrás: a instância local em v1.114.0 já
   inclui 1.198 commits que o fork não tem.

---

## 6. Recomendação operacional

> **Estado: passo 1 executado.** O modelo escolhido foi **fork de acompanhamento** e a
> primeira sincronização já foi feita (ver secção 0). Falta criar o `UPSTREAM.md` e
> automatizar a verificação semanal.

### Decidir o modelo de fork (uma vez)

| Modelo | Quando usar | Como |
|---|---|---|
| **Fork de acompanhamento** | queremos receber o upstream e manter só tema/marca | `upstream` remoto + merge semanal; alterações nossas isoladas em `atlas/` |
| **Fork de produto** | queremos divergir muito (vertical, PT-PT, faturação) | congelar um tag, aplicar patches, sincronizar por cherry-pick trimestral |

Recomendação: **fork de acompanhamento agora**, porque o upstream corrige WhatsApp e
segurança a um ritmo que não conseguimos replicar. Divergir só quando houver código nosso
a sério.

### Passos concretos

1. Criar `UPSTREAM.md` com: repositório de origem, SHA sincronizado, data, política de
   sincronização e responsável.
2. Adicionar o remoto e automatizar verificação semanal:
   ```bash
   git remote add upstream https://github.com/melgarafael/DeskcommCRM.git
   git fetch upstream main
   git log --oneline HEAD..upstream/main | wc -l
   ```
3. Fazer o rebrand numa pasta/camada própria (`config/atlas/`, tema, textos), não em
   centenas de ficheiros espalhados — para o merge não doer.
4. Manter `ATLAS.md`, `LEGAL.md` e o cabeçalho do README; acrescentar `NOTICE` se
   publicarmos binários.
5. Publicar a integração real (WAHA + LeadFlow + Supabase) só depois de decidir se é
   código de produto ou scripts de operação. Hoje são scripts locais, não produto.

---

## 7. O que não foi possível verificar

- Repositórios **privados** da organização que a conta autenticada não veja.
- Ramos apagados ou trabalho que tenha existido apenas noutra máquina.
- Se a instância local v1.114.0 tem alterações não commitadas (o clone é `--depth 1` do
  upstream; não há remoto nosso).
- Histórico de conversas anterior a este chat sobre a integração.

Se existir outro repositório ou uma VPS com a versão integrada, indicar o nome ou o
endereço: faço a mesma comparação contra o upstream e contra a instância local.
