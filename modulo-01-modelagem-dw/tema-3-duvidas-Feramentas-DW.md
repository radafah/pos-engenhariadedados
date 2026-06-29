# Dúvidas e Respostas — Ferramentas e Tecnologias para Data Warehouse

> Registro das dúvidas originais da aula e dos desdobramentos que surgiram durante o estudo.

---

## 1. O que é o KDD? É uma técnica, conceito ou área?

**Dúvida original:** O que exatamente é o KDD? Como ele funciona? É uma técnica, conceito ou área?

**Resposta:**

KDD (Knowledge Discovery in Databases) é uma **área** — um campo de estudo e prática cujo objetivo é transformar dados brutos em conhecimento útil. Não é uma técnica isolada, mas um processo completo que envolve cinco etapas sequenciais:

```
1. Seleção           → escolher quais dados são relevantes para o problema
2. Pré-processamento → limpar, tratar inconsistências e valores ausentes
3. Transformação     → formatar os dados para que os algoritmos consigam trabalhar
4. Data Mining       → aplicar os algoritmos para encontrar padrões
5. Interpretação     → analisar o que foi encontrado e transformar em conhecimento útil
```

O Data Mining é a **etapa 4** dentro desse processo. O KDD é o processo completo — do dado bruto até o conhecimento interpretado e aplicável.

> Analogia: o KDD é o processo de garimpar ouro. O Data Mining é o momento em que você passa a bateia na água. Mas antes você teve que escolher o rio certo, preparar o equipamento, e depois ainda precisa avaliar o que encontrou.

---

## 2. Como aplicamos o Data Mining no dia a dia? É uma técnica ou só é possível usando uma ferramenta?

**Dúvida original:** Como aplicamos o Data Mining no dia a dia? É uma técnica ou somente é possível usando uma ferramenta? Eu que faço a pergunta ou analiso o que retornar?

**Resposta:**

Os dois — o Data Mining é uma **técnica** (conjunto de algoritmos matemáticos/estatísticos que identificam padrões nos dados), e as ferramentas como RapidMiner e SAS Enterprise Miner facilitam a aplicação dessas técnicas. A ferramenta não substitui o analista — ela faz o trabalho pesado do meio.

O processo na prática funciona assim:

1. **Você define o problema** — "quero entender por que clientes cancelam" ou "quero identificar transações suspeitas"
2. **A ferramenta aplica os algoritmos** — clustering, classificação, regressão, associação, etc.
3. **A ferramenta retorna os padrões encontrados** — grupos de comportamento, regras, correlações
4. **Você interpreta e decide** — o que é relevante, o que é ruído, o que vira ação

Sim — você faz a pergunta, a ferramenta processa, e você analisa o que retornou para extrair o que é aproveitável. O analista/engenheiro continua sendo essencial na etapa 1 e na etapa 4.

**Exemplos práticos:**

- **E-commerce:** "clientes que compraram X também compram Y" — recomendação de produtos
- **Banco:** identificar padrões de fraude em transações antes de acontecerem
- **RH:** prever quais colaboradores têm maior risco de desligamento
- **Varejo:** descobrir que cerveja e fraldas são compradas juntas às sextas — e reorganizar a loja

No contexto corporativo com DW: você já tem os dados históricos estruturados — o Data Mining entra para extrair inteligência além do que os relatórios tradicionais mostram.

---

## 3. O Tableau trabalha com tempo real? É uma vantagem sobre o Power BI?

**Dúvida original:** O Tableau trabalha com tempo real? É uma vantagem encima do Power BI?

**Resposta:**

O Tableau tem suporte a dados em tempo real via conexão live. O Power BI também tem essa capacidade, mas com limitações importantes na prática:

- **Atualização agendada:** mínimo de 30 minutos no serviço padrão, com até 48 atualizações por dia no Pro
- **DirectQuery:** consulta ao vivo, mas extremamente lento dependendo do conector e do volume de dados
- **Streaming dataset:** funciona para tempo real, mas é limitado em termos de modelagem e visuais disponíveis

O Tableau tem uma arquitetura de conexão live mais madura historicamente, o que gerou a percepção de ser "melhor em tempo real". Mas na prática corporativa, especialmente no ecossistema Azure, Power BI com dados bem modelados no DW e atualização incremental resolve a grande maioria dos casos sem precisar de tempo real verdadeiro.

**Tempo real de verdade** no mundo de dados geralmente exige uma stack específica — Kafka + processamento em stream + dashboard conectado diretamente. Power BI e Tableau são ferramentas de BI, não de streaming.

| | Tableau | Power BI |
|---|---|---|
| Tempo real | Sim, arquitetura live mais madura | Sim, com limitações por conector |
| Integração Microsoft | Limitada | Nativa (Azure, Excel, Teams) |
| Curva de aprendizado | Maior | Menor |
| Custo | Mais caro | Mais acessível |
| Mercado Brasil | Presente | Dominante |

---

## 4. Definir os conceitos de ETL, OLAP, Data Mining e Visualização de Dados

**Dúvida original:** Definir melhor o conceito de: ETL, OLAP, Data Mining e Ferramentas de Visualização de Dados.

**Resposta:**

**ETL (Extract, Transform, Load):**
Processo de mover e preparar dados entre sistemas.
- **Extract:** coleta dos dados nas fontes de origem
- **Transform:** limpeza, padronização e enriquecimento
- **Load:** carga no DW — pode ser full (tudo do zero) ou incremental (só o que mudou)

**OLAP (Online Analytical Processing):**
Tecnologia para consulta analítica estruturada. Você faz perguntas que já sabe — "qual foi a venda por região no último trimestre?" — e o OLAP responde rápido cruzando dimensões. Ferramentas: Microsoft SQL Server Analysis Services, SAP BW, Oracle OLAP.

**Data Mining:**
Vai além do OLAP. Você não sabe exatamente o que vai encontrar — os algoritmos descobrem padrões, correlações e comportamentos que não seriam óbvios numa análise tradicional. É a etapa central do processo KDD.

**Visualização de Dados:**
Camada final que transforma números em informação compreensível — gráficos, dashboards, mapas — para apoiar a tomada de decisão. Ferramentas: Power BI, Tableau, Google Data Studio.

| Conceito | Foco | Tipo de análise |
|---|---|---|
| OLAP | Consulta estruturada | Responde perguntas definidas |
| Data Mining | Descoberta de padrões | Exploratória — descobre o que não é óbvio |
| Visualização | Comunicação dos dados | Transforma resultado em informação visual |

---

## 5. O que é uma visão holística de negócio?

**Dúvida original:** O que é uma visão holística de negócio?

**Resposta:**

É a capacidade de enxergar a empresa como um **todo integrado**, e não como departamentos isolados. Em vez de analisar só as vendas ou só o financeiro, você cruza os dados de todas as áreas para entender como uma decisão em um setor impacta os outros.

No contexto de dados, o DW viabiliza essa visão — porque centraliza informações de múltiplas fontes (ERP, CRM, operações) em um único lugar. Um dashboard holístico pode mostrar ao mesmo tempo receita, custo operacional, satisfação do cliente e performance de estoque — e as relações entre eles.

> É o oposto de tomar decisão olhando para um único número sem contexto.

---
