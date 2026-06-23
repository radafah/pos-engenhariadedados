# Dúvidas e Respostas — Arquiteturas de DW, Data Marts e Modelagem

> Registro das dúvidas originais da aula e dos desdobramentos que surgiram durante o estudo.

---

## 1. A arquitetura em camadas é a chamada de medalhão (bronze, prata e ouro)?

**Dúvida original:** A arquitetura em camadas seria a chamada de medalhão (bronze, prata e ouro)?

**Resposta:**

Não necessariamente. A arquitetura **em camadas** é o conceito — define que o dado deve passar por etapas separadas (ingestão, transformação, consumo).

O **medalhão** é uma implementação específica desse conceito, com a nomenclatura bronze/prata/ouro. Existem outras implementações em camadas que não usam essa nomenclatura.

> Todo medalhão é em camadas, mas nem toda arquitetura em camadas é medalhão.

---

### Desdobramento 1.1 — O que é de fato a arquitetura em camadas?

**Dúvida:** A arquitetura em camadas não ficou clara. Como ela funciona?

**Resposta:**

É a ideia de que o dado passa por **etapas separadas e bem definidas** antes de chegar no consumo final. Cada etapa tem uma responsabilidade clara:

```
Fonte → Ingestão → Transformação → Consumo
```

Cada etapa pode ser um sistema, uma plataforma ou um time diferente.

**Por que isso existe?** Porque misturar tudo em um lugar só (monolítica) gera problemas conforme o volume e a complexidade crescem:

- Difícil de reprocessar dado com erro
- Difícil de escalar partes específicas
- Uma mudança pode quebrar tudo

Separando em camadas, você isola os problemas. Se a transformação falhou, o dado bruto ainda está intacto na camada anterior.

**A relação com o Medalhão:**

O medalhão pegou esse conceito e deu nomes específicos para as camadas:

| Camada | Nome medalhão | O que acontece |
|---|---|---|
| Ingestão | Bronze | Dado bruto, como veio da fonte |
| Transformação | Prata | Dado limpo e padronizado |
| Consumo | Ouro | Dado refinado para análise |

O conceito de camadas existia antes do medalhão — ele popularizou e padronizou a nomenclatura, especialmente com o Databricks.

---

## 2. Arquitetura em camadas vs Monolítica — qual a diferença real?

**Dúvida original:** A arquitetura em camadas tem camada de dados brutos, não seria o mesmo que a monolítica? Como diferenciar sendo que elas parecem se cruzar?

**Resposta:**

- **Monolítica:** tudo no mesmo banco, no mesmo sistema. Ingestão, transformação e consumo acontecem no mesmo lugar. Simples de operar, mas difícil de escalar e manter.
- **Em camadas:** cada etapa pode ser um serviço ou plataforma diferente. Cada ferramenta faz o que faz melhor.

O dado bruto na arquitetura em camadas é uma **decisão arquitetural de rastreabilidade** — você guarda o original para poder reprocessar se houver erro nas transformações posteriores. Na monolítica simplesmente não existe essa separação.

**Resumo prático:**

| | Monolítica | Em camadas |
|---|---|---|
| Onde tudo acontece | Mesmo sistema | Sistemas separados por etapa |
| Dado bruto preservado | Não necessariamente | Sim, por design |
| Escalabilidade | Limitada | Alta — escala por camada |
| Complexidade | Baixa | Maior |

---

## 3. Em qual cenário se aplica o Data Mart independente?

**Dúvida original:** Em qual cenário se aplicaria o DM independente?

**Resposta:**

Quando um setor precisa de análise **urgente** e ainda não existe um DW corporativo, ou quando o DW central não atende aquele domínio específico.

É uma solução mais rápida de implementar, mas tem custo: cada área pode chegar a definições diferentes para o mesmo conceito — "receita" no financeiro pode ser calculada diferente do que no comercial, gerando inconsistência entre os Data Marts.

**Comparativo:**

| | DM Dependente | DM Independente |
|---|---|---|
| Fonte dos dados | DW corporativo | Direto da fonte |
| Consistência | Alta | Risco de divergência |
| Velocidade de implementação | Menor | Maior |
| Quando usar | Padrão recomendado | Urgência ou ausência de DW |

---

## 4. Em qual situação real o Floco de Neve seria preferível à Estrela?

**Dúvida original:** Na modelagem de Floco de Neve, em qual situação real ela seria preferível à Estrela?

**Resposta:**

Quando a dimensão é **muito grande e com muita redundância** — por exemplo, uma dimensão de produtos com milhões de registros onde categoria e fornecedor se repetem constantemente. Normalizar evita desperdício de armazenamento e facilita a manutenção quando um atributo muda.

Na prática, o Floco de Neve é exceção. O mercado prefere a Estrela pela performance e simplicidade de consulta.

