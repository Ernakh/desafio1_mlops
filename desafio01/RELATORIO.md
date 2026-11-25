# Desafio 1: relatório

**Aluno(a): Fabrício Tonetto Londero**

**Repositório:** https://github.com/Ernakh/desafio1_mlops

## 1. Versões dos dados

Rode o comando abaixo e complete a tabela com os 8 primeiros caracteres de cada md5:

```bash
for tag in dados-v1 dados-v2 dados-v3; do
  echo "$tag $(git show $tag:data/raw/bike.csv.dvc | awk '/md5/{print $3}')"
done
```

| Tag | md5 (8 primeiros caracteres) | Período |
|---|---|---|
| `dados-v1` | `fc7d423a` | 01/01/2011 a 31/12/2011 |
| `dados-v2` | `11e40c60` | 01/01/2011 a 30/06/2012 |
| `dados-v3` | `7747b2ef` | 01/01/2011 a 31/12/2012 |

## 2. Comparação no mesmo teste

Copie os números de `reports/comparacao.json`.

**Período do teste:** 04/12/2012 a 31/12/2012 (670 linhas)

| Alias         |Versão | RMSE   | MAE    | R²    |
|---------------|-------|--------|--------|-------|
| `@champion`   |  4.0  | 90.753 | 62.872 | 0.683 |
| `@challenger` |  6.0  | 62.894 | 41.50  | 0.848 |

## 3. Decisão

**Promovido ou rejeitado?** Promovido

**Justificativa (2 a 3 frases):**  No mesmo conjunto de teste, a versão 6 obteve RMSE de 62,894, enquanto a versão 4 obteve 90,753. Isso representa uma redução de aproximadamente 30,7% no RMSE. Como a regra é promover o candidato quando seu RMSE for menor, o alias `champion` passou para a versão 6 e a versão 4 foi registrada em `previous_champion`.

## 4. O que cada ferramenta fez nesse projeto?

**Pergunta 01:**
Com suas palavras, quais os dois usos do DVC nesse projeto?

Usei o DVC para versionar os três recortes da base de dados. O arquivo `bike.csv.dvc` guarda o hash que identifica cada versão, e as tags `dados-v1`, `dados-v2` e `dados-v3` permitem localizar esses estados no git. Também usei para executar o pipeline definido no `dvc.yaml`. Quando acrescentei os dados da v3, o `dvc repro` rodou os estágios afetados e gerou a comparação entre os modelos.


**Pergunta 02:**
Com suas palavras, para que serve o MLflow nesse projeto?

Foi usaod para registrar os treinos, seus parâmetros, métricas e modelos. O Model Registry permitiu identificar a versão em uso pelo alias `champion` e avaliar a candidata pelo alias `challenger`. Na promoção, o script registrou a decisão e o motivo nas tags da versão 6 e guardou o número da versão anterior para possbilitar um rollback.
