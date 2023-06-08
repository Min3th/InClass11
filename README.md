#include <iostream>
#include <limits>

constexpr int kNumVertices = 6;

int minKey(int key[], bool mstSet[]) {
    int min_val = std::numeric_limits<int>::max();
    size_t min_index = 0;

    for (size_t v = 0; v < kNumVertices; ++v) {
        if (!mstSet[v] && key[v] < min_val) {
            min_val = key[v];
            min_index = v;
        }
    }

    return min_index;
}

void printMST(int parent[], int graph[kNumVertices][kNumVertices]) {
    std::cout << "Edge \tWeight\n";
    for (size_t i = 1; i < kNumVertices; ++i) {
        std::cout << parent[i] << " - " << i << " \t"
                  << graph[i][parent[i]] << " \n";
    }
}

int calculateMST(int parent[], int graph[kNumVertices][kNumVertices]) {
    int total_cost = 0;

    for (size_t i = 1; i < kNumVertices; ++i) {
        total_cost += graph[i][parent[i]];
    }

    return total_cost;
}

int primMST(int graph[kNumVertices][kNumVertices]) {
    int parent[kNumVertices];
    int key[kNumVertices];
    bool mstSet[kNumVertices] = {false};

    for (size_t i = 0; i < kNumVertices; ++i) {
        key[i] = std::numeric_limits<int>::max();
    }

    key[0] = 0;
    parent[0] = -1;

    for (size_t count = 0; count < kNumVertices - 1; ++count) {
        const int u = minKey(key, mstSet);
        mstSet[u] = true;

        for (size_t v = 0; v < kNumVertices; ++v) {
            if (graph[u][v] && !mstSet[v] && graph[u][v] < key[v]) {
                parent[v] = u;
                key[v] = graph[u][v];
            }
        }
    }

    // printMST(parent, graph);
    return calculateMST(parent, graph);
}

int main() {
    int graph[kNumVertices][kNumVertices] = {{0, 3, 0, 0, 0, 1},
                                             {3, 0, 2, 1, 10, 0},
                                             {0, 2, 0, 3, 0, 5},
                                             {0, 1, 3, 0, 5, 0},
                                             {0, 10, 0, 5, 0, 4},
                                             {1, 0, 5, 0, 4, 0}};

    const int total_cost = primMST(graph);
    std::cout << "Total cost of MST: " << total_cost << "\n";

    return 0;
}
