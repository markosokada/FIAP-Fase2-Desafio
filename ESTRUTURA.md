# Estrutura do repositório

Referência de organização para o Tech Challenge — Fase 2. A Dimensão 1 da rúbrica
avalia exatamente isto: 3 pts pela estrutura de diretórios, 3 pts pelo README,
2 pts pelo `requirements.txt` e 2 pts pela organização geral.

## Template, no momento do clone

```
.
├── .gitignore
├── CHECKLIST.md
├── ESTRUTURA.md
├── README.md
├── requirements.txt
├── data
│   ├── README.md
│   ├── raw
│   │   └── .gitkeep
│   └── processed
│       └── .gitkeep
├── docs
│   └── README.md
└── notebooks
    ├── 01_eda.ipynb
    ├── 02_preprocessamento.ipynb
    ├── 03_modelagem.ipynb
    ├── 04_avaliacao.ipynb
    └── README.md
```

## O mesmo repositório, no momento da entrega

Nomes de arquivo são ilustrativos, mas a organização deve ser esta.

```
.
├── .gitignore
├── CHECKLIST.md
├── README.md                          preenchido, sem marcadores PREENCHER
├── requirements.txt                   versões fixas do que foi realmente usado
│
├── data
│   ├── README.md                      instruções para baixar o dataset
│   ├── raw/                           vazio no Git — dados não versionados
│   └── processed/                     vazio no Git — gerado pelo notebook 02
│
├── docs
│   ├── README.md
│   └── apresentacao_executiva.pdf     Dimensão 7 · 5 pts
│
└── notebooks
    ├── 01_eda.ipynb                   Dimensão 3 · 20 pts
    ├── 02_preprocessamento.ipynb      Dimensão 4 · 15 pts
    ├── 03_modelagem.ipynb             Dimensão 5 · 20 pts
    ├── 04_avaliacao.ipynb             Dimensão 6 · 20 pts
    └── README.md
```

## Convenções

| Regra | Motivo |
|---|---|
| Notebooks numerados `01_` … `04_` | a ordem de leitura vira a ordem de execução |
| Nada de dataset em `data/` no Git | repositório leve e licença da fonte respeitada |
| Saídas dos gráficos salvas nos notebooks | o avaliador vê os resultados sem rodar nada |
| Modelos `.pkl` fora do Git | são reproduzíveis a partir do código |
| `RANDOM_STATE = 42` na primeira célula de cada notebook | mesmo número em toda execução |
| `snake_case`, sem acento e sem espaço em nomes de arquivo | compatibilidade entre Windows, macOS e Linux |
| Uma branch por integrante, merge via PR em `main` | histórico legível e trabalho paralelo sem conflito |

## Erros que mais custam pontos

1. **Repositório privado.** Zera a Dimensão 1 inteira, 10 pontos. Verifique em janela anônima.
2. **README com `<!-- PREENCHER -->`.** Sinaliza entrega inacabada antes mesmo da análise.
3. **Notebook com células fora de ordem** (`[7]`, `[2]`, `[15]`). Indica que o resultado
   não é reproduzível — e reprodutibilidade vale 3 pts diretos na Dimensão 5.
4. **Notebook commitado sem as saídas.** O avaliador abre e não vê gráfico nenhum.
5. **`requirements.txt` genérico**, copiado de outro projeto, listando o que não foi usado.
6. **Gráficos sem interpretação.** Custa metade dos pontos do critério na Dimensão 3.
7. **Dataset de 200 MB commitado** em `data/raw/`.