---

## 5. Quais as ferramentas de BI disponíveis no mercado?

**Dúvida original:** Quais são as ferramentas de BI disponíveis atualmente no mercado?

**Resposta:**

| Ferramenta | Empresa | Destaque |
|---|---|---|
| Power BI | Microsoft | Mais usado no mercado corporativo, forte integração com Azure |
| Tableau | Salesforce | Referência em visualização, muito usado em grandes empresas |
| Looker | Google | Forte em modelagem semântica, integrado ao BigQuery |
| Qlik Sense | Qlik | Motor associativo, bom para exploração livre |
| Metabase | Metabase | Open source, popular em empresas menores |
| Apache Superset | Apache | Open source, muito usado em ambientes de engenharia de dados |

No contexto Azure, o Power BI é o mais natural por integração nativa.

---

## 6. Como o DW se integra com tecnologias de Big Data?

**Dúvida original:** De que forma um DW se integra às tecnologias de Big Data?

**Resposta:**

O DW tradicional tem limitações de volume e variedade de dados. As tecnologias de Big Data surgiram para preencher essa lacuna. A integração acontece assim:

```
Fontes diversas → Big Data (processamento em escala) → DW (dado refinado para análise)
```

O Big Data processa o volume bruto e o DW recebe apenas o dado já tratado e estruturado. Em ambientes modernos, essa fronteira virou o **Data Lakehouse** — que une os dois conceitos em uma única plataforma.

---

## 7. O que é o Data Lake?

**Dúvida original:** Qual o conceito do Data Lake?

**Resposta:**

É um repositório centralizado que armazena **dados em qualquer formato** — estruturado, semiestruturado e não estruturado — na sua forma bruta, sem transformação prévia.

A ideia é: ingere tudo primeiro, transforma depois quando precisar. Isso é diferente do DW, onde o dado já chega tratado.

Exemplos de tecnologia: Azure Data Lake Storage, Amazon S3, Google Cloud Storage.

---

## 8. Diferença entre Data Lake e Data Warehouse

**Dúvida original:** Qual a diferença do Data Lake para o Data Warehouse?

**Resposta:**

| | Data Lake | Data Warehouse |
|---|---|---|
| Formato dos dados | Qualquer formato (JSON, CSV, imagem, log) | Estruturado |
| Schema | Schema on read — define na leitura | Schema on write — define na escrita |
| Quando transforma | Depois, quando precisar | Antes, via ETL |
| Custo de armazenamento | Baixo | Mais alto |
| Público | Engenheiros e cientistas de dados | Analistas e BI |
| Risco | Pode virar "data swamp" sem governança | Dado já governado |

---

## 9. O que é o Redshift e por que é um dos principais DWs do mundo?

**Dúvida original:** O que é o Redshift e porque ele é um dos principais Data Warehouses do mundo atualmente?

**Resposta:**

O Amazon Redshift é o Data Warehouse em nuvem da AWS. É um dos mais adotados no mundo pelos seguintes motivos:

- **Colunar:** armazena dados por coluna, otimizado para queries analíticas em grande volume
- **Escala massiva:** suporta petabytes de dados
- **Integração com o ecossistema AWS:** S3, Glue, QuickSight, Lambda
- **SQL familiar:** você consulta com SQL padrão, sem curva de aprendizado de nova linguagem
- **Redshift Spectrum:** permite consultar dados direto no S3 sem carregar no cluster
- **Modelo pay-as-you-go:** você paga apenas pelo que consumir, sem contratos fixos ou infraestrutura própria — isso democratizou o DW para empresas menores

Foi um dos primeiros DWs em nuvem a ganhar escala real, o que consolidou sua adoção antes dos concorrentes.

---

### Desdobramento 9.1 — O que é o modelo pay-as-you-go?

**Dúvida:** O que é o modelo pay-as-you-go?

**Resposta:**

É o modelo de cobrança por uso — você paga apenas pelo que consumir, sem contratos fixos ou infraestrutura própria.

Antes da nuvem, para ter um DW uma empresa precisava comprar servidores, licenças de software e uma equipe para manter tudo. Custo alto e fixo, independente de usar muito ou pouco.

Com o pay-as-you-go:
- Ficou 10 horas processando? Paga por 10 horas.
- Não usou o fim de semana? Não paga.
- Precisa escalar para o dobro amanhã? Escala sem comprar hardware.

Isso democratizou o DW — empresas menores passaram a ter acesso a tecnologias que antes só grandes corporações podiam pagar.

---

## 10. O que é Hadoop e Spark? Existem outras tecnologias do mesmo tipo?

**Dúvida original:** O que é o Hadoop e Spark, qual sua função? Existem outras tecnologias do mesmo sentido?

