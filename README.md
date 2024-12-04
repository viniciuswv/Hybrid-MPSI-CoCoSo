# Hybrid-MPSI-CoCoSo: Framework híbrido para apoio à decisão multicritério
O Hybrid MPSI-CoCoSo é um modelo híbrido para análise de decisão multicritério (MCDA) em dois estágios, desenvolvido no âmbito do curso de Doutorado Profissional do Programa de Pós-graduação em Estudos Marítimos (Escola de Guerra Naval - Marinha do Brasil).

## Descrição
Este projeto implementa um novo framework híbrido de decisão multicritério, baseando-se nos métodos **Modified Preference Selection Index, MPSI (Gligorić et al., 2022)** e **Combined Compromise Solution, CoCoSo (Yazdani et al., 2018)**. O objetivo é calcular, automaticamente, os pesos (importância) de cada critério de maneira similar ao apresentado no método **MPSI** e, em seguida, utilizar tais pesos para ranquear as alternativas de maneira similar ao desenvolvido no método **CoCoSo**.
O maior benefício deste framework é eliminar a subjetividade associada à atribuição manual de pesos (ou facilitar, no caso da ausência de especialistas) e fornecer uma análise baseada exclusivamente em dados. Além disso, ao ser disponibilizado em um único código online, o framework democratiza o acesso e permite que os cálculos sejam realizados de forma integrada, combinando os dois modelos de decisão em um único processo.



## Pré-requisitos
- Matriz de Decisão em arquivo Excel formato ".xslx", contendo na 1ª coluna as alternativas e, nas colunas seguintes, os critérios. A 1ª linha da matriz deverá conter os títulos, por exemplo, "Alternativas" como título da 1ª coluna e "Critério a", "Critério b", etc, para os títulos das colunas seguintes. Para valores com casas decimais, garanta que o separador seja ponto "." e não vírgula ",")
- Biblioteca `pandas`
- Biblioteca `numpy`
- Biblioteca `google.colab` para execução no Google Colab

Nota: As bibliotecas são carregadas automaticamente ao executar o código.



## Instruções de Uso no Google Colab

### Passos:

Abra o framework através do link do Google Colab disponível no repositório do programa. Em seguida, certifique-se de que esteja conectado à máquina virtual clicando no botão "Conectar", disponível na parte superior direita do IDE. Uma vez conectado, execute a linha de código. Após executar, role a tela para baixo e perceba que o programa lhe solicitará algumas informações, conforme o passo-a-passo abaixo descrito:

1. **Upload do Arquivo**:
   - Ao ser solicitado, faça o upload de um arquivo Excel contendo a **matriz de decisão**:
     - Primeira coluna: Alternativas.
     - Colunas seguintes: Critérios.
     - Primeira linha: Títulos das colunas.
     - Números com casas decimais separadas com ponto (e não vírgula).

2. **Definição dos Tipos de Critério**:
   - Ao ser solicitado, informe se cada critério é de **maximização** ou **minimização**.
   - Exemplo: `max, min, max, ...`.

3. **Definir o Valor de $\lambda$**:
   - Ao ser solicitado, digite um valor entre `0.1` e `1` para controlar o balanceamento das estratégias de agregação (Nota: use "ponto" para designar notação decimal).

4. **Cálculo e Ranqueamento**:
   - O programa calcula os pesos e, em seguida, ranqueia as alternativas.

5. **Visualização dos Resultados**:
   - Ao ser solicitado, escolha se deseja visualizar as alternativas em ordem crescente (digite `crescente` para 1º, 2º, 3º, ...) ou decrescente (digite `decrescente` para ..., 3º, 2º, 1º).

6. **Resultados**:
   - O programa exibe os pesos calculados para cada critério bem como a lista das alternativas ranqueadas.



## Exemplo de Uso
Considere um arquivo Excel com a seguinte matriz de decisão:

| Alternativas | Custo   | Qualidade | Tempo de Resposta | Reputação  |
|--------------|---------|-----------|-------------------|------------|
| Fornecedor A | 1.5     | 4         | 10                | 5          |
| Fornecedor B | 2       | 5         | 15                | 4          |
| Fornecedor C | 1.2     | 3         | 8                 | 3          |

1. **Upload**: Faça o upload do arquivo Excel.
2. **Definir Critérios**: `min, max, min, max`.
3. **Valor de $\lambda$**: Defina, por exemplo, `0.5`.
4. **Ordem de Visualização**: Escolha `crescente` ou `decrescente`.
5. **Resultados**: A lista será gerada conforme a escolha de visualização.



## Como o Programa Funciona?

### Cálculo dos Pesos com o Método MPSI (1º estágio)
Em um modelo de análise multicritério, os pesos são valores atribuídos a diferentes critérios para indicar sua importância relativa. Eles ajudam a determinar quais critérios são mais relevantes na decisão final, permitindo que o modelo priorize os aspectos mais importantes ao comparar opções. O método MPSI é interessante pois baseia-se no grau de oscilação, ou seja, na variação do valor de preferência para cada critério. Essa variação apresenta efetivamente a distância entre o valor normalizado e o valor médio por critério e é expressa através de uma distância euclidiana.

O passo-a-passo do estabelecimento automático dos pesos está descrito a seguir.

