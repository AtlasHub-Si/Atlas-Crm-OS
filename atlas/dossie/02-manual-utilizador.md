# 02 — Manual de utilizador (por função)

> Versão de referência: v1.114.0. Caminhos do menu atual.
> Nomes de botões podem mudar entre versões; o **percurso** mantém-se.

---

## 0. Antes de começar

### Entrar
1. Abrir o endereço da organização (ex.: `https://crm.atlashub.digital`).
2. Email + password. Se a organização exigir, confirmar MFA.
3. No canto superior escolher **idioma**, **tema** e **organização** (quem tem mais do que
   uma).

### Papéis e o que cada um pode

| Papel | Pode | Não pode |
|---|---|---|
| `viewer` | Ver conversas, contactos, funis e relatórios | Responder, editar, configurar |
| `agent` | Atender, assumir, transferir, criar contactos/negócios/tarefas | Ver custos, configurar IA, gerir equipa |
| `ai_operator` | Tudo do agente + agentes, conhecimento, skills, roteadores | Gerir cobrança e equipa |
| `manager` | Tudo + campanhas, metas, distribuição, relatórios, importações | Marca da instalação, módulos, cobrança global |
| `admin` | Tudo na organização, incluindo equipa, conexões, faturação | Administração global da instalação (só plataforma) |

Papéis adicionais: **administrador da plataforma** (vê todas as organizações) e **operador
de agência** (gere os clientes da agência, com sessão delegada e auditada).

---

## 1. Atendente (`agent`)

### 1.1 Rotina diária

**Manhã (15 min)**
1. **Inbox** → filtro “não atribuídas”.
2. Ordenar por última mensagem; assumir as conversas da sua carteira.
3. Ver **Radar** para conversas em risco (sem resposta, sentimento negativo, pedido de
   humano).
4. Confirmar **Agenda** do dia e **Tarefas** em atraso.

**Durante o dia**
1. Responder no **Inbox**. Usar **Respostas rápidas** (`/app/templates`) para o que se
   repete.
2. Quando a conversa avança, mover o negócio no **Funil** (`/app/kanban`) e atualizar o
   valor.
3. Registar a próxima ação em **Tarefas** — sem próxima ação, o negócio morre.
4. Passar dados para a ficha: empresa, cargo, origem, etiquetas.

**Fim do dia (10 min)**
1. Nenhuma conversa sua sem resposta.
2. Negócios sem próxima tarefa → criar ou fechar.
3. Transferir para colega o que não é seu (com nota do contexto).

### 1.2 Assumir, transferir, encerrar

| Ação | Quando | Efeito |
|---|---|---|
| Assumir | a conversa é sua | passa a responsável; o agente de IA deixa de responder |
| Transferir | outra pessoa/equipa deve continuar | mantém histórico e passa responsabilidade |
| Convidar colega | precisa de ajuda sem largar a conversa | o responsável não muda |
| Encerrar | assunto resolvido | fecha o atendimento e liberta a fila |
| Devolver à IA | quer que o agente continue | o agente retoma com o contexto |

**Regra de ouro:** nunca encerrar sem registar o resultado (ganho, perdido, sem resposta).

### 1.3 Criar e qualificar um contacto

1. **Contactos → Novo**.
2. Preencher nome, telefone (formato internacional), email, empresa, origem.
3. Se a empresa já existe, associar em vez de duplicar.
4. Qualificar: dor, orçamento, prazo, decisor, próximo passo.
5. Criar negócio no funil certo, com valor e etapa.

### 1.4 Agenda

1. **Agenda** → escolher tipo de agendamento (definido pelo gestor).
2. Escolher responsável, data e hora; confirmar.
3. O cliente recebe confirmação e lembrete (se configurado).
4. Marcar presença/falta depois do compromisso — alimenta o relatório.

### 1.5 O que nunca fazer

- Copiar dados de um cliente para outro.
- Enviar campanha em massa pelo seu número pessoal.
- Prometer preço ou prazo fora da proposta aprovada.
- Apagar contacto ou conversa (não é reversível; pedir ao gestor).

---

## 2. Gestor (`manager`)

### 2.1 Configurar a operação (primeira semana)

1. **Configurações → Organização**: dados fiscais, idioma, moeda, fuso horário.
2. **Configurações → Etapas do funil**: desenhar 5–7 etapas com critério de saída claro.
3. **Configurações → Distribuição de atendimento**: regras de atribuição (rodízio, carga,
   etiqueta, equipa).
