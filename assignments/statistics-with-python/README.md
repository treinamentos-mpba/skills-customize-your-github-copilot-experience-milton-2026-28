# 📘 Atividade: Statistics with Python

## 🎯 Objetivo

Analise dados com pandas e realize cálculos estatísticos com NumPy, incluindo média, mediana, variância, desvio padrão e correlação. Pratique a preparação de dados e a interpretação dos resultados usando um conjunto pequeno de registros fictícios de estudantes.

## 📝 Tarefas

### 🛠️ Carregar e explorar os dados

#### Descrição
Comece lendo o arquivo `data.csv` fornecido nesta atividade. Você deve conhecer funções, listas e conceitos básicos de Python; esta primeira etapa deve levar menos de 10 minutos.

Crie e ative um ambiente virtual e instale as bibliotecas:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install pandas numpy
```

Crie seu programa em `analysis.py`. O CSV contém as colunas `student_id` (identificador fictício), `group` (turma), `study_hours` (horas de estudo) e `score` (nota entre 0 e 100). Execute o programa na pasta que contém o CSV com `python analysis.py`.

#### Requisitos
O programa concluído deve:

- Importar pandas e NumPy e carregar `data.csv` com `pandas.read_csv`.
- Exibir as primeiras cinco linhas, as dimensões do DataFrame e os tipos das colunas.
- Informar a quantidade de valores ausentes em cada coluna.
- Confirmar que o arquivo contém 8 registros e 4 colunas, com uma nota ausente.

### 🛠️ Preparar os dados para análise

#### Descrição
Crie uma cópia dos dados para análise e remova apenas os registros sem nota ou horas de estudo. Não substitua notas ausentes por zero: uma observação sem nota não representa uma nota zero.

#### Requisitos
O programa concluído deve:

- Manter o DataFrame original intacto para permitir a comparação antes e depois da limpeza.
- Usar `dropna` com as colunas `score` e `study_hours` para obter o conjunto de análise.
- Informar o número de registros removidos e o total restante: 1 removido e 7 restantes.
- Verificar que não há valores ausentes nas duas colunas numéricas do conjunto limpo.
- Usar esse mesmo conjunto limpo em todos os cálculos das próximas tarefas.

### 🛠️ Calcular estatísticas descritivas

#### Descrição
Calcule medidas de tendência central e dispersão para as notas. Compare os resultados das duas bibliotecas usando a mesma definição estatística: pandas usa `ddof=1` por padrão na variância e no desvio padrão, enquanto NumPy usa `ddof=0`.

#### Requisitos
O programa concluído deve:

- Calcular contagem, média, mediana, mínimo e máximo de `score` com pandas.
- Converter a coluna de notas em um array NumPy com `to_numpy` e calcular média e mediana com NumPy.
- Calcular variância e desvio padrão amostrais nas duas bibliotecas, informando explicitamente `ddof=1`.
- Calcular também variância e desvio padrão populacionais com NumPy, usando `ddof=0`, e identificá-los separadamente na saída.
- Comparar as medidas equivalentes das duas bibliotecas com `numpy.isclose`, sem arredondar os valores antes da comparação.
- Confirmar que a média e a mediana das notas são 75 e que o desvio padrão amostral é aproximadamente 10,80.
- Explicar em uma frase por que os resultados amostrais e populacionais de dispersão são diferentes.

### 🛠️ Comparar grupos e interpretar a correlação

#### Descrição
Resuma as notas por turma e investigue a associação entre horas de estudo e notas. Acrescente ao final de `analysis.py` a impressão de um resumo com pelo menos três frases interpretando os resultados; os dados são sintéticos e não permitem conclusões gerais sobre estudantes reais.

#### Requisitos
O programa concluído deve:

- Usar `groupby` e `agg` para apresentar contagem, média, mediana e desvio padrão amostral das notas por turma.
- Confirmar que a turma A possui 4 registros válidos e a turma B possui 3.
- Calcular a correlação de Pearson entre `study_hours` e `score` com pandas e com `numpy.corrcoef`, extraindo o coeficiente da matriz retornada pelo NumPy.
- Comparar os coeficientes com `numpy.isclose` e confirmar que o resultado é aproximadamente 1,0 para os dados fornecidos.
- Explicar que as turmas têm a mesma média, mas isso não garante distribuições idênticas, considerando também a dispersão e o tamanho dos grupos.
- Explicar que correlação não demonstra causalidade e que uma amostra tão pequena limita a interpretação.
- Exibir medidas numéricas com duas casas decimais somente na apresentação final, preservando a precisão durante os cálculos.