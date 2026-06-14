# Dúvidas e Respostas — Modelagem e Arquitetura de Data Warehouse

> Registro das dúvidas originais da aula e dos desdobramentos que surgiram durante o estudo.

---

## 1. OLTP e OLAP — Como funcionam e quando cada um ocorre?

**Dúvida original:** O OLAP surge em que momento? Ele é uma réplica do banco OLTP com limitação/definição de data, ou é outra coisa? Qual o momento do OLAP e qual o momento do DW?

**Resposta:**

OLTP e OLAP não são bancos diferentes — são **modos de uso**.

- **OLTP (Online Transaction Processing):** modo transacional. Toda operação do dia a dia da empresa — venda registrada, estoque atualizado, pedido criado. Otimizado para escrever rápido e ler pouco.
- **OLAP (Online Analytical Processing):** modo analítico. Consultas que agregam, comparam, cruzam histórico. Otimizado para ler muito e escrever pouco.

O fluxo padrão é:

```
OLTP (operação)
      ↓
    ETL
      ↓
     DW  ← aqui você aplica OLAP de forma eficiente
      ↓
      BI
```

Só carregar dado no DW não é OLAP. Consultar esse dado de forma analítica — agregações, comparações de período, cruzamento de dimensões — é OLAP.

**OLAP precisa de ETL e DW?** Não necessariamente. Tecnicamente você pode rodar uma query analítica direto no banco OLTP — isso já seria OLAP. Mas seria lento, competiria com as transações em andamento, e o dado não estaria modelado para análise. O DW existe para separar os dois mundos.

---

### Desdobramento 1.1 — OLTP também é uma técnica ou é o tipo do banco?

**Dúvida:** O OLTP também é uma técnica? Ou seria o tipo do banco em si?

**Resposta:**

Os dois, dependendo do contexto:

| | OLTP | OLAP |
|---|---|---|
| **Tecnicamente** | Técnica/característica | Técnica/característica |
| **Na prática do mercado** | Virou sinônimo do banco transacional | Virou sinônimo de consulta analítica sobre o DW |

OLTP virou sinônimo de banco transacional porque todo banco transacional tem características OLTP — a associação é quase 1 para 1. OLAP nunca virou sinônimo de banco porque você pode aplicar a técnica em lugares diferentes (DW, cubo, engine analítico).

**Resumo prático:**
- OLTP = o banco (transacional, operacional)
- OLAP = a técnica (analítica, aplicada sobre o DW)

---

### Desdobramento 1.2 — Posso aplicar OLAP direto no OLTP?

**Dúvida:** Então eu poderia aplicar o OLAP direto no OLTP, mas por questão de performance acaba não fazendo sentido?

**Resposta:** Correto. Tecnicamente é possível, mas é custoso e impraticável porque:
- A query analítica compete com as transações em andamento
- O dado não está modelado para análise
- O banco não está otimizado para leitura em volume

---

## 2. O DW é o banco OLAP após ETL?

**Dúvida original:** O DW é o banco OLAP após ETL?

**Resposta:**

Quase isso, mas com precisão: o **DW é o destino físico**. O OLAP é o **modelo de acesso** que você aplica sobre esse destino.

- DW = estrutura (repositório histórico, modelado dimensionalmente)
- OLAP = o modo de usar essa estrutura

O DW pode ter um OLAP engine em cima (ex: cubo SSAS, Dremio, ou o próprio modelo estrela consultado via SQL).

---

## 3. Toda empresa tem OLTP e DW?

**Dúvida original:** Toda empresa tem os dois?

**Resposta:** Não. Pequenas empresas frequentemente têm só OLTP e rodam relatórios direto do transacional (lento e arriscado). DW + OLAP é uma escolha arquitetural que faz sentido quando o volume e a complexidade analítica justificam o investimento.

---

## 4. Tipos de banco de dados — Relacional, NoSQL e Orientado a Objetos

**Dúvida original:** O que é um banco de dados orientado a objetos? O que o distingue do relacional e do NoSQL?

**Resposta:**

| Tipo | Lógica central |
|---|---|
| **Relacional** | Dados em tabelas com linhas e colunas. Relacionamentos por chaves. Consultas em SQL. Ex: MySQL, PostgreSQL, SQL Server. |
| **NoSQL** | Não usa tabelas. Dado pode ser documento JSON, grafo, chave-valor. Criado para escala e flexibilidade de schema. Ex: MongoDB, Redis, Cassandra. |
| **Orientado a objetos** | Dado é armazenado como um objeto completo — com atributos e métodos, igual ao objeto de uma linguagem de programação. Sem necessidade de "achatar" em linhas e colunas. |

