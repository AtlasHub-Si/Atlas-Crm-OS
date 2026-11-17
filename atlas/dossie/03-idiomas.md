# 03 — Idiomas: estado real e plano de adaptação

> Medido em 17/11/2026 no código local (v1.114.0). Nada aqui é estimativa sobre o que
> "deveria existir"; os números vêm dos ficheiros.

---

## 1. O que existe hoje

| Idioma | Código | Estado no registo | Catálogo | Cobertura |
|---|---|---|---|---|
| Português (Brasil) | `pt-BR` | **completo** (visível) | embutido no código | 100% (12.705 chaves) |
| Espanhol | `es` | **completo** (visível) | coluna `es` no dicionário | 100% (guarda automática) |
| Inglês | `en` | **em construção** (invisível) | `traducoes/en.json` | 9.843 / 12.705 ≈ **77%** |
| Turco | `tr` | **em construção** (invisível) | `traducoes/tr.json` | 11.465 / 12.705 ≈ **90%** |
| Chinês simplificado | `zh-CN` | **em construção** (invisível) | `traducoes/zh-CN.json` | 6.051 / 12.705 ≈ **48%** |
| Francês | `fr` | **não existe** | — | 0% |

**Como o sistema escolhe o idioma:** preferência da pessoa → idioma da organização →
padrão `pt-BR`. O navegador só decide quando não há preferência guardada.

**Arquitetura:** a chave do dicionário é a própria frase em português. Falta de tradução
mostra português (nunca uma chave técnica). O idioma **não** vai no caminho do URL, o que
evita partir links, webhooks e rotas.

**Guarda de qualidade:** `tests/unit/i18n-espanhol-cobre-a-tela` varre o AST de todas as
telas e reprova (a) prosa portuguesa fora de `t()` e (b) chave usada sem tradução. Um
idioma "completo" não pode ficar para trás sem o CI avisar.

---

## 2. Achados que importam para o negócio

### 2.1 Inglês está feito a 77% e invisível

O catálogo inglês tem **9.843 frases**. Está escondido por uma linha:
`nivel: "em_construcao"` em `lib/i18n/registro.ts`. Promover é mudar o nível para
`completo`, mas isso **obriga** a fechar as ~2.860 frases em falta, porque a guarda passa a
exigir 100%.

- **Custo baixo, retorno alto:** é o idioma que abre mercado internacional e canais
  (GitHub, Product Hunt, diretórios SaaS).
- **Prazo estimado:** 1–2 semanas com tradução assistida + revisão humana.

### 2.2 Português de Portugal não existe como variante

`pt-PT` cai em `pt-BR` (a subtag `pt` é comum). O utilizador português vê:

| Vê (pt-BR) | Espera (pt-PT) |
|---|---|
| tela | ecrã |
| contato | contacto |
| nota fiscal | fatura |
| CNPJ / CPF | NIF |
| celular | telemóvel |
| arquivo | ficheiro |
| planejamento | planeamento |

Isto não é cosmético: para vender em Portugal, "nota fiscal" e "CNPJ" gritam "produto
brasileiro". Duas opções:

1. **Catálogo `pt-PT`** com ~1.500–2.500 substituições lexicais (não é tradução, é
   variante). Recomendado.
2. **Dicionário de substituição** (mesma chave, valor pt-PT) — mais rápido, menos limpo.

Além do texto: campos fiscais, validação de NIF, moeda e formato de telefone.

### 2.3 Francês parte de zero

São **12.705 chaves**. O repositório já tem ferramentas para traduzir em lotes:

```bash
pnpm tsx scripts/i18n/lotes-para-traduzir.ts --idioma fr --tamanho 400 --saida <pasta>
pnpm tsx scripts/i18n/juntar-lotes.ts --idioma fr --entrada <pasta>
```

E um guia de estilo por idioma (existe para o turco em `docs/i18n/tr-sozluk.md`). Para o
francês é preciso criar o equivalente: glossário + regras (vouvoiement, buttons no
imperativo, "IA" → "IA", etc.).

**Estimativa (proposta):**

| Via | Custo | Prazo | Qualidade |
|---|---|---|---|
| Máquina + revisão | 1.500–2.500 € | 2–3 semanas | Boa para UI |
| Tradutor profissional | 4.000–7.000 € | 4–6 semanas | Alta |
| Misto (máquina em telas secundárias) | 2.000–3.500 € | 3 semanas | Boa |

Depois: registar `fr`, guarda de cobertura, revisão de contexto (não traduzir marca,
placeholders intactos) e teste com um falante nativo.

### 2.4 Espanhol está completo, mas é espanhol neutro

Bom para América Latina e Espanha. Rever termos comerciais por país (factura/factura,
ordenador/Computadora) só se o volume justificar.

---

## 3. O que não é texto e também precisa de adaptação

| Item | Estado | Ação para PT/FR |
|---|---|---|
| Moeda | default BRL; cobrança aceita EUR/USD | definir EUR por organização |
| Campos fiscais | CNPJ/CPF, BrasilAPI | NIF/VAT, validação e serviços locais |
| Telefone | assume formato brasileiro em vários fluxos | validar E.164 e prefixos locais |
| LGPD | vocabulário e fluxos brasileiros | GDPR: bases legais, DPA, retenção, direitos |
| Documentos | "nota fiscal", "boleto", "Pix" | "fatura", "referência MB", "transferência" |
| Templates WhatsApp | pt-BR | revisão cultural e legal |
| Datas | `date-fns` com `ptBR` e `es` | acrescentar locale `fr` (e `pt`) |
| Emails | templates em pt-BR | traduzir com o mesmo catálogo |
| Ajuda/curso | português | legenda ou versão por idioma |

Nota técnica: o locale de data é um mapa exaustivo por idioma (`lib/i18n/datas.ts`). O
compilador obriga a completá-lo ao promover um idioma — é uma proteção, não um obstáculo.

---

## 4. Ordem recomendada

1. **PT-PT** (mercado onde estás, esforço médio, impacto imediato na credibilidade).
2. **EN** (já está a 77%; abre distribuição internacional).
3. **FR** (novo mercado, custo mais alto; só depois de EN validado).
4. **ES** já está pronto: usar como mercados de teste sem custo.

**Regra de negócio:** não vender um idioma que não está 100%. Um ecrã meio traduzido
destrói mais confiança do que um produto assumidamente em português.

---

## 5. Critérios de aceitação por idioma

- [ ] 100% das chaves do dicionário cobertas (guarda CI verde).
- [ ] Nenhuma prosa portuguesa fora de `t()` nas telas.
- [ ] Placeholders `{nome}` / `{{nome}}` intactos.
- [ ] Datas, números e moeda no formato local.
- [ ] Emails transacionais traduzidos.
- [ ] Revisão por falante nativo (mínimo: 20 ecrãs críticos).
- [ ] Documentação de ajuda traduzida ou explicitamente em inglês.
- [ ] Teste ponta a ponta: registo → conversa → cobrança, no idioma.

---

## 6. O que não foi verificado

- Qualidade real das 9.843 frases inglesas (não foram revistas por nativo).
- Se a guarda de i18n passa neste momento (as dependências não estão instaladas no
  repositório local; correr `pnpm test:unit i18n` num ambiente com `node_modules`).
- Cobertura de tradução dos templates de email e das páginas públicas.
- Terminologia jurídica GDPR em cada país.