4. **Configurações → Tags**: no máximo 15–20 etiquetas úteis.
5. **Configurações → Metas**: objetivo mensal por equipa e por pessoa.
6. **Equipa**: convidar, definir papéis, confirmar aceitação.

### 2.2 Acompanhar o funil

**Semanal**
1. **Funis**: ver negócios parados por etapa (> 7 dias sem atividade).
2. **Desempenho** (`/app/metrics`): entradas, conversão por etapa, tempo médio, receita.
3. **Painel de atendimento** (`/app/painel`): volume, tempo de primeira resposta,
   conversas sem responsável.
4. **Atividades**: quem fez o quê; base para coaching.

**Mensal**
1. Comparar meta vs realizado.
2. Identificar a etapa que mais perde (normalmente proposta ou follow-up).
3. Rever scripts e respostas rápidas com o que funcionou.

### 2.3 Campanhas e cadências

1. **Campanhas → Nova**: escolher público, mensagem, número remetente e ritmo.
2. Rever a **prévia** e a lista de supressão (quem não deve receber).
3. Começar com volume baixo (aquecimento) e aumentar.
4. Acompanhar entrega, resposta e bloqueios.
5. **Cadências de e-mail** para sequências longas; **Follow-ups** da IA para WhatsApp.

**Regra:** campanha sem segmento é spam. Segmentar por origem, etapa ou etiqueta.

### 2.4 Prospecção

1. **Prospecção → Encontrar empresas**: segmento + localização (Apify/Scavio) **ou**
   **Usar minha planilha** (até 500 linhas, modelo disponível).
2. Avaliar site e redes; verificar WhatsApp (se o módulo estiver ligado).
3. Gerar prévia da abordagem e rever uma a uma.
4. Criar campanha, escolher número e ritmo, iniciar.
5. Medir: entregues, respostas, reuniões, clientes.

### 2.5 Qualidade

- Amostrar 10 conversas por semana por atendente.
- Verificar: saudação, diagnóstico, proposta, próximo passo, tom.
- Registar desvios e formar — não corrigir em público no chat.

---

## 3. Operador de IA (`ai_operator`)

### 3.1 Criar um agente

1. **IA → Agentes → Novo**.
2. Definir: nome, função, idioma, tom, limites (o que não faz), escalonamento.
3. Escolher modelo e credencial (ver 3.4).
4. Associar **skills** e **conhecimento**.
5. Testar em **IA → Atendimento** com 10 perguntas reais, incluindo as difíceis.
6. Publicar e ligar a um número/canal.

### 3.2 Conhecimento (RAG)

1. **IA → Conhecimento → Fontes**.
2. Carregar documentos curtos e atualizados: preços, horários, políticas, FAQ.
3. Evitar PDFs gigantes; dividir por tema.
4. Testar perguntas cuja resposta está no documento.
5. Atualizar sempre que o negócio mudar — conhecimento velho é pior que nenhum.

### 3.3 Skills, roteadores e follow-ups

| Ecrã | Para quê |
|---|---|
| **Skills** | capacidades reutilizáveis (marcar, orçamentar, consultar estado) |
| **Roteadores** | escolher o agente certo conforme intenção/etiqueta |
| **Follow-ups** | retomar conversas paradas com regras e limites |
| **Memória** | o que o agente retém por contacto |
| **Casos** | situações que exigem humano, com alerta |
| **Execuções** | depurar o que o agente fez e porquê |

### 3.4 Credenciais e custo

1. **IA → Credenciais**: chave do fornecedor (cifrada).
2. **IA → Provedores**: modelo por tarefa (barato para triagem, forte para venda).
3. **IA → Uso e orçamento**: definir teto mensal e acompanhar.
4. **IA → Evolução**: ver lacunas de conhecimento e falhas recorrentes.

**Nunca** colar chaves em chat, ticket ou documento partilhado.

### 3.5 Governança

- O agente **não** promete preço fora da tabela.
- O agente **transfere** em: pedido explícito, reclamação, negociação, tema jurídico.
- Toda a transferência tem motivo registado.
- Rever semanalmente os casos humanos para melhorar o conhecimento.

---

## 4. Administrador (`admin`)

### 4.1 Equipa e acessos

