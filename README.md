# Hybrid-MPSI-CoCoSo
Modelo híbrido de análise multicritério (MCDA).

## Descrição
Este projeto implementa um novo framework de decisão multicritério híbrido baseando-se nos métodos **MPSI** e **CoCoSo**. O objetivo é calcular automaticamente os pesos dos critérios similar ao cálculos de pesos do **MPSI** e, em seguida, utilizar esses pesos para ordenar as alternativas de maneira similar ao desenvolvido no método **CoCoSo**.
O maior benefício deste framework é eliminar a subjetividade associada à atribuição de pesos manuais e fornecer uma análise baseada exclusivamente em dados. Além disso, em um único código disponível online, o acesso é facilitado e todos os cálculos são realizados de uma única vez.

### Métodos Utilizados como Referência para o Modelo:
### 1. Matriz Normalizada:
	•	Para critérios de maximização:
$$
r_{ij} = \frac{x_{ij}}{\max(x_{ij})}
$$
	•	Para critérios de minimização:
$$
r_{ij} = \frac{\min(x_{ij})}{x_{ij}}
$$
	2.	Média dos Critérios Normalizados:
$$
v_j = \frac{1}{m} \sum_{i=1}^{m} r_{ij}
$$
	3.	Variância de Preferência:
$$
p_j = \sum_{i=1}^{m} (r_{ij} - v_j)^2
$$
	4.	Cálculo dos Pesos:
$$
w_j = \frac{p_j}{\sum_{j=1}^{n} p_j}
$$

2. Ordenação das Alternativas com CoCoSo

O método CoCoSo utiliza os pesos calculados pelo MPSI para ranquear as alternativas.

	1.	Matriz Normalizada:
	•	Para critérios de maximização:
$$
r_{ij} = \frac{x_{ij} - \min(x_{ij})}{\max(x_{ij}) - \min(x_{ij})}
$$
	•	Para critérios de minimização:
$$
r_{ij} = \frac{\max(x_{ij}) - x_{ij}}{\max(x_{ij}) - \min(x_{ij})}
$$
	2.	Cálculo das Sequências Ponderadas:
	•	Sequência ponderada somatória ($ S_i $):
$$
S_i = \sum_{j=1}^{n} w_j \cdot r_{ij}
$$
	•	Sequência ponderada multiplicativa ($ P_i $):
$$
P_i = \prod_{j=1}^{n} r_{ij}^{w_j}
$$
	3.	Estratégias de Agregação:
	•	Primeira estratégia ($ k_{ia} $):
$$
k_{ia} = \frac{P_i + S_i}{\sum_{i=1}^{m} (P_i + S_i)}
$$
	•	Segunda estratégia ($ k_{ib} $):
$$
k_{ib} = \frac{S_i}{\min(S_i)} + \frac{P_i}{\min(P_i)}
$$
	•	Terceira estratégia ($ k_{ic} $):
$$
k_{ic} = \frac{\lambda \cdot S_i + (1 - \lambda) \cdot P_i}{\lambda \cdot \max(S_i) + (1 - \lambda) \cdot \max(P_i)}
$$
	4.	Valor Agregado Final ($ K_i $):
$$
K_i = \left( k_{ia} \cdot k_{ib} \cdot k_{ic} \right)^{\frac{1}{3}} + \frac{1}{3} \left( k_{ia} + k_{ib} + k_{ic} \right)
$$


## Pré-requisitos
- Biblioteca `pandas`
- Biblioteca `numpy`
- Biblioteca `google.colab` para execução no Google Colab

## Instruções de Uso no Google Colab

### Passos:
1. **Upload do Arquivo**:
   - Ao ser questionado, faça o upload de um arquivo Excel contendo a **matriz de decisão**:
     - Primeira coluna: Alternativas.
     - Colunas seguintes: Critérios.
     - Primeira linha: Títulos das colunas.
     - Números com casas decimais separadas com ponto (e não vírgula).

2. **Definição dos Tipos de Critério**:
   - O usuário informa se cada critério é de **maximização** ou **minimização**.
   - Exemplo: `max, min, max, ...`.

3. **Definir o Valor de \( \lambda \)**:
   - Ao ser questionado, digite um valor entre 0.1 e 1 para controlar o balanceamento das estratégias de agregação no CoCoSo.