No banco orientado a objetos, um objeto `Cliente` com atributos e métodos entra e sai do banco como objeto, sem transformação. Na prática, quase não é usado no mercado.

---

### Desdobramento 4.1 — O que é ORM?

**Dúvida:** O que é ORM?

**Resposta:**

ORM (Object-Relational Mapping) é uma **ferramenta de código**, não um tipo de banco. É uma ponte entre a aplicação orientada a objetos e o banco relacional.

Você trabalha com objetos no código (ex: objeto `Cliente`). O ORM faz a tradução automaticamente:
- Salvar o objeto `Cliente` → ORM converte em `INSERT` na tabela
- Buscar um cliente → ORM faz o `SELECT` e devolve um objeto

O mercado resolveu a incompatibilidade entre código OO e banco relacional com ORMs, em vez de adotar bancos orientados a objetos.

---

## 5. Outros tipos de banco além do transacional

**Dúvida:** Existem outros tipos de banco de dados além do transacional?

**Resposta:**

Sim. Os principais:

| Tipo | Característica | Exemplos |
|---|---|---|
| **Transacional (OLTP)** | Otimizado para escrita rápida, transações simultâneas | PostgreSQL, SQL Server, MySQL |
| **Analítico Columnar** | Otimizado para leitura e agregação, armazena por coluna | Redshift, BigQuery, Snowflake, ClickHouse |
| **In-Memory** | Dado fica na RAM, extremamente rápido, usado para cache | Redis |
| **Distribuído** | Dado espalhado em vários nós, escala horizontal | Cassandra |
| **Grafo** | Otimizado para relacionamentos complexos | Neo4j |

> Nota: o Analítico Columnar usa SQL normalmente — é relacional na interface, mas muda o armazenamento interno para otimizar leitura analítica. Os demais (In-Memory, Distribuído, Grafo) são NoSQL.

---

### Desdobramento 5.1 — Como funciona o banco analítico columnar?

**Dúvida:** Como funciona o banco analítico columnar na prática?

**Resposta:**

**Banco relacional normal — armazena linha por linha:**

No disco: `1, Notebook, 3000, 2024-01-01, 2, Mouse, 150...`

**Banco columnar — armazena coluna por coluna:**

```
id:     1, 2, 3
nome:   Notebook, Mouse, Teclado
valor:  3000, 150, 300
data:   2024-01-01, 2024-01-02, 2024-01-03
```

Para a query `SELECT SUM(valor) FROM vendas WHERE data > '2024-01-01'`:
- Banco linha por linha → varre todas as colunas de todas as linhas
- Banco columnar → lê **só as colunas `valor` e `data`**, ignora o resto

Em tabelas com 100 colunas e bilhões de linhas, a diferença de performance é enorme. Por fora continua sendo SQL normal — a mudança é interna.

---

## 6. PK vs FK

**Dúvida original:** O que diferencia a PK da FK?

**Resposta:**

- **PK (Primary Key):** identifica unicamente cada linha dentro da própria tabela. Não pode ser nula, não pode repetir.
- **FK (Foreign Key):** é a PK de outra tabela usada aqui para criar o relacionamento. Pode repetir, pode ser nula.

```
Tabela CLIENTE          Tabela PEDIDO
-----------             --------------------
id_cliente (PK)  ←───  id_cliente (FK)
nome                    id_pedido (PK)
                        valor
```

FK é o mecanismo de integridade referencial — você não pode ter um pedido com `id_cliente = 99` se esse cliente não existir.

---

## 7. O que é um ERP?

**Dúvida original:** O que é de fato um ERP?

**Resposta:**

ERP (Enterprise Resource Planning) é um **sistema integrado de gestão** que centraliza os processos operacionais de uma empresa em um único banco de dados — financeiro, estoque, RH, compras, vendas, produção.

No contexto de DW: **o ERP é uma fonte OLTP de alta relevância**. Ele gera o dado transacional que você vai extrair via ETL. Exemplos: SAP, Oracle EBS, TOTVS Protheus.

---

## 8. Banco normalizado vs desnormalizado

**Dúvida original:** Banco normalizado e desnormalizado — qual a diferença? E a tabela também?

