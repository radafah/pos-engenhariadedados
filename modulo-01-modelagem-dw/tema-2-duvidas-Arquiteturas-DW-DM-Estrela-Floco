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

*Documento gerado a partir das aulas de Modelagem e Arquitetura de Data Warehouse — Tema 2.*
