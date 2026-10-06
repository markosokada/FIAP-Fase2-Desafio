# Tech Challenge — Fase 2 | POSTECH Data Analytics

> **INSTRUÇÕES:** este README é um template. Substitua **todos** os blocos marcados com
> `<!-- PREENCHER -->` e apague as linhas de instrução antes de submeter.
> O README vale **3 pontos** na Dimensão 1 da rúbrica.

---

## 1. Identificação

| Campo | Valor |
|---|---|
| Turma           | 2DTATBB  |
| Grupo           | Grupo 05 |
| Data de entrega | 06/10/2026 |

### Integrantes

| Nome completo | RM | E-mail |
|---|---|---|
|Natália Diniz Figueiredo Ramiro  |rm377771|natydfr@gmail.com          |
|Marcia Paula Soares Vieira       |rm377740|marciapaulasv@gmail.com    |
|Marcos Yuiichi Gomes Okada       |rm377783|marcosokada@bb.com.br      |
|Milton Cardoso de Paula Junior   |rm377780|milton.is.kauztik@gmail.com|
|Zildomar Aranha de Carvalho Filho|rm377759|zildoaranha@gmail.com      |

---

## 2. Links da entrega

Estes três links são **obrigatórios** e devem ser idênticos aos do PDF de submissão.

| Item | Link |
|---|---|
| Repositório |https://github.com/markosokada/FIAP-Fase2-Desafio.git|
| Vídeo executivo (≤ 5 min) | https://youtu.be/McV35SpMYVY|
| Apresentação | https://github.com/markosokada/FIAP-Fase2-Desafio/blob/main/docs/Apresentacao%20_Vers%C3%A3o_Final.pdf |

> ⚠️ Repositório privado ou inacessível **zera** toda a Dimensão 1 da rúbrica.
> Confira o acesso em uma janela anônima antes de enviar.

---

## 3. O problema

### Contexto de Negócio e Motivação para o Uso de Machine Learning

A concessão de crédito é uma das atividades mais rentáveis e, simultaneamente, mais arriscadas do setor financeiro. O grande desafio das instituições de crédito consiste em **minimizar o índice de inadimplência (perda financeira direta)** sem prejudicar o crescimento do negócio (rejeição excessiva de clientes legítimos). 

Tradicionalmente, a análise de perfil é realizada por meio de políticas de crédito estáticas baseadas em regras rígidas. Esse modelo tradicional falha ao não conseguir capturar correlações sutis e interações não-lineares complexas ocultas no histórico cadastral e de comportamento do consumidor.

A motivação para o uso de **Machine Learning** neste cenário justifica-se pela necessidade de transformar dados brutos de cadastro e histórico de pagamentos em um **escore de risco probabilístico preditivo**.  
Algoritmos avançados conseguem aprender padrões históricos de comportamento para discriminar, de forma automatizada e em alta escala, a probabilidade de um novo proponente se tornar um inadimplente estrutural.  
A automação desse processo otimiza a esteira de aprovação, reduz custos operacionais de cobrança e blinda a carteira de crédito da instituição contra perdas severas.

### Variável alvo

1. A variável alvo é a TARGET, uma variável binária (0 ou 1) que identifica o comportamento de risco de crédito do cliente no nível do indivíduo.  

2. Como ela foi definida e qual limiar de binarização foi adotado?A variável alvo foi extraída a partir do histórico mensal de pagamentos (df_credit_record), que originalmente continha a coluna STATUS dividida em 8 categorias ordinais e textuais. Houve um processo de binarização onde foi adotado o limiar de atrasos maior ou igual a 30 dias para a segmentação de risco, seguindo a regra estabelecida pela equipe de análise de dados. 

O mapeamento ocorreu da seguinte forma:  
Mau Pagador (Classe 1): Clientes que apresentaram status 1, 2, 3, 4 ou 5 (atrasos a partir de 30 dias até casos críticos superiores a 150 dias ou baixados em prejuízo) em qualquer momento do seu histórico mensal (critério do pior caso histórico/máximo).  
Bom Pagador (Classe 0): Clientes que mantiveram status C (pago no mês), X (sem empréstimo no mês) ou 0 (atrasos leves de 1 a 29 dias).  

