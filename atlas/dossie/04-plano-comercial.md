# 04 — Plano comercial

> Preços em EUR são **proposta a validar**, não tabela publicada. Antes de usar em
> proposta, confirmar custo de IA, capacidade da instância e preços de concorrentes.

---

## 1. Princípio

Não se vende "um CRM com IA". Vende-se **uma operação comercial que responde em minutos,
não perde leads e mostra o que resulta**.

- O software é o meio. O resultado é: **mais conversas respondidas, menos leads perdidas,
  receita visível**.
- O concorrente barato vende inbox. Nós vendemos **aquisição + conversão + cobrança** num
  só sistema, com os dados do cliente.

---

## 2. As quatro linhas de oferta

| Linha | O que é | Receita | Papel |
|---|---|---|---|
| **A. AtlasDesk** | subscrição, acesso web, 24/7, sem servidor | recorrente | motor do negócio |
| **B. Implementação** | instalação, configuração, migração, formação | one-off | porta de entrada e caixa |
| **C. Pacote vertical** | funil + agentes + automações de um setor | one-off + recorrente | diferenciação |
| **D. Suporte open-source** | quem auto-hospeda paga apoio, atualizações e SLA | recorrente | margem sem infraestrutura |

**Regra comercial:** a implementação nunca é grátis. Pode ser **incluída** num contrato
anual de AtlasDesk (12 meses pagos antecipadamente), mas tem preço visível — senão o
cliente não valoriza e o suporte consome a margem.

---

## 3. Segmentos e o que cada um compra

| Segmento | Tamanho | Dor principal | Oferta | Ciclo |
|---|---|---|---|---|
| **Freelancer / vendedor isolado** | 1 pessoa | perde leads por não responder | AtlasDesk Solo + formação curta | 1–7 dias |
| **Micro / pequena empresa** | 2–10 | WhatsApp caótico, sem seguimento | AtlasDesk Equipa + implementação leve | 1–3 semanas |
| **PME estruturada** | 10–50 | vários canais, sem visão de funil | AtlasDesk Negócio + vertical + formação | 3–6 semanas |
| **Média/grande** | 50–500 | integrações, compliance, multi-equipa | AtlasDesk Enterprise ou dedicado + projeto | 2–4 meses |
| **Consultor / agência** | variável | quer vender aos seus clientes | AtlasDesk Agência (white-label) + revenue share | 2–4 semanas |
| **Setor regulado** | qualquer | dados não podem sair | instalação dedicada + suporte | 1–3 meses |

### Quem decide, por segmento

| Segmento | Decisor | Influenciador | Bloqueador |
|---|---|---|---|
| Freelancer | ele próprio | — | preço |
| Pequena | dono/gerente | quem atende | medo de mudar |
| PME | diretor comercial | IT / marketing | integração |
| Grande | COO / diretor comercial | IT, compras, jurídico | segurança e RGPD |
| Agência | sócio | operação | margem e suporte |

---

## 4. Arquitetura de preço (proposta)

### 4.1 AtlasDesk (mensal, sem fidelização; anual = 2 meses grátis)

| Plano | Preço | Assentos | Números | Agentes | IA incluída | Suporte |
|---|---|---|---|---|---|---|
| **Solo** | 49 € | 1 | 1 | 1 | teto 10 €/mês | email, 48 h |
| **Equipa** | 129 € | 5 | 2 | 3 | teto 30 €/mês | email/chat, 24 h |
| **Negócio** | 299 € | 15 | 5 | 10 | teto 80 €/mês | prioritário, 8 h |
| **Enterprise** | desde 799 € | à medida | à medida | à medida | negociado | SLA 4 h |
| **Agência** | desde 399 € | 5 + clientes | por cliente | por cliente | negociado | parceiro |

Extras: assento adicional 19 €/mês; número adicional 15 €/mês; excesso de IA a custo + 20%.

**Porque limitar a IA:** é o único custo verdadeiramente variável. Um cliente que queima
300 € de IA num plano de 129 € destrói a margem. O produto já tem `teto_ia_usd_cents` por
plano — usar.

### 4.2 Implementação (one-off)

| Pacote | Inclui | Preço |
|---|---|---|
| **Arranque** | instalação, marca, 1 número, 1 agente, formação 2 h | 1.500 € |
| **Operação** | + funil, campanhas, 3 agentes, conhecimento, formação 4 h | 3.500 € |
| **Vertical** | + pacote do setor, integrações locais, 2 semanas acompanhamento | 6.500 € |
| **Enterprise** | análise, migração, SSO, integrações, formação por função | 12.000 €+ |

Formação avulsa: 450 €/sessão (até 8 pessoas). Acompanhamento: 350 €/mês (8 h).

### 4.3 Open-source

