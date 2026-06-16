# Modelagem e Arquitetura do Data Warehouse
**Tema 1** — 13/06/2026

---

## Conceitos Centrais

| Conceito | Definição |
|----------|-----------|
| **OLTP** | Dados transacionais — banco ativo no dia a dia da empresa (ex: sistema de vendas) |
| **OLAP** | Dados analíticos — orientado a consultas e análise |
| **DW** | Base de dados consolidada — grande repositório histórico de múltiplas fontes |

---

## Anotações

- O banco **OLTP** é aquele ativo no dia a dia, que sustenta todos os processos rodando no momento na empresa (ex: sistema de vendas do trabalho).
- O **DW** é um grande repositório de dados em grande volume — dados históricos e consolidados.
  - Sua função é a **consolidação de informações de diferentes fontes** e o **suporte à decisão**, pois facilita a criação de relatórios.
- A modelagem de dados do DW é feita através da **modelagem dimensional**:
  - **Tabela Fato** → transações
  - **Dimensão** → cadastros
- **Banco de Dados NoSQL**: dados armazenados sem relação igual a um banco relacional.
-     Exemplos d bancos NoSQL: Mongo = documentos | Redis = valor-chave | Cassandra = coluna-coluna

---

## Dúvidas Abertas

> *A preencher com pesquisa nos materiais complementares*

1. O OLAP surge em que momento? Ele é uma réplica do banco OLTP com limitação/definição de data, ou é outra coisa?
2. O DW é o banco OLAP após ETL?
3. Qual o momento do OLAP e qual o momento do DW?
4. Toda empresa tem os dois?
5. O que é um banco de dados orientado a objetos? O que o distingue do relacional e do NoSQL?
6. O que diferencia a PK da FK?
7. O que é de fato um ERP?
8. Banco normalizado e desnormalizado — qual a diferença? E a tabela tbm?
9. O que seria a modelagem dimensional?
10. Características das tabelas fato e das dimensões.

---

## Próximos Passos

- [ ] Ler material complementar 1
- [ ] Ler material complementar 2
- [ ] Responder dúvidas abertas com base nos materiais
- [ ] Exercícios práticos de modelagem dimensional