3. Justificativa com base na distribuição das classes e comportamento de negócioA escolha desse limiar e a sua binarização são sustentadas por dois fatores críticos:  
Comportamento de Frequência Estatística: A distribuição volumétrica original mostra que as categorias C, X e 0 concentram a esmagadora maioria dos registros mensais da operação, sendo o status 0 considerado um evento comum de varejo e de baixo risco estrutural.  
Em contrapartida, as ocorrências do status 1 ao 5 tornam-se eventos extremamente raros e marginais na escala.  
A transição física do status 0 para o 1 marca estatisticamente o ponto de ruptura da normalidade e o início da inadimplência.  
Desbalanceamento Final: Ao consolidar o histórico mensal no nível de cada cliente único e realizar o cruzamento (Inner Join) com a base cadastral, a distribuição final da variável alvo revela um cenário de forte desbalanceamento, resultando em 11,77% de Maus Pagadores (Classe 1) na base fundida.  
Esse volume é estatisticamente suficiente para o aprendizado de padrões discriminatórios (via análise bivariada de Renda e Idade) e justifica o tratamento analítico robusto que foi aplicado nas fases de modelagem e avaliação de risco. 

### Dataset

#### Dateset application_record

| Campo | Valor |
|---|---|
| **Fonte** | Kaggle - Credit Card Approval Prediction Dataset |
| **Linhas × colunas (Cadastral Bruto)** | 438.557 linhas × 18 colunas |

|  **Nome da Coluna**      | Tipo de Dado | Descrição / Significado |  
| :---: | :---: | :---: |
| **ID**                  | int64   | dentificador único do cliente. |  
| **CODE_GENDER**         | object  | Gênero do cliente (Ex: M/F). |  
| **FLAG_OWN_CAR**        | object  | Indica se o cliente possui carro próprio (Y/N). |  
| **FLAG_OWN_REALTY**     | object  | Indica se o cliente possui imóvel próprio (Y/N). |  
| **CNT_CHILDREN**        | int64   | Quantidade de filhos do cliente. |  
| **AMT_INCOME_TOTAL**    | float64 | Rendimento anual total do cliente. |  
| **NAME_INCOME_TYPE**    | object  | Tipo de renda (Ex: Assalariado, Pensionista, Estudante). |  
| **NAME_EDUCATION_TYPE** | object  | Grau de escolaridade (Ex: Ensino Médio, Ensino Superior). |  
| **NAME_FAMILY_STATUS**  | object  | Estado civil (Ex: Casado, Solteiro, Divorciado). |  
| **NAME_HOUSING_TYPE**   | object  | Tipo de moradia (Ex: Casa própria, Apartamento alugado). |  
| **DAYS_BIRTH**          | int64   | Idade em dias (contados regressivamente a partir de hoje. Ex:  -12000). |  
| **DAYS_EMPLOYED**       | int64   | Tempo de emprego em dias (regressivo. Se positivo, significa desempregado). |  
| **FLAG_MOBIL**          | int64   | Indica se informou telefone celular (1 = Sim, 0 = Não). |  
| **FLAG_WORK_PHONE**     | int64   | Indica se informou telefone comercial (1 = Sim, 0 = Não). |  
| **FLAG_PHONE**          | int64   | Indica se informou telefone fixo residencial (1 = Sim, 0 = Não). |  
| **FLAG_EMAIL**          | int64   | Indica se informou e-mail (1 = Sim, 0 = Não). |  
| **OCCUPATION_TYPE**     | object  | Profissão/Ocupação (Contém dados faltantes). |  
| **CNT_FAM_MEMBERS**     | float64 | Quantidade de membros na família. |  

| **Período / versão** | Versão Atualizada / Dados Históricos |
| **Licença de uso** | CC0: Public Domain |
#### Dateset credit_record