**Resposta:**

**Hadoop** foi a primeira grande tecnologia de Big Data, criada pelo Google/Yahoo no início dos anos 2000. A ideia central é processar dados imensos distribuindo o trabalho em vários computadores menores ao invés de uma máquina poderosa.

Funciona em dois componentes principais:
- **HDFS** — sistema de arquivos distribuído, onde os dados ficam armazenados espalhados entre as máquinas
- **MapReduce** — modelo de processamento que divide a tarefa em partes, processa em paralelo e junta o resultado

O problema do Hadoop: é **lento**, porque lê e escreve em disco a cada etapa.

**Spark** veio para resolver exatamente isso. Faz o mesmo processamento distribuído, mas **na memória RAM**, o que o torna até 100x mais rápido que o Hadoop em muitos cenários. É o padrão do mercado hoje para processamento em larga escala. Suporta: batch, streaming, machine learning e SQL — tudo na mesma plataforma.

**Outras tecnologias do mesmo segmento:**

| Tecnologia | Função |
|---|---|
| Apache Kafka | Streaming de eventos em tempo real |
| Apache Flink | Processamento de stream, concorrente do Spark Streaming |
| Databricks | Plataforma gerenciada sobre Spark — o mais usado no mercado atual |
| Azure Synapse | Solução da Microsoft que une Big Data e DW |
| Dask | Processamento distribuído em Python, alternativa mais leve ao Spark |

---

## 11. As 5 Formas Normais na modelagem de dados

**Dúvida original:** Quais as 5FN na modelagem de dados de um banco de dados?

**Resposta:**

| Forma Normal | O que exige |
|---|---|
| **1FN** | Sem grupos repetidos, cada célula tem um único valor atômico |
| **2FN** | Estar na 1FN + todo atributo não-chave depende da chave inteira (elimina dependências parciais) |
| **3FN** | Estar na 2FN + nenhum atributo não-chave depende de outro atributo não-chave (elimina dependências transitivas) |
| **4FN** | Estar na 3FN + sem dependências multivaloradas independentes |
| **5FN** | Estar na 4FN + sem dependências de junção — a tabela não pode ser reconstruída pela junção de tabelas menores sem perda |

Na prática do mercado, chegar na **3FN já é suficiente** para a grande maioria dos casos. A 4FN e 5FN são raras e aparecem mais em contextos acadêmicos.

---

### Desdobramento 11.1 — Exemplo prático das 5 Formas Normais

**Dúvida:** Consegue dar um exemplo das 5FN?

**Resposta:**

Cenário: pedidos de uma loja.

**Tabela inicial — sem normalização:**

| id_pedido | cliente | telefones_cliente | produtos | categorias |
|---|---|---|---|---|
| 1 | Raul | 11999, 11888 | Notebook, Mouse | Eletrônicos, Periféricos |

---

**1FN — cada célula deve ter um único valor:**

| id_pedido | cliente | telefone | produto | categoria |
|---|---|---|---|---|
| 1 | Raul | 11999 | Notebook | Eletrônicos |
| 1 | Raul | 11999 | Mouse | Periféricos |
| 1 | Raul | 11888 | Notebook | Eletrônicos |

---

**2FN — atributos devem depender da chave inteira:**

`cliente` e `telefone` dependem só de `id_pedido`, não da chave composta `(id_pedido + produto)`. Separamos:

**Tabela Pedido:** `id_pedido | id_cliente`

**Tabela Cliente:** `id_cliente | cliente | telefone`

**Tabela Item Pedido:** `id_pedido | produto | categoria`

---

**3FN — nenhum atributo não-chave depende de outro atributo não-chave:**

`categoria` depende de `produto`, não de `id_pedido`. Separamos:

**Tabela Produto:** `id_produto | produto | id_categoria`

**Tabela Categoria:** `id_categoria | categoria`

---

**4FN — sem dependências multivaloradas independentes:**

Se um cliente pode ter vários telefones **e** vários endereços, e esses dois fatos são independentes entre si, não podem estar na mesma tabela — senão você gera combinações falsas.

**Errado (viola 4FN):**

| id_cliente | telefone | endereco |
|---|---|---|
| 10 | 11999 | Rua A |
| 10 | 11888 | Rua B |
| 10 | 11999 | Rua B ← combinação falsa |

**Correto:** tabela separada para telefones e outra para endereços.

---

**5FN — a tabela não pode ser reconstruída por junções menores sem perda:**

É a mais rara e complexa. Aparece quando existem três ou mais entidades relacionadas entre si com regras específicas de combinação que não podem ser representadas em tabelas menores sem gerar dados incorretos ao fazer o JOIN de volta. Na prática quase nunca se chega aqui — é mais relevante em teoria do que no dia a dia.

---

