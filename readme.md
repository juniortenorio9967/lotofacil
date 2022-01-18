LOTOFÁCIL - Estudo de repetição em concursos e geração de 15 números aleatórios

Objetivo:

Código Python para análise dos resultados da Lotofácil, verificando a existência de repetição de números em qualquer concurso,
e na sequência, permite ao usuário gerar números aleatórios (randômicos), em seguida escolher a quantidade de números
pares/ímpares, onde pares + ímpares some 15, finalizando com a geração dos 15 números ordenados. Após a geração, o sistema compara
o jogo gerado com a base de concursos do arquivo "lotofacil.xlsx", de maneira que se os números gerados pelo software, por um acaso
já existam, o usuário poderá gerar outro jogo, evitando assim, a repetição.

Utiliza-se para isto a base de dados de concursos do site oficial das loterias (este código é um estudo, e o resultado gerado NÃO é recomendação de jogo),
sequenciado no arquivo "lotofacil.xlsx", atualizado em 17/01/2022.

Para executar os códigos Python, foi utilizado aplicativo web Jupyter Notebook, que faz parte do pacote de desenvolvimento Python ANACONDA,
onde também estão incluídas várias bibliotecas incluindo as que são utilizadas nesse código, como Pandas e Numpy.
O Jupyter Notebook é como um bloco de notas, onde o desenvolvedor pode executar trechos curtos e corrigir erros rapidamente, o que acelera o desenvolvimento,
entretanto, o código abaixo poderá ser colado integralmente de uma só vez.


#Importar libs e base de dados

import pandas as pd
import numpy as np
import math, random

loto = pd.read_excel("lotofacil.xlsx")

# descarta linhas "missing value"
loto.dropna(subset=["Concurso"], inplace=True)

# Extrai sub-conjunto contendo apenas a bolas sorteadas
sobolas = loto[['Bola1', 'Bola2', 'Bola3', 'Bola4', 'Bola5', 'Bola6','Bola7', 'Bola8', 'Bola9','Bola10', 'Bola11', 'Bola12','Bola13', 'Bola14', 'Bola15']]


# Gera grupos de numeros pares e ímpares

def gerarImpar():
    num = 2
    while num % 2 == 0:
        num = math.ceil(25 * random.random())

    return num

def gerarPar():
    num = 3
    while num % 2 != 0:
        num = math.ceil(25 * random.random())

    return num

def gerarListaImpares(qtde):
       
    imparesGerados = []
    while len(imparesGerados) < qtde:
        while True:
            x = gerarImpar()
            if imparesGerados.count(x) == 0: #verifica se há ocorrencia x na lista
                imparesGerados.append(x)     # não existindo, adiciona x à lista
                break                        # e sai do while true
            
    return sorted(imparesGerados)

def gerarListaPares(qtde):
    
    paresGerados = []
    while len(paresGerados) < qtde:
        while True:
            x = gerarPar()
            if paresGerados.count(x) == 0: #verifica se há ocorrencia x na lista
                paresGerados.append(x)     # não existindo, adiciona x à lista
                break
                
    return sorted(paresGerados)

# o usuário escolhe a quantidade de pares e ímpares
# e o sistema gera um conjunto de 15 números

volanteGerado = sorted(gerarListaImpares(8) + gerarListaPares(7))


# Verifica se o volante gerado já existe na base de dados,
# evitando que o usuário repita um jogo existente

meujogo = np.asarray(volanteGerado)
print('meu jogo convertido para numpy')
print(meujogo)
qtde_linhas = len(sobolas)

for i in range(qtde_linhas):
    origin = i + 1 #contador
    origem = np.asarray(sobolas.iloc[i]) #array numpy
    result = np.array_equal(origem, meujogo)
    if result == True:
        print(result)
        print("Duplicidade encontrada:")
        print(origin)
        print(origem)
        print("Seu jogo:")
        print(meujogo)

if result == False:
    print("Nenhuma duplicidade encontrada")
    print(volanteGerado)