| Campo | Valor |
|---|---|
| **Fonte** | Kaggle - Credit Card Approval Prediction Dataset |
| **Linhas × colunas (Resultado Final)** | (1048575 linhas X 3 colunas)|

|  **Nome da Coluna**      | Tipo de Dado | Descrição / Significado |  
| :---: | :---: | :---: |
| **ID**                  | int64   | Identificador único do cliente ou da conta de crédito.5001711, 5001712| 
| **MONTHS_BALANCE**      | int64   | Mês de referência dos dados em relação ao mês atual.0 (mês atual), -1 (1 mês atrás), -2 (2 meses atrás)|
| **STATUS**              | object  | Situação de pagamento (0: 1-29 dias; 1: 30-59; 2: 60-89; 3: 90-119; 4: 120-149; 5: >150 dias; C: pago; X: sem empréstimo).|

| **Período / versão** | Versão Atualizada / Dados Históricos |
| **Licença de uso** | CC0: Public Domain |

#### Dateset application_record_tratado

| **Linhas × colunas (Resultado Final)** | (36457 linhas X 44 colunas)|

|  **Nome da Coluna**      | Tipo de Dado | Descrição / Significado |  
| :---: | :---: | :---: |
| **NAME_EDUCATION_TYPE** | float64 | Grau de escolaridade codificado ordinalmente: **0.0**: Ensino Fundamental (`Lower secondary`), **1.0**: Ensino Médio (`Secondary / secondary special`), **2.0**: Ensino Superior Incompleto (`Incomplete higher`), **3.0**: Ensino Superior Completo (`Higher education`), **4.0**: Pós-graduação (`Academic degree`) |
| **NAME_INCOME_TYPE_Pensioner** | float64 | Flag binária (`1.0` ou `0.0`). Indica se o tipo de renda é Pensionista. |
| **NAME_INCOME_TYPE_State servant** | float64 | Flag binária (`1.0` ou `0.0`). Indica se o tipo de renda é Servidor Público. |
| **NAME_INCOME_TYPE_Student** | float64 | Flag binária (`1.0` ou `0.0`). Indica se o tipo de renda é Estudante. |
| **NAME_INCOME_TYPE_Working** | float64 | Flag binária (`1.0` ou `0.0`). Indica se o tipo de renda é Assalariado/Trabalhador Regular. |
| **NAME_FAMILY_STATUS_Married** | float64 | Flag binária (`1.0` ou `0.0`). Indica se o estado civil é Casado. |
| **NAME_FAMILY_STATUS_Separated** | float64 | Flag binária (`1.0` ou `0.0`). Indica se o estado civil é Separado/Divorciado. |
| **NAME_FAMILY_STATUS_Single / not married** | float64 | Flag binária (`1.0` ou `0.0`). Indica se o estado civil é Solteiro. |
| **NAME_FAMILY_STATUS_Widow** | float64 | Flag binária (`1.0` ou `0.0`). Indica se o estado civil é Viúvo. |
| **NAME_HOUSING_TYPE_House / apartment** | float64 | Flag binária (`1.0` ou `0.0`). Indica se a moradia é Casa ou Apartamento próprio. |
| **NAME_HOUSING_TYPE_Municipal apartment** | float64 | Flag binária (`1.0` ou `0.0`). Indica se a moradia é Apartamento Municipal (público). |
| **NAME_HOUSING_TYPE_Office apartment** | float64 | Flag binária (`1.0` ou `0.0`). Indica se a moradia é Apartamento Comercial/Empresarial. |
| **NAME_HOUSING_TYPE_Rented apartment** | float64 | Flag binária (`1.0` ou `0.0`). Indica se a moradia é Apartamento Alugado. |
| **NAME_HOUSING_TYPE_With parents** | float64 | Flag binária (`1.0` ou `0.0`). Indica se o cliente reside com os pais. |
| **OCCUPATION_TYPE_Cleaning staff** | float64 | Flag binária (`1.0` ou `0.0`). Indica se pertence à equipe de limpeza. |
| **OCCUPATION_TYPE_Cooking staff** | float64 | Flag binária (`1.0` ou `0.0`). Indica se pertence à equipe de cozinha. |
| **OCCUPATION_TYPE_Core staff** | float64 | Flag binária (`1.0` ou `0.0`). Indica se pertence à equipe administrativa ou essencial. |
| **OCCUPATION_TYPE_Drivers** | float64 | Flag binária (`1.0` ou `0.0`). Indica se a profissão do proponente é Motorista. |
| **OCCUPATION_TYPE_HR staff** | float64 | Flag binária (`1.0` ou `0.0`). Indica se pertence à equipe de Recursos Humanos. |
| **OCCUPATION_TYPE_High skill tech staff** | float64 | Flag binária (`1.0` ou `0.0`). Indica se é Técnico de Alta Qualificação. |
| **OCCUPATION_TYPE_IT staff** | float64 | Flag binária (`1.0` ou `0.0`). Indica se pertence à equipe de Tecnologia da Informação. |
| **OCCUPATION_TYPE_Laborers** | float64 | Flag binária (`1.0` ou `0.0`). Indica se a ocupação é Operário ou Trabalhador Manual Geral. |
| **OCCUPATION_TYPE_Low-skill Laborers** | float64 | Flag binária (`1.0` ou `0.0`). Indica se é Operário de Baixa Qualificação. |
| **OCCUPATION_TYPE_Managers** | float64 | Flag binária (`1.0` ou `0.0`). Indica se exerce função de Gerente ou Gestor. |
| **OCCUPATION_TYPE_Medicine staff** | float64 | Flag binária (`1.0` ou `0.0`). Indica se pertence ao corpo de Medicina ou Saúde. |
| **OCCUPATION_TYPE_Private service staff** | float64 | Flag binária (`1.0` ou `0.0`). Indica se atua na equipe de Serviços Privados ou Particulares. |
| **OCCUPATION_TYPE_Realty agents** | float64 | Flag binária (`1.0` ou `0.0`). Indica se a ocupação mapeada é Corretor Imobiliário. |
| **OCCUPATION_TYPE_Sales staff** | float64 | Flag binária (`1.0` ou `0.0`). Indica se pertence à equipe de Vendas comerciais. |
| **OCCUPATION_TYPE_Secretaries** | float64 | Flag binária (`1.0` ou `0.0`). Indica se a profissão mapeada é Secretário(a). |
| **OCCUPATION_TYPE_Security staff** | float64 | Flag binária (`1.0` ou `0.0`). Indica se pertence ao time corporativo de Segurança. |
| **OCCUPATION_TYPE_Unknown** | float64 | Flag binária (`1.0` ou `0.0`). Categoria estruturada para mapear dados ausentes de ocupação. |
| **OCCUPATION_TYPE_Waiters/barmen staff** | float64 | Flag binária (`1.0` ou `0.0`). Indica se a profissão mapeada é Garçom ou Bartender. |
| **CODE_GENDER** | float64 | Gênero biológico do cliente mapeado numericamente: **1.0** = Masculino / **0.0** = Feminino. |
| **FLAG_OWN_CAR** | float64 | Indica se o cliente possui automóvel próprio: **1.0** = Sim / **0.0** = Não. |
| **FLAG_OWN_REALTY** | float64 | Indica se o cliente possui bem imóvel residencial próprio: **1.0** = Sim / **0.0** = Não. |
| **FLAG_WORK_PHONE** | float64 | Indica se informou contato telefônico comercial válido: **1.0** = Sim / **0.0** = Não. |
| **FLAG_EMAIL** | float64 | Indica se o cliente forneceu um endereço de correio eletrônico (E-mail): **1.0** = Sim / **0.0** = Não. |
| **CNT_FAM_MEMBERS** | float64 | Quantidade quantitativa total de membros integrantes no núcleo familiar do cliente. |
| **TARGET** | float64 | **Variável Resposta (Alvo):** **1.0** = Mau Pagador (atraso \(\ge 30\) dias) / **0.0** = Bom Pagador. |
| **AGE_YEARS** | float64 | **Métrica Derivada:** Idade cronológica do cliente final expressa em anos inteiros e frações. |
| **EMPLOYED_SPECIAL_VALUE** | float64 | **Métrica Derivada:** Flag indicativa isolando o registro anômalo técnico original de emprego. |
| **EMPLOYED_YEARS** | float64 | **Métrica Derivada:** Tempo total de vínculo empregatício ativo convertido para a escala anual. |
| **LOG_INCOME** | float64 | **Métrica Derivada:** Alinhamento logarítmico (`log1p`) do salário para fins de suavização de escala. |