| Oferta | Preço |
|---|---|
| Instalação assistida | 900 € |
| Suporte anual (email, atualizações, 1 revisão/trimestre) | 1.200 €/ano |
| Auditoria de segurança/desempenho | 1.500 € |
| Formação de equipa técnica | 900 €/dia |

### 4.4 Pacotes verticais (add-on)

| Setor | O que inclui | Preço |
|---|---|---|
| Clínicas | funil, marcações, lembretes, no-show, consentimento | +1.500 € |
| Imobiliárias | qualificação, visitas, proposta, follow-up longo | +1.500 € |
| Advocacia | triagem, conflito, documento, honorários | +2.000 € |
| Contabilidade | onboarding documental, prazos, campanhas sazonais | +1.500 € |
| E-commerce | recuperação de carrinho, pós-venda, logística | +1.500 € |

---

## 5. como vender a cada segmento

### 5.1 Freelancer / vendedor isolado

- **Entrada:** demonstração de 15 min com o **próprio WhatsApp dele** ligado a uma conta
  de teste.
- **Argumento:** "quantas conversas ficaram sem resposta esta semana?"
- **Oferta:** Solo + formação de 90 min. Sem projeto.
- **Fecho:** cartão, mensal, sem contrato longo.
- **Risco:** churn alto; compensa em volume e baixo suporte.

### 5.2 Pequena empresa

- **Entrada:** diagnóstico de 30 min (quantas leads entram, quem responde, onde se perdem).
- **Demo:** inbox + agente a responder + funil + relatório semanal.
- **Argumento:** "o vosso WhatsApp é o vosso funil; hoje ninguém o vê".
- **Oferta:** Equipa + Arranque.
- **Fecho:** dono decide; evitar comité.

### 5.3 PME estruturada

- **Entrada:** workshop com comercial + operação (mapear etapas reais, não as ideais).
- **Demo:** cenário do setor, com dados de exemplo e um caso real.
- **Argumento:** tempo de primeira resposta, taxa de conversão por etapa, custo por lead.
- **Oferta:** Negócio + Vertical + formação por função.
- **Fecho:** prova de conceito de 30 dias com 3 métricas acordadas.

### 5.4 Média/grande

- **Entrada:** reunião com COO/comercial; questionário de segurança e RGPD em paralelo.
- **Oferta:** Enterprise (multi-tenant com quota) ou instalação dedicada.
- **Peças obrigatórias:** DPA, subprocessadores, SLA, plano de continuidade, formação por
  papel, gestor de conta.
- **Ciclo:** 2–4 meses. Não prometer prazo antes da validação jurídica.
- **Argumento:** custo por conversa vs licenças por utilizador; dados sob controlo;
  possibilidade de marca própria.

### 5.5 Consultor / agência

- **Entrada:** mostrar a **consola de agência** e o white-label, não o inbox.
- **Argumento:** "vende isto aos teus clientes com a tua marca e cobra tu".
- **Oferta:** Agência + revenue share ou preço por cliente.
- **Fecho:** contrato de parceria, formação de 1 dia, primeiro cliente piloto.
- **Cuidado:** definir quem dá suporte ao cliente final (a agência) e quem dá suporte à
  agência (nós).

---

## 6. Perguntas de descoberta (usar sempre)

1. Quantas conversas de WhatsApp entram por dia? Quem responde?
2. Qual é o tempo médio até à primeira resposta? E fora de horas?
3. Onde estão as leads hoje? Planilha, caderno, CRM antigo?
4. Quantas se perdem por falta de seguimento?
5. Qual é o ticket médio e o ciclo de venda?
6. Quem decide e quem usa no dia a dia?
7. Que sistemas têm de falar com isto (faturação, calendário, loja)?
8. Que dados não podem sair da empresa? (RGPD/setor)
9. Já tentaram um CRM? Porque falhou?
10. O que teria de acontecer em 90 dias para considerar um sucesso?

---

## 7. Pitch

### 30 segundos
"Instalamos uma operação de vendas no WhatsApp: um agente de IA responde em segundos,
qualifica e passa a humano quando é preciso. Tudo fica num CRM com funil, campanhas e
relatórios. O cliente não gere servidores — nós gerimos. A partir de 49 €/mês, ou
implementado e formado na vossa empresa."

### 2 minutos (estrutura)
1. **Situação:** "A maioria das PME perde leads porque responde horas depois."
2. **Problema:** "O WhatsApp não é um CRM: não tem funil, não tem seguimento, não mede."
3. **Solução:** "Nós pomos IA a responder, funil a organizar e relatório a mostrar."
4. **Prova:** demonstração ao vivo, com o cenário do setor.
5. **Diferencial:** "Além de atender, encontramos empresas do vosso setor e abordamos."
6. **Oferta:** preço fechado, prazo, o que o cliente tem de fazer.
7. **Próximo passo:** data para arranque ou prova de 30 dias.

---

## 8. Objeções e resposta