4. **Cálculo e Ranqueamento**:
   - O algoritmo usa o MPSI para calcular os pesos e, em seguida, aplica o CoCoSo para ordenar as alternativas.

5. **Visualização dos Resultados**:
   - Ao ser questionado, escolha se deseja visualizar as alternativas em ordem **crescente** (1º, 2º, 3º, ...) ou **decrescente** (..., 3º, 2º, 1º).

6. **Resultados**:
   - O programa exibe os pesos calculados para cada critério e a lista das alternativas com o valor agregado \( K_i \).

## Exemplo de Uso
Considere um arquivo Excel com a seguinte matriz de decisão:

| Alternativas | Custo   | Qualidade | Tempo de Resposta | Reputação  |
|--------------|---------|-----------|-------------------|------------|
| Fornecedor A | 1.5     | 4         | 10                | 5          |
| Fornecedor B | 2       | 5         | 15                | 4          |
| Fornecedor C | 1.2     | 3         | 8                 | 3          |

1. **Upload**: Faça o upload do arquivo Excel.
2. **Definir Critérios**: `min, max, min, max`.
3. **Valor de \( \lambda \)**: Defina, por exemplo, `0.5`.
4. **Ordem de Visualização**: Escolha `crescente` ou `decrescente`.
5. **Resultados**: A lista será gerada conforme a escolha de visualização.

## Sugestões para Expansão
- Implementar interface gráfica a partir de um arquivo executável (.exe).
- Gerar relatórios automáticos com a memória de cálculo em PDF.
- Adicionar gráficos e análises de sensibilidade.

1. Cálculo dos Pesos com MPSI

O método MPSI é utilizado para calcular automaticamente os pesos dos critérios com base na variância de preferência. Os cálculos são realizados da seguinte forma:

	1.	Matriz Normalizada:
	•	Para critérios de maximização:

r_{ij} = \frac{x_{ij}}{\max(x_{ij})}

	•	Para critérios de minimização:

r_{ij} = \frac{\min(x_{ij})}{x_{ij}}

	2.	Média dos Critérios Normalizados:

v_j = \frac{1}{m} \sum_{i=1}^{m} r_{ij}

	3.	Variância de Preferência:

p_j = \sum_{i=1}^{m} (r_{ij} - v_j)^2

	4.	Cálculo dos Pesos:

w_j = \frac{p_j}{\sum_{j=1}^{n} p_j}


2. Ordenação das Alternativas com CoCoSo

O método CoCoSo utiliza os pesos calculados no MPSI para calcular os valores de  K_i  e ordenar as alternativas com base em diferentes estratégias de agregação.

	1.	Matriz Normalizada (CoCoSo):
	•	Para critérios de maximização:

r_{ij} = \frac{x_{ij} - \min(x_{ij})}{\max(x_{ij}) - \min(x_{ij})}

	•	Para critérios de minimização:

r_{ij} = \frac{\max(x_{ij}) - x_{ij}}{\max(x_{ij}) - \min(x_{ij})}

	2.	Cálculo das Sequências Ponderadas:
	•	Sequência ponderada somatória ( S_i ):

S_i = \sum_{j=1}^{n} w_j \cdot r_{ij}

	•	Sequência ponderada multiplicativa ( P_i ):

P_i = \prod_{j=1}^{n} r_{ij}^{w_j}

	3.	Estratégias de Agregação:
	•	Primeira estratégia ( k_{ia} ):

k_{ia} = \frac{P_i + S_i}{\sum_{i=1}^{m} (P_i + S_i)}

	•	Segunda estratégia ( k_{ib} ):

k_{ib} = \frac{S_i}{\min(S_i)} + \frac{P_i}{\min(P_i)}

	•	Terceira estratégia ( k_{ic} ):

k_{ic} = \frac{\lambda \cdot S_i + (1 - \lambda) \cdot P_i}{\lambda \cdot \max(S_i) + (1 - \lambda) \cdot \max(P_i)}

	4.	Valor Agregado Final ( K_i ):

K_i = \left( k_{ia} \cdot k_{ib} \cdot k_{ic} \right)^{\frac{1}{3}} + \frac{1}{3} \left( k_{ia} + k_{ib} + k_{ic} \right)




---