---

## 4. Como reproduzir

```bash
git clone https://github.com/markosokada/FIAP-Fase2-Desafio.git
cd FIAP-Fase2-Desafio

python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

pip install -r requirements.txt
jupyter notebook
```

Baixe o dataset e coloque o arquivo bruto em `data/raw/` (os dados **não** são versionados —
veja `data/README.md`).

Depois execute os notebooks nesta ordem:

| # | Notebook | O que faz |
|---|---|---|
| 1 | `notebooks/01_eda.ipynb` | Análise exploratória |
| 2 | `notebooks/02_preprocessamento.ipynb` | Limpeza, escala e feature engineering |
| 3 | `notebooks/03_modelagem.ipynb` | Treino e comparação dos modelos |
| 4 | `notebooks/04_avaliacao.ipynb` | Métricas, importância de variáveis e conclusões |

**Semente fixa:** `RANDOM_STATE = 42`, declarada na primeira célula de cada notebook.
Rodar os notebooks na ordem acima, a partir de um ambiente limpo, deve reproduzir
exatamente os números da seção 5.

---

## 5. Resultados

| Modelo | Acurácia | Precisão | Recall | F1 |
|---|---|---|---|---|
| <!-- PREENCHER --> | | | | | |
| Regressão Logística    | 0.57 | 0.14 | 0.51 | 0.22 |
| Árvore de Decisão      | 0.79 | 0.31 | 0.66 | 0.42 |
| Random Forest          | 0.80 | 0.32 | 0.63 | 0.43 |
| Support Vector Machine | 0.67 | 0.18 | 0.53 | 0.27 |

