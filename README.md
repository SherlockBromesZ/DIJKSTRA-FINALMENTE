# Algoritmo de Dijkstra em C++

Este repositório contém uma implementação simples do algoritmo de Dijkstra em C++. O algoritmo de Dijkstra é usado para encontrar o caminho mais curto entre um nó de origem e todos os outros nós em um grafo ponderado.

## Como funciona o algoritmo

1. **Inicialização**:
   - Definimos um vetor `distanciaMinima` para armazenar a menor distância de cada nó a partir do nó de origem.
   - Inicializamos todas as distâncias como "infinito" (`numeric_limits<int>::max()`), exceto a distância do nó de origem, que é 0.
   - Usamos uma fila de prioridade (`priority_queue`) para processar os nós com a menor distância conhecida.

2. **Processamento**:
   - Enquanto a fila de prioridade não estiver vazia:
     - Extraímos o nó com a menor distância conhecida.
     - Para cada vizinho do nó atual, verificamos se a distância através do nó atual é menor do que a distância conhecida anteriormente.
     - Se for menor, atualizamos a distância e adicionamos o vizinho à fila de prioridade.

3. **Resultado**:
   - Após processar todos os nós, o vetor `distanciaMinima` conterá a menor distância do nó de origem para cada nó no grafo.

## Código

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <limits>

using namespace std;

const int NUM_NODES = 10;  // Número de nós no grafo

vector<pair<int, int>> adjacencias[NUM_NODES];

vector<int> dijkstra(int noOrigem) {
  vector<int> distanciaMinima(NUM_NODES, numeric_limits<int>::max());
  priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> filaPrioridade;

  distanciaMinima[noOrigem] = 0;
  filaPrioridade.push({0, noOrigem});

  while (!filaPrioridade.empty()) {
    int noAtual = filaPrioridade.top().second;
    filaPrioridade.pop();

    // Itera sobre os vizinhos do nó atual
    for (auto [noVizinho, pesoAresta] : adjacencias[noAtual]) {
      if (distanciaMinima[noAtual] + pesoAresta < distanciaMinima[noVizinho]) {
        distanciaMinima[noVizinho] = distanciaMinima[noAtual] + pesoAresta;
        filaPrioridade.push({distanciaMinima[noVizinho], noVizinho});
      }
    }
  }

  return distanciaMinima;
}

int main() {
  // Construir o grafo com exemplo de entrada
  adjacencias[0].push_back({1, 4});
  adjacencias[0].push_back({7, 8});
  adjacencias[1].push_back({2, 8});
  adjacencias[1].push_back({7, 11});
  adjacencias[2].push_back({3, 7});
  adjacencias[2].push_back({8, 2});
  adjacencias[2].push_back({5, 4});
  adjacencias[3].push_back({4, 9});
  adjacencias[3].push_back({5, 14});
  adjacencias[4].push_back({5, 10});
  adjacencias[5].push_back({6, 2});
  adjacencias[6].push_back({7, 1});
  adjacencias[6].push_back({8, 6});
  adjacencias[7].push_back({8, 7});

  // Calcular as menores distâncias a partir do nó 0
  vector<int> distanciaMinima = dijkstra(0);

  // Imprimir as menores distâncias
  for (int i = 0; i < NUM_NODES; ++i) {
    if (distanciaMinima[i] == numeric_limits<int>::max()) {
      cout << "Distância de 0 a " << i << ": Infinito" << endl;
    } else {
      cout << "Distância de 0 a " << i << ": " << distanciaMinima[i] << endl;
    }
  }

  return 0;
}
```

## Explicação do Código

- **Bibliotecas Importadas**:
  - `iostream`: Para entrada e saída de dados.
  - `vector`: Para armazenar as listas de adjacências.
  - `queue`: Para a fila de prioridade.
  - `limits`: Para definir o valor "infinito".

- **Constantes e Variáveis Globais**:
  - `NUM_NODES`: Número de nós no grafo.
  - `adjacencias`: Lista de adjacências para representar o grafo.

- **Função `dijkstra`**:
  - Recebe o nó de origem como argumento.
  - Inicializa a fila de prioridade e o vetor `distanciaMinima`.
  - Processa os nós usando a fila de prioridade para encontrar os caminhos mais curtos.

- **Função `main`**:
  - Constrói o grafo adicionando arestas à lista de adjacências.
  - Chama a função `dijkstra` para calcular as menores distâncias a partir do nó 0.
  - Imprime as menores distâncias para cada nó.

## Como Executar

1. **Compilar**:
   - Use um compilador C++ como `g++` para compilar o código:
   ```bash
   g++ -o dijkstra dijkstra.cpp
   ```

2. **Executar**:
   - Execute o programa compilado:
   ```bash
   ./dijkstra
   ```

## Exemplo de Saída

```bash
Distância de 0 a 0: 0
Distância de 0 a 1: 4
Distância de 0 a 2: 12
Distância de 0 a 3: 19
Distância de 0 a 4: 28
Distância de 0 a 5: 16
Distância de 0 a 6: 18
Distância de 0 a 7: 8
Distância de 0 a 8: 14
Distância de 0 a 9: Infinito
```
