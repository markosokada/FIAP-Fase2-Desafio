# notebooks/

Ordem numerada obrigatória. Cada notebook deve rodar de cima para baixo em ambiente limpo
(`Kernel → Restart & Run All`) sem erro — é isso que caracteriza reprodutibilidade.

| Arquivo | Escopo | Rúbrica |
|---|---|---|
| `01_eda.ipynb` | distribuições, correlações, outliers, balanceamento | Dim. 3 · 20 pts |
| `02_preprocessamento.ipynb` | nulos, definição do alvo, normalização, features | Dim. 4 · 15 pts |
| `03_modelagem.ipynb` | split/CV, treino de ≥ 2 modelos, comparação | Dim. 5 · 20 pts |
| `04_avaliacao.ipynb` | métricas, feature importance, implicações de negócio | Dim. 6 · 20 pts |

## Regras

- **Antes do commit:** verifique que a numeração das células está em ordem crescente
  (`[1]`, `[2]`, `[3]`...). Células fora de ordem indicam execução caótica e custam pontos.
- Mantenha as saídas dos gráficos salvas no notebook — o avaliador precisa ver os
  resultados sem executar nada.
- Todo gráfico precisa de um parágrafo de interpretação em markdown logo abaixo.
  Gráfico solto, sem leitura, perde metade dos pontos do critério.
- A primeira célula de cada notebook define `RANDOM_STATE = 42` e os caminhos.
  Não mude o valor entre notebooks.