Modelo escolhido: **Random Forest** (Campeão com média de F1-Score de 0,4035).
Métricas priorizadas: **F1-Score** (Equilíbrio entre Precisão e Recall). Algoritmos lineares (**Regressão Logística**) e de distância (**SVM**) até alcançaram um bom Recall, mas geraram falsos alarmes excessivos, o que barraria clientes legítimos. O Random Forest contornou esse problema ao mapear com precisão os padrões não-lineares do cadastro, oferecendo a melhor proteção contra a inadimplência sem sufocar a operação comercial do banco.
---

## 6. Principais conclusões


Os resultados obtidos com a homologação do nosso modelo final trazem impactos práticos imediatos para a operação da mesa de crédito, permitindo uma transição de uma postura reativa para uma estratégia preditiva orientada a dados. A partir da implementação deste novo motor de decisão, a diretoria e as equipes de negócios devem adotar as seguintes ações práticas:

    • ⚙️ Automação e Eficiência em Larga Escala: O modelo comprovou sua capacidade de aprovar com segurança 5.309 clientes legítimos de forma 100% automatizada. Na prática, isso elimina a necessidade de análises manuais demoradas para a esmagadora maioria das propostas. O impacto direto é a redução drástica do tempo de espera do cliente (melhorando a conversão de vendas) e a otimização do custo operacional da mesa de crédito, permitindo que a equipe foque apenas nos casos mais complexos.
    • 🛡️ Bloqueio Preventivo de Prejuízos: Ao detectar com precisão 537 maus pagadores antes mesmo que o recurso saia do caixa, o modelo estanca perdas financeiras diretas que hoje corroeriam a margem de lucro da instituição. Esse bloqueio automático evita o acionamento de dispendiosas esteiras de cobrança jurídica ou perdas por calotes irrecuperáveis.
    • 🎯 Estratégias de Mitigação de Risco Inteligentes: O modelo revelou que a maior frequência de inadimplência está concentrada em perfis de clientes mais jovens ou que atuam em profissões operacionais de menor qualificação. No entanto, o objetivo do negócio não é simplesmente fechar as portas para esse público, o que destruiria o faturamento comercial. A implicação prática aqui é a criação de políticas de concessão inteligente: para esses grupos de maior risco, a mesa de crédito pode parametrizar o sistema para exigir garantias adicionais, conceder limites iniciais de crédito reduzidos ou aplicar uma precificação de juros ajustada ao risco.
    • 📊 Previsibilidade Financeira e Calibração de Reservas: Nenhuma esteira de crédito é 100% infalível, e o modelo deixou claro o seu risco residual (os 321 calotes que ele deixa passar). O grande ganho prático para a diretoria financeira é a previsibilidade: sabendo exatamente qual é a taxa de erro esperada do algoritmo no cenário real, a instituição pode calibrar com precisão o seu fundo de reserva para inadimplência (PCLD) e embutir essa perda prevista no cálculo do spread bancário, garantindo a lucratividade da operação.