1. **Equipa → Convidar**: email + papel.
2. Confirmar aceitação; remover acessos de quem sai no mesmo dia.
3. Ativar MFA para administradores.
4. Rever papéis trimestralmente (menor privilégio).

### 4.2 Canais e integrações

1. **Conexões**: WhatsApp (QR), email, calendário, e-commerce.
2. **Webhooks**: eventos para sistemas externos.
3. **Integrações**: Nuvemshop, Meta Ads, banco externo, etc.
4. Testar cada integração com um caso real antes de anunciar.

### 4.3 Faturação e cobrança

1. **Configurações → Financeiro**: métodos, planos de contas, moeda.
2. **Faturamento**: emitir e acompanhar documentos internos.
3. **Cobrança** (se ligada): planos dos clientes, assinaturas, faturas, atrasos.
4. **Honorários**: comissões e repasses.

> O produto não emite fatura certificada portuguesa. Para Portugal, ligar a Moloni,
> InvoiceXpress ou Vendus (ver documento 00 §8).

### 4.4 Recursos opcionais e marca

1. **Configurações → Recursos opcionais**: ligar apenas o que o cliente usa.
2. Cada módulo desligado esconde menu, rotas e permissões — reduz erro do utilizador.
3. **Admin → Marca** (plataforma): nome, cor, logo, ícone, CSS.
4. Confirmar contraste e legibilidade nos dois temas.

### 4.5 Auditoria e segurança

- **Audit Log**: quem viu/alterou o quê.
- Exportar antes de qualquer alteração grande.
- Revisar acessos de agência e delegações.
- Plano de resposta a incidente: quem contacta o cliente, em quanto tempo.

---

## 5. Freelancer / empresa de uma pessoa

Faz tudo, mas na ordem certa:

1. **Configurações** mínimas (organização, funil de 4 etapas, tags).
2. **1 agente de IA** com conhecimento essencial.
3. **1 número** de WhatsApp dedicado.
4. **Prospecção**: 20–30 empresas por semana, abordagem personalizada.
5. **Inbox** de manhã e ao fim do dia; tarefas sempre com próxima ação.
6. **Desempenho** uma vez por semana: quantas conversas, quantas propostas, quantos ganhos.
7. Só depois ligar campanhas e automações.

---

## 6. Referência rápida de ecrãs

| Ecrã | Caminho | Serve para |
|---|---|---|
| Inbox | `/app/inbox` | conversas de WhatsApp |
| Funis | `/app/kanban` | negócios por etapa |
| Contactos / Empresas / Pessoas | `/app/contacts`, `/app/companies`, `/app/people` | fichas e decisores |
| Tarefas | `/app/tasks` | próximas ações |
| Agenda | `/app/agenda` | marcações e lista de espera |
| Campanhas | `/app/campaigns` | envio em massa |
| Prospecção | `/app/prospecting` | encontrar e abordar empresas |
| Radar | `/app/radar` | risco de perda |
| Painel de atendimento | `/app/painel` | carga e tempos |
| Desempenho | `/app/metrics` | resultados e metas |
| Agentes | `/app/ai/agents` | configuração da IA |
| Conhecimento | `/app/ai/knowledge/sources` | base de verdade |
| Uso e orçamento | `/app/ai/usage` | custo de IA |
| Conexões | `/app/connections` | WhatsApp e canais |
| Equipa | `/app/team` | pessoas e papéis |
| Organização | `/app/settings/tenant` | configuração do cliente |
| Audit Log | `/app/audit` | rasto de alterações |

---

## 7. Erros comuns e correção

| Erro | Consequência | Correção |
|---|---|---|
| Duplicar contactos | histórico dividido | procurar antes de criar; usar telefone como chave |
| Negócio sem próxima tarefa | esquece-se | tarefa obrigatória ao mover etapa |
| Agente sem conhecimento | inventa respostas | carregar FAQ e testar |
| Campanha para toda a base | bloqueios e spam | segmentar e excluir inativos |
| Encerrar sem resultado | relatórios falsos | exigir desfecho |
| Partilhar número entre clientes | contaminação e ban | um número por organização |
| Ignorar handoff | cliente repetido | alerta e fila de casos |

---

## 8. Limites do manual

- Não documenta APIs (ver `docs/specs/` no repositório).
- Não substitui formação prática: o produto muda entre versões.
- Funcionalidades de módulos opcionais só existem se estiverem ligados na instalação.
