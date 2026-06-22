# Anotações — Modelagem e Arquitetura do Data Warehouse

> Tema 2 — Anotações de aula e desdobramentos do estudo.

---

## Conceitos Centrais

- **Arquiteturas de DW** — formas de organizar e estruturar um Data Warehouse
- **Data Mart (DM)** — subconjunto do DW focado em um domínio ou área de negócio
- **Modelagens** — formas de estruturar as tabelas dentro do DW (Estrela e Floco de Neve)

---

## Anotações

### Conceito

<img src="https://cetax.com.br/wp-content/uploads/2022/03/data-warehouse2.gif" alt="Fluxograma do DW" width="500">

Staging Area: area de preparação dos dados. Onde acontece o ETL, por exemplo.

### Arquiteturas de DW

Existem três grandes abordagens arquiteturais:

- **Monolítica (ou centralizada):**
- **Em camadas:** 
- **Distribuída ou federada:** 

### Soluções de DW no mercado

- Amazon Redshift
- Google BigQuery
- Snowflake

## Data Mart

São orientados a assuntos (financeiro, marketing, RH, DP, Juridico, etc).
É uma sibdivisão do Data Warehouse.
Tem 2 pontos principais
- Foco Especifico
- Acesso Rápido

Eles podem ser Dependentes ou Independentes:

### Data Mart Dependente

```
Fonte → DW → DM
```

- Dados originam do DW corporativo
- Garante consistência e conceitos bem definidos entre os Data Marts
- Consistencia de dados e integração centralizada.

### Data Mart Independente

```
Fonte → DM
```

- Dados vêm direto da fonte, sem passar pelo DW
- Pode gerar problemas de consistência e divergência de conceitos entre áreas
- Flexibilidade e rapidez na implementação.
Porém, os dados podem acabar sendo aplicado de forma diferente de outras areas da empresa. Por cada uma trabalhar de uma forma, e ter visões diferentes sobre o mesmo assunto.

### Modelagens

- **Estrela**
Modelagem de dados no Esquema Estrela é composto por uma tabela da to (quantitativo) conectada a varias dimensões (informaçãoes contextuais).

- **Floco de Neve**
No Esquema Floco de Neve as tabelas dimensão são normalizadas em multiplas menores.

Ou seja, enquanto na estrela, numa dimensão eu ja tenho a descrição de informações como fornecedor e categoria, na Floco de Neve, essas informações são feitas em outras dimensões. Dessa forma, eu tenho a categoria ou o fornecedor cadastrado apenas uma vez, e não varias, evitando assim a reduncia e economizando espaço.


### Data Lake

É um repósitorio com todos os dados brutos de uma empresa, sem tratamento. É como se guardasse todo o historico para ser ou não usado em algum momento.

---

### Projeto de Modelagem de Dados

Quando estivermos em um projeto podemos analisar o processo em 4 etapas

1. Entender os dados disponiveis
2. Escolher o esquema a ser estruturado
3. Estruturar no esquema, criando as fatos e dimensões
4. Analisando e Dimensionando os dados com ferramentas de BI

## Dúvidas Abertas

1. A arquitetura em camadas seria a chamada de medalhão (bronze, prata e ouro)?
2. Quais exemplos práticos dessas arquiteturas? Por exemplo, m em camadas tem a camada de dados brutos — não seria o mesmo que a monolítica? Como diferenciar sendo que elas parecem se cruzar?
3. Em qual cenário se aplicaria o DM independente?
4. Na modelagem de Floco de Neve, em qual situação real ela seria preferível à Estrela?
5. Quais são as ferramentas ditas para "Business Inteligence"?
6. De que forma o DW se integrou as tecnologias de Big Data?
7. Qual o conceito de Datalke? Como se diferencia do DW?

---

## Pesquisar

-**REDSHIFT**: é um dos maiores DW em nuvem do mundo e pertence a Amazon. Entender melhor como ele funciona.

---

## Próximos Passos

- [ ] Resolver as dúvidas abertas e registrar no arquivo de dúvidas do tema 2
- [ ] Estudar exemplos práticos das três arquiteturas
- [ ] Aprofundar diferenças entre DM dependente e independente
- [ ] Assistir á aulas em video

---

*Anotações registradas manualmente em aula e transcritas para este repositório.*