Em resumo: O modelo deixa de ser apenas uma ferramenta técnica e passa a guiar a estratégia da empresa no que diz respeito à concessão de crédito, garantindo o equilíbrio perfeito entre acelerar as vendas e manter o caixa protegido.

---
### Importancia das variáveis

A avaliação de importância dos recursos quantifica a contribuição de cada dado cadastral na estrutura de decisão do Random Forest. Longe de indicar um fator determinante isolado, a métrica reflete o ganho de informação no processo de quebra das árvores. Sob essa ótica, a Idade em anos (AGE_YEARS), o Tempo de Emprego (EMPLOYED_YEARS) e a Renda Suavizada (LOG_INCOME) figuraram como as variáveis mais utilizadas pelo modelo para realizar as separações, apresentando a maior frequência e peso na divisão dos nós das árvores.
Esse comportamento do algoritmo valida as premissas clássicas do domínio de análise de risco de crédito:

    • 📌 Maturidade e Comportamento — Idade (AGE_YEARS): A idade se consolidou como o recurso de maior relevância estatística para as decisões do modelo. 
    Conforme observado na Análise Exploratória de Dados (EDA), há uma correlação indicando que perfis mais jovens apresentam maior frequência de inadimplência grave. 
    Embora o algoritmo identifique essa forte associação, é importante ressaltar que os dados disponíveis não permitem estabelecer uma relação direta de causa e efeito. 
    Estatisticamente, a idade atua como uma variável aproximada (proxy) de estabilidade comportamental: o modelo captura que grupos de clientes mais velhos tendem a apresentar um comportamento de pagamento mais seguro, o que pode refletir padrões associados a hábitos de consumo consolidados ou maior aversão ao risco de negativação.

    • 📌 Estabilidade de Caixa — Tempo de Emprego (EMPLOYED_YEARS): O tempo de permanência no emprego atual consolidou-se como a segunda variável mais importante. 
    No mercado de crédito, a estabilidade profissional é um dos pilares mais fortes para prever a adimplência. Um cliente com maior tempo de casa possui um fluxo de caixa pessoal muito mais previsível e menor volatilidade de renda, estando menos exposto ao risco de choques financeiros causados por demissões repentinas. 
    O modelo também se apoia fortemente na variável de controle EMPLOYED_SPECIAL_VALUE para isolar o risco daqueles que não possuem emprego ativo, mas têm rendimentos garantidos (como aposentados e pensionistas).

    • 📌 Capacidade de Absorção de Choques — Renda (LOG_INCOME): A renda anual total calculada na escala logarítmica foi o terceiro fator de maior impacto. 
    Embora na análise bivariada as distribuições brutas de renda fossem semelhantes, o Random Forest — por ser um modelo não-linear e baseado em árvores — consegue cruzar de forma inteligente a faixa de renda com as demais variáveis. 
    Na prática, a renda não define o risco de forma isolada, mas serve para o modelo calcular a capacidade de pagamento e o fôlego financeiro do proponente: uma renda robusta combinada com uma idade madura gera um perfil de risco baixíssimo, enquanto uma renda menor combinada com pouca idade acende o sinal de alerta do algoritmo.

