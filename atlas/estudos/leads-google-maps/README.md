# Estudo — captação de leads por Google Maps

> **Objetivo:** avaliar se conseguimos alimentar o funil do AtlasDesk com leads locais
> (PME, clínicas, escritórios, lojas) sem depender de plataformas pagas nem de API do
> Google Maps.
>
> **Estado:** protótipo funcional validado com um caso real (dentistas em Lisboa).
> Não está integrado no CRM; é um estudo de desenvolvimento.

---

## 1. Resposta curta

Funciona. Um comando devolve uma lista de empresas com nome, telefone, site, categoria,
morada, avaliação e nº de reviews. No teste, **100 empresas em poucos minutos**, sem API
key e sem custo por registo.

O que **não** sai pronto: emails. O enriquecimento de email é um passo separado (o motor
lê a homepage de cada empresa) e foi corrido apenas na amostra de 6.

---

## 2. Como foi feito

Componente de terceiros, não nosso:

| Item | Valor |
|---|---|
| Projeto | `Mahanaicoach/google-maps-scraper-kit` — MIT |
| Versão usada | clone do commit `446f231` (13/11/2026) |
| Onde está localmente | `/home/nex/OpenBot/Shared/google-maps-scraper-kit` |
| Como corre | motor próprio local (Python) ou Docker; sem API key |
| Créditos e licença | ver [`NOTICE-terceiros.md`](NOTICE-terceiros.md) |

O kit **não foi copiado para este repositório**. Só os dados de teste e este relatório
entram aqui, para não arrastar 11 MB de código de terceiros para dentro do fork nem
duplicar a manutenção de um projeto que não é nosso.

### Fluxo

```text
pesquisa ("dentistas em Lisboa")
        ↓
motor local (engine) → Google Maps
        ↓
CSV por tarefa em .engine/data/<uuid>.csv
        ↓
limpeza de colunas → CSV de trabalho
        ↓
(enriquecimento opcional: emails a partir do site)
        ↓
importação para o CRM: contacto + empresa + origem "maps"
```

---

## 3. Dados deste teste

| Ficheiro | Linhas | Conteúdo |
|---|---|---|
| [`dados/dentistas-lisboa-100.csv`](dados/dentistas-lisboa-100.csv) | 100 | nome, categoria, telefone, site, morada, avaliação, nº reviews, place_id |
| [`dados/amostra-dentistas-lisboa-6-com-emails.csv`](dados/amostra-dentistas-lisboa-6-com-emails.csv) | 6 | o mesmo + emails, Instagram, Facebook, LinkedIn, YouTube |

Cobertura medida na lista de 100: **98 com telefone**, **83 com site**, **0 com email**.
Os 6 da amostra têm email porque passaram pelo passo de enriquecimento.

Reproduzir com as mesmas pesquisas: [`queries.example.json`](queries.example.json).

---

## 4. O que isto vale para o AtlasDesk

1. **Entrada de funil barata.** Cada cliente que compra implementação recebe uma lista
   inicial do seu nicho e cidade — trabalho de horas, não de semanas.
2. **Prova de valor antes de vender.** "Já tenho 100 clínicas na sua zona com telefone;
   quer que eu ligue?" vende melhor do que uma demo de software.
3. **Segmentação por dados.** Rating e nº de reviews indicam quem tem volume e quem está
   a crescer — bom para priorizar prospeção.
4. **Diferencial de produto.** Um importador "Google Maps → funil" dentro do CRM é uma
   funcionalidade concreta que a maioria dos CRMs open-source não tem.

### Como integraria (proposta, não implementado)

- Script de importação que lê o CSV e cria `empresa` + `contato` com
  `origem = 'google_maps'`, telefone normalizado em E.164.
- Deduplicação por `place_id` e por telefone.
- Campo de consentimento por contacto, obrigatório antes de qualquer mensagem.
- Fluxo de nutrição separado do funil de clientes ativos.

---

## 5. Limites e riscos — ler antes de usar a sério

| Risco | Detalhe |
|---|---|
| **RGPD** | Telefone e email de empresas identificáveis são dados pessoais quando ligados a pessoa singular. Base legal e informação ao titular são obrigatórias antes de contacto comercial. A lista não é "livre" por vir do Google. |
| **Termos do Google** | A recolha automatizada de resultados do Maps contraria os termos de serviço do Google. Risco: bloqueio de IP, e não é uma base contractual para um produto vendido a clientes. |
| **Qualidade** | Sem enriquecimento, falta email em 100% dos registos. Telefones fixos e sites desatualizados são frequentes. |
| **Escala** | Uma lista de 100 é trivial; milhares exigem proxies e cuidado com rate limits. |
| **Não é fonte única** | Serve para arranque. Um CRM vive de leads inbound e de referências, não de listas compradas ou recolhidas. |

**Recomendação:** usar como ferramenta interna de arranque comercial e como
funcionalidade opcional do produto, com aviso legal explícito ao cliente sobre a origem
dos dados e a responsabilidade pelo contacto.

---

## 6. Estado

- [x] Protótipo validado num nicho e numa cidade
- [x] Dados de teste guardados neste repositório
- [ ] Enriquecimento de email em lote
- [ ] Importador CSV → CRM (com deduplicação e consentimento)
- [ ] Decisão: ferramenta interna ou funcionalidade do produto
