# Árvores, Random Forest e Boosting

Atividade prática sobre classificação de sobrevivência no Titanic, resolvida no notebook original `pratica_alunos.ipynb`.

## Entrega

- Notebook completo e executado, com lacunas preenchidas e respostas baseadas nos resultados reais.
- Comparação de árvores com profundidades 2, 4, 8 e sem limite.
- Random Forest com nove combinações de número de árvores e profundidade.
- Gradient Boosting com taxas 0.01, 0.1 e 1.0.
- Desafio com seis configurações adicionais e escolha pela maior acurácia de validação entre esses candidatos.
- Gráficos da árvore e da importância das variáveis.
- Extras: previsão de uma passageira inventada e avaliação do XGBoost.
- Tabela final com todos os modelos avaliados.

## Como executar

A execução foi verificada com Python 3.14. No terminal:

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

Abra `pratica_alunos.ipynb` no Visual Studio Code com as extensões Python e Jupyter, selecione o ambiente `.venv` e execute as células de cima para baixo com `Shift + Enter`.

Também é possível abrir no Google Colab. Se necessário, instale os pacotes de `requirements.txt` na sessão. Diferenças nas versões podem causar pequenas alterações nas pontuações.

A preparação original lê o CSV da internet. Para executar offline, altere somente `url` para `"titanic.csv"` e mantenha esse arquivo no diretório de trabalho. O CSV local é uma cópia da mesma base.

## Preparação preservada

A preparação e a função de avaliação foram mantidas exatamente como no material enviado:

- 891 passageiros e alvo `survived`.
- Idade ausente preenchida pela mediana, e porto ausente preenchido com `S`.
- Sexo convertido para 0 e 1; porto convertido em colunas indicadoras.
- Nove variáveis finais de entrada.
- Separação estratificada de 75% para treino e 25% para teste, com semente 42.
- Validação cruzada estratificada de cinco partes, com embaralhamento e semente 42.
- Acurácias médias de treino e validação nas mesmas divisões para todos os modelos.

A árvore desenhada e a floresta usada no gráfico de importância são ajustadas apenas em `X_treino`. As comparações da função `avaliar` usam validação cruzada sobre `X` e `y`, conforme o enunciado.

## Principais resultados

| Modelo | Treino | Validação |
| --- | --- | --- |
| Árvore max_depth=8 | 90,3% | 82,9% |
| RF 300 árvores, max_depth=8 | 92,1% | 84,3% |
| RF 300 árvores, sem limite, folha 3 | 89,7% | 84,2% |
| Boosting lr=0.01 | 83,5% | 81,5% |
| Boosting lr=0.1 | 90,1% | 82,7% |
| Boosting lr=1.0 | 98,2% | 81,1% |
| XGBoost | 87,7% | 82,4% |

A Random Forest com 300 árvores e profundidade 8 alcançou a maior validação: 84,3%. O melhor modelo isolado de árvore, com profundidade 8, teve 82,9%.

A primeira pergunta da árvore foi sobre sexo. No gráfico da floresta com 200 árvores e profundidade 6, as maiores importâncias foram sexo, tarifa e idade. Importância de variável indica participação nas decisões desse modelo, não uma relação causal.

## Limites da análise

A mediana da idade é calculada antes da separação e da validação cruzada na preparação original. Isso permite que informações das partes de avaliação participem desse preenchimento. Em um projeto real, o tratamento deve ser ajustado somente nos dados de treino, dentro de um pipeline.

As configurações foram comparadas nas mesmas divisões usadas para escolher o modelo. Por isso, o resultado escolhido pode ser otimista. Embora exista uma separação treino/teste para os gráficos, `avaliar` utiliza a base completa: esse teste não permanece independente da escolha. Uma decisão de produção exige uma avaliação final separada ou validação cruzada aninhada.

A previsão da passageira inventada é apenas uma demonstração do uso de `predict`, não uma certeza sobre uma pessoa real.

## Fontes

- [Base Titanic — seaborn-data](https://github.com/mwaskom/seaborn-data/blob/master/titanic.csv).
- [Contexto histórico — Royal Museums Greenwich](https://www.rmg.co.uk/stories/maritime-history/whistle-life-surviving-titanic-tragedy).