---   
### Limitações e próximos passos

Nenhum modelo preditivo é uma ferramenta perfeita e imune a falhas. Expor as limitações técnicas e de dados deste projeto de forma transparente não é um sinal de ineficiência, mas sim o selo de governança e rigor acadêmico. Isso garante que a diretoria de crédito possa calibrar seu nível de confiança nas tomadas de decisão regulatórias e operacionais.

As principais limitações identificadas neste ciclo de desenvolvimento, bem como as estratégias recomendadas para superá-las em iterações futuras, são descritas a seguir:

    A) Natureza Estática e Autorreferencial dos Dados (A Ausência de Reguladores de Crédito)
A maior fragilidade estrutural do modelo atual reside na origem estática das variáveis preditivas. O algoritmo tomou decisões de risco baseando-se estritamente em um formulário cadastral preenchido pelo próprio cliente no momento da solicitação (como idade, renda declarada e estado civil).

        • O Impacto: O modelo opera "às cegas" em relação ao comportamento financeiro de mercado em tempo real. Ele não possui acesso à pontuação do cliente em birôs de crédito externos (como Serasa, SPC ou Boa Vista) e, mais criticamente, não captura o histórico de restrições ou o endividamento sistêmico do cidadão registrado no Sistema de Informações de Crédito (SCR) do Banco Central.
        
        • O Próximo Passo: Em uma segunda fase do projeto, é mandatório integrar o pipeline a APIs de reguladores de crédito tradicionais e dados de Open Finance. Adicionar variáveis como o histórico de negativações recentes, a taxa de utilização de limite de cartões de terceiros e o volume de consultas recentes ao CPF blindará o modelo contra fraudes de declaração e elevará drasticamente a precisão da ferramenta.

    B) Desafio de Classes Raras e Alta Cardinalidade em Profissões (OCCUPATION_TYPE)
Conforme mapeado na fase de Análise Exploratória (EDA), a base fundida sofre de uma severa cauda de alta cardinalidade e amostras microscópicas em certas ocupações (como as equipes de TI e RH, que contam com uma volumetria reduzida no universo de teste).

        • O Impacto: O algoritmo Random Forest pode sofrer de superajuste (overfitting) ao criar ramificações específicas e profundas para esses microgrupos profissionais. O modelo corre o risco de "decorar" o comportamento de pouquíssimos indivíduos daquela profissão, perdendo a capacidade de generalizar o risco de forma justa quando um novo profissional de tecnologia ou recursos humanos solicitar um cartão de crédito.

        • O Próximo Passo: Para contornar essa limitação, deve-se aplicar uma etapa de reagrupamento socioeconômico estruturado. Em vez de operar com 18 profissões isoladas, os registros devem ser consolidados em 4 ou 5 grandes macro-grupos baseados em faixas salariais homogêneas de mercado e estabilidade jurídica — por exemplo: Corporativo Técnico, Operacional de Risco, Administrativo Estável e Autônomos.

    C) Restrição Algorítmica e Falta de Otimização de Hiperparâmetros
O ciclo de modelagem atual cumpriu a exigência de testar múltiplos classificadores sob validação cruzada robusta, alcançando um F1-Score médio de 0,4035 no classificador campeão. Contudo, o algoritmo Random Forest foi treinado utilizando seus parâmetros estruturais padrão (baseline).

        • O Impacto: O modelo pode estar operando em uma zona de subotimização. Sem um ajuste fino, o equilíbrio entre a captura de inadimplentes (Recall) e o veto a clientes saudáveis (Precisão) fica limitado ao comportamento padrão do estimador.

        • O Próximo Passo: Com maior tempo computacional, recomenda-se realizar uma Otimização Bayesiana ou um GridSearchCV focado nos hiperparâmetros da Random Forest (como profundidade máxima das árvores, número mínimo de amostras por folha e balanceamento customizado de pesos). Adicionalmente, o pipeline deve abrir espaço para testar algoritmos avançados de Gradient Boosting (como XGBoost e LightGBM), amplamente conhecidos por extraírem maior eficiência e performance em dados tabulares altamente desbalanceados.


