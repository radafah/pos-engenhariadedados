# Atividade de Revisão — Modelagem e Arquitetura de Data Warehouse

> Respostas elaboradas sem consulta ao material. Avaliação: **8,5 / 10**.

---

## OLTP e OLAP

**1. OLTP e OLAP — Qual a diferença? São tipos de banco ou modos de uso?**

OLTP é um banco transacional, onde as informações são gravadas, alteradas e deletadas a todo momento. OLAP é o banco analítico. Sendo que ambos são modos de uso, mas o OLTP é visto mais como um tipo de banco e o OLAP como modo de uso.

---

**2. Qual o fluxo padrão entre OLTP, ETL, DW e BI?**

Os dados são registrados no OLTP, depois através do ETL eu limpo e normalizo os dados que são carregados no DW para então serem visualizados através de técnicas do BI.

---

**3. É possível aplicar OLAP direto no OLTP? Por que não é recomendado?**

Sim, mas não é recomendado por causa da performance.

---

**4. O DW é o banco OLAP após o ETL — essa afirmação está correta? O que falta nela?**

Incorreto. O DW é o repositório de dados após a tratativa de dados pelo ETL. OLAP é a técnica aplicada aos dados carregados no DW.

---

**5. Toda empresa tem OLTP e DW? Quando faz sentido ter os dois?**

Não. Só tem sentido quando se faz necessário analisar e organizar melhor os dados.

---

## Tipos de banco

**6. Qual a diferença entre banco relacional, NoSQL e orientado a objetos?**

Relacional é para uma estrutura consolidada e baixo a moderado volume de dados. NoSQL é para dados sem uma estrutura definida e grande volume. O orientado a objetos não é muito utilizado hoje em dia.

---

**7. O que é ORM e qual problema ele resolve?**

O ORM corrige o problema de organização das informações, transformando em colunas e linhas.

> ⚠️ **Correção:** O ORM não resolve organização em linhas e colunas — o banco relacional já faz isso. O ORM resolve a incompatibilidade entre código orientado a objetos e banco relacional: você trabalha com objetos no código, e o ORM traduz automaticamente para INSERT, SELECT, etc.

---

**8. Quais são os principais tipos de banco além do transacional? Cite pelo menos quatro com seus casos de uso.**

Analítico, Grafos, Chave-valor.

---

**9. Como o banco colunar armazena os dados diferente do relacional? Qual a vantagem disso?**

O colunar grava os dados de forma de coluna e não linha, o que facilita na hora de uma consulta. Por exemplo, quando quero saber o total de vendas de um dia específico, no transacional precisa ir linha a linha; no colunar só precisa ver a coluna data e valor.

---

## Modelagem relacional

**10. Qual a diferença entre PK e FK? O que acontece se você tentar inserir uma FK que não existe na tabela de origem?**

PK é a chave principal da tabela, e a FK é a chave de outra tabela. Se tentar gravar uma FK que não existe na origem, ele vai ficar em branco, porque a PK existe.

> ⚠️ **Correção:** O banco não deixa em branco — ele rejeita a operação e retorna erro. É exatamente esse o papel da integridade referencial: impedir que um pedido referencie um produto que não existe.

---

**11. O que é um ERP e qual o seu papel no contexto de DW?**

O ERP é um sistema que contempla todos os setores da empresa. O DW é necessário para realizar as análises dos dados.

---

**12. Qual a diferença entre banco normalizado e desnormalizado? Quando cada um é indicado?**

No normalizado eu tenho a chave das informações e no desnormalizado eu já tenho a informação direta. Quando preciso da informação mais rápida e direta uso a desnormalizada; quando tenho mais processamento e preciso de mais detalhe uso a normalizada.

> ⚠️ **Correção:** Os nomes foram invertidos na explicação. O desnormalizado tem a informação direta (mais rápido para leitura). O normalizado tem as chaves e exige joins (mais detalhado, mais lento para leitura, mas sem redundância).

---

## Modelagem dimensional

**13. O que são tabelas fato e dimensão? Qual a diferença entre elas?**

Fato é onde registro todas as transações e na dimensão eu tenho os cadastros. Na fato eu tenho as compras, onde as informações são em maioria chaves, e na dimensão eu tenho os valores que correspondem. Então na fato eu tenho o código do produto, mas na dimensão eu tenho o nome do produto.

---

**14. Qual a diferença entre Esquema Estrela e Floco de Neve? A diferença está na quantidade de tabelas fato?**

Em ambas eu trabalho com a fato normalizada, mas na estrela a dimensão é desnormalizada e na floco é normalizada. O que acaba fazendo com que o floco tenha mais tabelas. E não, a diferença não está na quantidade de fato — na verdade é na dimensão.

---

**15. A tabela fato é normalizada ou desnormalizada? E a dimensão?**

Fato sempre normalizada. E a dimensão vai depender do esquema: na estrela é desnormalizada e no floco é normalizada.

---

## NoSQL

**16. Como funciona a busca em um banco NoSQL sem índice? E com índice?**

Elas buscam através dos atributos (campos). Mas no índice já tenho um mapeamento dos principais campos.

---

**17. Por que o NoSQL foi criado? Qual limitação do relacional ele resolve?**

O NoSQL foi criado devido a fluxos de hoje em dia que são de grandes volumes e não têm um padrão. Cenário que o banco relacional não consegue suportar bem.

---

**18. Cite três casos reais de uso de NoSQL e por que o relacional não seria a melhor escolha em cada um.**

Cadastro de produtos Amazon, pois cada categoria tem seus atributos específicos. Netflix, pois cada usuário tem um comportamento diferente.

---

**19. Volume alto de dados sozinho justifica usar NoSQL? Dê um contraexemplo.**

Não, porque isso um banco relacional consegue suportar. O NoSQL deve ser usado quando além disso não existir um padrão de estrutura de informações.

---

**20. Qual a pergunta-chave para decidir entre relacional e NoSQL?**

Meus dados são em alto volume e estruturados? Se sim, relacional; se não, não relacional.

> ⚠️ **Correção:** Quase certa, mas faltou o critério de consistência. A pergunta completa é: *"Meus dados têm estrutura previsível e eu preciso de consistência forte?"* — volume alto com estrutura definida (ex: transações financeiras) ainda é relacional.

---

*Atividade realizada sem consulta ao material.*