**Resposta:**

**Normalizado:** elimina redundância dividindo os dados em várias tabelas relacionadas. Segue as formas normais (1FN, 2FN, 3FN). Ideal para OLTP — escreve rápido, sem anomalias de atualização.

**Desnormalizado:** replica dados propositalmente para evitar JOINs. Ideal para OLAP/DW — lê rápido, query mais simples.

```
Normalizado (OLTP)              Desnormalizado (DW)
─────────────────               ──────────────────────────────
PEDIDO                          FATO_VENDA
  id_pedido                       id_pedido
  id_cliente (FK)                 nome_cliente     ← redundante, mas rápido
  id_produto (FK)                 categoria_produto ← idem
  valor                           valor
                                  data_venda
```

A mesma lógica vale para tabelas individualmente: normalizada divide atributos em entidades separadas; desnormalizada colapsa tudo em estrutura plana.

---

## 9. Modelagem Dimensional — Estrela e Floco de Neve

**Dúvida original:** O que seria a modelagem dimensional?

**Resposta:**

Técnica de modelagem específica para DW, criada por Ralph Kimball. Organiza os dados em **fatos** (o que aconteceu, com métricas) e **dimensões** (o contexto do que aconteceu).

---

### Desdobramento 9.1 — Esquema Estrela vs Floco de Neve

**Dúvida:** Qual a diferença entre Estrela e Floco de Neve? Achava que era a quantidade de tabelas fato.

**Resposta:**

A diferença **não é** quantas tabelas fato existem. É o **grau de normalização das dimensões**.

**Esquema Estrela — dimensões desnormalizadas:**

```
                DIM_TEMPO
                    |
DIM_CLIENTE — FATO_VENDA — DIM_PRODUTO
                    |
                DIM_LOJA
```

A dimensão produto traz tudo junto:

| id_produto | nome | categoria | fornecedor | tipo |
|---|---|---|---|---|
| 1 | Notebook X | Eletrônicos | Dell | Hardware |

**Esquema Floco de Neve — dimensões normalizadas:**

A dimensão produto vira:

| id_produto | nome | id_categoria | id_fornecedor | id_tipo |
|---|---|---|---|---|
| 1 | Notebook X | 3 | 7 | 2 |

Com tabelas separadas `DIM_CATEGORIA`, `DIM_FORNECEDOR`, `DIM_TIPO`.

**Comparativo:**

| | Estrela | Floco de Neve |
|---|---|---|
| Dimensões | Desnormalizadas | Normalizadas |
| JOINs | Poucos | Mais |
| Performance | Melhor | Pior |
| Quando usar | Padrão do mercado | Dimensões muito grandes ou com muita redundância |

> Quantas tabelas fato existem é uma questão separada — define se você tem um Data Mart (um assunto) ou um DW integrado (múltiplos assuntos). Mas dentro de cada um, você ainda escolhe entre Estrela ou Floco de Neve.

---

## 10. Tabelas Fato e Dimensão

**Dúvida original:** Características das tabelas fato e das dimensões.

**Resposta:**

| Característica | Tabela Fato | Tabela Dimensão |
|---|---|---|
| **Conteúdo** | Métricas/medidas (valor, quantidade, custo) | Atributos descritivos (nome, categoria, cidade) |
| **Volume** | Alto — cresce com cada transação | Baixo — cresce com novos cadastros |
| **Chaves** | FKs para todas as dimensões + PK própria | PK própria (surrogate key) |
| **Granularidade** | Define o nível de detalhe do fato | Define as possibilidades de filtro/agrupamento |
| **Exemplos** | Valor da venda, quantidade vendida | Cliente, Produto, Tempo, Loja |

A tabela fato sem dimensões é um número sem contexto. A dimensão sem fato é contexto sem evento. Os dois se completam.

---

### Desdobramento 10.1 — Existe um padrão de normalização para cada tipo de tabela?

**Dúvida:** Existe um padrão de normalizada e desnormalizada para essas tabelas? Uma dimensão sempre vai ser desnormalizada?

**Resposta:**

| Tabela | Padrão |
|---|---|
| **Fato** | Sempre normalizada — só métricas e FKs, não há o que desnormalizar |
| **Dimensão (Estrela)** | Desnormalizada — padrão do mercado |
| **Dimensão (Floco de Neve)** | Normalizada — exceção para dimensões muito grandes |

---