---

## 7. Estrutura do repositório

```
.
├── data/          dados brutos (raw) e tratados (processed) — não versionados
├── notebooks/     análise em ordem numerada
└── docs/          apresentação executiva
```

Detalhes e convenções em [`ESTRUTURA.md`](ESTRUTURA.md).
Antes de enviar, percorra o [`CHECKLIST.md`](CHECKLIST.md).

---

## 8. Tecnologias

anyio==4.15.1
argon2-cffi==25.1.0
argon2-cffi-bindings==26.1.0
arrow==1.4.0
asttokens==3.0.2
async-lru==2.3.0
attrs==26.1.0
babel==2.18.0
beautifulsoup4==4.15.0
bleach==6.4.0
certifi==2026.7.22
cffi==2.1.1
charset-normalizer==3.5.1
comm==0.2.3
contourpy==1.4.0
cycler==0.12.1
debugpy==1.8.22
defusedxml==0.7.1
executing==2.2.1
fastjsonschema==2.22.2
fonttools==4.66.0
fqdn==1.5.1
h11==0.16.0
httpcore==1.0.9
httpx==0.28.1
idna==3.20
ipykernel==7.3.0
ipython==9.17.1
ipython_pygments_lexers==1.1.1
ipywidgets==8.1.9
isoduration==20.11.0
jedi==0.20.0
Jinja2==3.1.6
joblib==1.4.2
json5==0.15.0
jsonpointer==3.1.1
jsonschema==4.26.0
jsonschema-specifications==2025.9.1
jupyter==1.1.1
jupyter-console==6.6.3
jupyter-events==0.12.1
jupyter-lsp==2.3.1
jupyter_builder==1.2.3
jupyter_client==8.10.0
jupyter_core==5.9.1
jupyter_server==2.21.1
jupyter_server_terminals==0.5.4
jupyterlab==4.6.4
jupyterlab_pygments==0.3.0
jupyterlab_server==2.28.1
jupyterlab_widgets==3.0.17
kiwisolver==1.5.1
lark==1.3.1
MarkupSafe==3.0.3
matplotlib==3.9.2
matplotlib-inline==0.2.2
mistune==3.3.4
narwhals==2.26.0
nbclient==0.11.0
nbconvert==7.17.1
nbformat==5.11.1
nest-asyncio2==1.7.3
notebook==7.6.3
notebook_shim==0.2.4
numpy==2.1.3
packaging==26.3
pandas==2.2.3
pandocfilters==1.5.1
parso==0.8.7
pexpect==4.9.0
pillow==12.3.0
platformdirs==4.11.12
prometheus_client==0.26.0
prompt_toolkit==3.0.53
psutil==7.2.2
ptyprocess==0.7.0
pure_eval==0.2.4
pycparser==3.0
Pygments==2.21.0
pyparsing==3.3.3
python-dateutil==2.9.0.post0
python-json-logger==4.2.0
pytz==2026.4
PyYAML==6.0.3
pyzmq==27.2.0
referencing==0.37.0
requests==2.34.2
rfc3339-validator==0.1.4
rfc3986-validator==0.1.1
rfc3987-syntax==1.1.0
rpds-py==2026.6.3
scikit-learn==1.9.1
scipy==1.18.1
seaborn==0.13.2
Send2Trash==2.1.0
six==1.17.0
soupsieve==2.10
stack-data==0.6.3
terminado==0.18.1
threadpoolctl==3.7.0
tinycss2==1.5.1
tornado==6.5.10
traitlets==5.16.1
typing_extensions==4.16.0
tzdata==2026.4
uri-template==1.3.0
urllib3==2.8.0
wcwidth==0.9.1
webcolors==25.10.0
webencodings==0.6.1
websocket-client==1.9.2
widgetsnbextension==4.0.16