| Objeção | Resposta |
|---|---|
| "Já temos WhatsApp" | "E quem responde às 21h? O WhatsApp não tem funil nem relatório — nós mantemos o número e acrescentamos isso." |
| "É caro" | Comparar com o custo de **uma** lead perdida por semana; mostrar plano Solo. |
| "A IA vai dizer disparates" | Mostrar conhecimento, limites, transferência para humano e registo de execuções. |
| "Não quero robôs a falar com clientes" | "O agente qualifica e transfere; o humano entra quando importa." |
| "Os nossos dados são sensíveis" | Instalação dedicada, RLS, cifra, DPA, opção de LLM local. |
| "Já tentámos um CRM e ninguém usou" | Formação por papel, poucos ecrãs, acompanhamento de 30 dias, métrica única. |
| "Podemos instalar de graça" | "Podem. É open-source. Nós vendemos a operação, a configuração e o suporte — e o tempo de quem tenta sozinho custa mais." |
| "E se o WhatsApp bloquear?" | Número dedicado, aquecimento, limites, campanhas segmentadas; cláusula contratual. |
| "Quem trata do RGPD?" | DPA, registo de tratamentos, retenção; o cliente é responsável, nós somos subcontratante. |
| "Podemos mudar de fornecedor?" | Exportação de dados e código MIT; sem lock-in. É argumento de venda, não risco. |

---

## 9. Estrutura de proposta (1–2 páginas)

1. **Contexto** (3 linhas com números do cliente).
2. **Objetivo** (1 métrica mensurável em 90 dias).
3. **Solução** (o que fica a funcionar; o que não está incluído).
4. **Plano e preço** (implementação + mensalidade + extras).
5. **Calendário** (semana 1, 2, 3, 4).
6. **Responsabilidades** (cliente: números, conteúdo, decisões; nós: resto).
7. **Métricas de sucesso** e como são medidas.
8. **Condições** (pagamento, cancelamento, dados, suporte).
9. **Próximo passo** com data.

Nunca enviar proposta sem demo. Nunca enviar preço sem contexto de retorno.

---

## 10. Onboarding 30/60/90

| Fase | Objetivo | Entregas |
|---|---|---|
| **Dias 1–30** | operar | número ligado, agente a responder, funil, equipa formada, 1 métrica |
| **Dias 31–60** | converter | campanhas segmentadas, follow-ups, revisão de qualidade |
| **Dias 61–90** | provar | relatório com antes/depois, ajuste de agente, decisão de renovação |

Reunião de 30 min em cada marco. Sem isto, o churn chega ao mês 3.

---

## 11. Canais e parcerias

| Canal | Esforço | Retorno | Nota |
|---|---|---|---|
| Rede pessoal / referrals | baixo | alto | primeiro motor |
| GitHub + open-source | médio | alto a prazo | prova técnica e inbound |
| LinkedIn (casos e números) | médio | médio | conteúdo de setor |
| Parcerias com agências | médio | alto | escalam vendas |
| Associações empresariais | médio | médio | credibilidade local |
| Marketplaces (HostGator, Coolify) | baixo | médio | intenção técnica |
| Google Ads (termos de CRM WhatsApp) | alto custo | médio | só com página afinada |

**Parceria com contabilistas/consultores:** comissão de 15–20% no primeiro ano, ou preço
de parceiro. São quem conhece as PME e já tem confiança.

---

## 12. Contrato e confiança (o que fecha enterprise)

- DPA + lista de subprocessadores (Supabase, VPS, LLM, Stripe/Asaas, Apify/Scavio).
- SLA escrito: disponibilidade, tempo de resposta, janela de manutenção.
- Política de backup e restauro com evidência.
- Plano de saída: exportação de dados, prazo, formato.
- Propriedade dos dados: do cliente, sempre.
- Marca: AtlasDesk; crédito ao motor open-source, sem sugerir endosso.

---

## 13. Metas e funil (primeiros 6 meses, proposta)

| Métrica | Mês 1–2 | Mês 3–4 | Mês 5–6 |
|---|---|---|---|
| Contactos qualificados/semana | 20 | 40 | 60 |
| Demos/semana | 3 | 6 | 10 |
| Fecho | 1 | 3 | 5 |
| Clientes ativos | 2 | 8 | 15 |
| MRR | 100 € | 600 € | 1.500 € |
| Churn mensal | — | < 8% | < 5% |

Serviços (implementação) devem financiar os primeiros meses; a recorrência é o objetivo.

---

## 14. O que validar antes de publicar preços

- Custo real de IA por 1.000 conversas, com o modelo escolhido.
- Capacidade da instância: quantas organizações/números antes de degradar.
- Preços de Kommo, Octadesk, Intercom e Chatwoot no mercado-alvo.
- Disposição a pagar: 10 entrevistas por segmento antes de fixar tabela.
- Enquadramento fiscal em Portugal (IVA, faturação, retenção) e RGPD.