#### 1. Normalização da matriz de decisão:
Uma matriz de decisão é uma tabela que organiza as opções a serem comparadas (linhas) e os critérios usados para avaliá-las (colunas). Cada célula mostra o valor ou desempenho de uma opção em relação a um critério específico. Ela precisa ser normalizada para padronizar as diferentes escalas de cada critério, garantindo que todos sejam comparados de maneira justa e consistente.

Normalização para critérios de maximização:

$$
r_{ij} = \frac{x_{ij}}{\max(x_{ij})}
$$

Normalização para critérios de minimização:

$$
r_{ij} = \frac{\min(x_{ij})}{x_{ij}}
$$

#### 2. Cálculo da média dos critérios normalizados:
$$
v_j = \frac{1}{m} \sum_{i=1}^{m} r_{ij}
$$
	
#### 3. Cálculo da variância de preferência:
$$
p_j = \sum_{i=1}^{m} (r_{ij} - v_j)^2
$$
	
#### 4.Cálculo dos pesos:
$$
w_j = \frac{p_j}{\sum_{j=1}^{n} p_j}
$$

### Ordenação das alternativas com o Método CoCoSo (2º estágio)
O próximo estágio baseia-se no método CoCoSo, o qual faz os cálculos comparando diferentes opções com base nos critérios organizados na Matriz de Decisão. Ele utiliza os pesos definidos no 1º estágio fazendo uma combinação deles com as pontuações das opções para cada critério, gerando uma pontuação final que auxilia o "tomador de decisão" a identificar a melhor escolha.

Este método é interessante pois incorpora cálculos de múltiplos valores de agregação, visando identificar o compromisso que um critério tem em função dos demais, diferenciando o método por sua capacidade de balanceamento e equilíbrio no processo decisório.


#### 1.	Normalização da matriz de decisão:

Para critérios de maximização:

$$
r_{ij} = \frac{x_{ij} - \min(x_{ij})}{\max(x_{ij}) - \min(x_{ij})}
$$

Para critérios de minimização:

$$
r_{ij} = \frac{\max(x_{ij}) - x_{ij}}{\max(x_{ij}) - \min(x_{ij})}
$$

#### 2. Cálculo das Sequências Ponderadas:

Aqui são utilizados os valores dos pesos de cada alternativa, calculados no 1º estágio.


Sequência ponderada somatória ($S_i$):

$$
S_i = \sum_{j=1}^{n} w_j \cdot r_{ij}
$$
	
Sequência ponderada multiplicativa ($P_i$):

$$
P_i = \sum_{j=1}^{n} r_{ij}^{w_j}
$$
	
#### 3. Cálculo das Estratégias de Agregação:

Primeira estratégia ($k_{ia}$):

$$
k_{ia} = \frac{P_i + S_i}{\sum_{i=1}^{m} (P_i + S_i)}
$$

Segunda estratégia ($k_{ib}$):

$$
k_{ib} = \frac{S_i}{\min(S_i)} + \frac{P_i}{\min(P_i)}
$$

Terceira estratégia ($k_{ic}$):

$$
k_{ic} = \frac{\lambda \cdot S_i + (1 - \lambda) \cdot P_i}{\lambda \cdot \max(S_i) + (1 - \lambda) \cdot \max(P_i)}
$$

Nota: Convém observar a utilizanção de uma nova variável ($\lambda$) nesta terceira Estratégia de Agregação. De acordo com (Yazdani et al., 2018), ela desempenha um papel de balanceamento do cálculo de agregação, atribuindo maior flexibilidade ao modelo e permitido que o usuário teste seu comportamento em diferentes cenários (dados). Considere 0 $\geq$ $\lambda$ $\geq$ 1.

#### 4. Cálculo do Valor Agregado Final ($K_i$):
Aqui é o final do modelo multicritério. O cálculo do Valor Agregado Final considera os valores obtidos pelas três Estratégias de Agregação e fornece um valor final para cada alternativa. Quanto maior o valor, melhor será a posição obtida pelo critério.

$$
K_i = \left( k_{ia} \cdot k_{ib} \cdot k_{ic} \right)^{\frac{1}{3}} + \frac{1}{3} \left( k_{ia} + k_{ib} + k_{ic} \right)
$$



## Implementações Futuras
- Interface gráfica a partir de um arquivo executável (.exe).
- Geração de relatórios automáticos com a memória de cálculo em PDF.
- Geração de gráficos e comparação visual de análise de sensibilidade.



## Referências
Gligorić, M., Gligorić, Z., Lutovac, S., Negovanović, M., & Langović, Z. (2022). **Novel Hybrid MPSI–MARA Decision-Making Model for Support System Selection in an Underground Mine**. Systems, 10(6), Artigo 6. https://doi.org/10.3390/systems10060248

Yazdani, M., Zarate, P., Kazimieras Zavadskas, E., & Turskis, Z. (2018). **A combined compromise solution (CoCoSo) method for multi-criteria decision-making problems**. Management Decision, 57(9), 2501–2519. https://doi.org/10.1108/MD-05-2017-0458


## Como citar o Programa
VIANNA, Vinicius; LAURO, Adriano. Hybrid-MPSI-CoCoSo: framework híbrido para apoio à decisão multicritério. 2024. Disponível em: https://github.com/viniciuswv/Hybrid-MPSI-CoCoSo. Acesso em: dia mês. ano.

---
