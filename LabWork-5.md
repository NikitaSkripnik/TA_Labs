Код алгоритму Прима
```c++
#include <iostream>
#include <cstring>
using namespace std;

#define INF 9999999
#define V 8

int G[V][V] = {
    {INF, 6,   2,   4,   INF, INF, INF, INF},
    {6,   INF, INF, 6,   INF, 6,   INF, INF},
    {2,   INF, INF, INF, INF, INF, INF, 4},
    {4,   6,   INF, INF, 1,   INF, INF, INF},
    {INF, INF, INF, 1,   INF, 4,   7,   INF},
    {INF, 6,   INF, INF, 4,   INF, 5,   3},
    {INF, INF, INF, INF, 7,   5,   INF, 5},
    {INF, INF, 4,   INF, INF, 3,   5,   INF}
};

int main() {
    int no_edge;
    int selected[V];

    memset(selected, false, sizeof(selected));

    no_edge = 0;

    selected[0] = true;

    int x;
    int y;

    cout << "Edge : Weight" << endl;

    while (no_edge < V - 1) {
        int min = INF;
        x = 0;
        y = 0;

        for (int i = 0; i < V; i++) {
            if (selected[i]) {
                for (int j = 0; j < V; j++) {
                    if (!selected[j] && G[i][j] != INF) {
                        if (min > G[i][j]) {
                            min = G[i][j];
                            x = i;
                            y = j;
                        }
                    }
                }
            }
        }

        cout << x + 1 << " - " << y + 1 << " : " << G[x][y] << endl;

        selected[y] = true;
        no_edge++;
    }

    return 0;
}
```

Код алгоритму Крускала
```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

#define edge pair<int,int>

class Graph {
private:
    vector<pair<int, edge>> G;
    vector<pair<int, edge>> T;
    int *parent;
    int V;

public:
    Graph(int V);
    ~Graph();
    void AddWeightedEdge(int u, int v, int w);
    int find_set(int i);
    void union_set(int u, int v);
    void kruskal();
    void print();
};

Graph::Graph(int V) {
    this->V = V;
    parent = new int[V];

    for (int i = 0; i < V; i++)
        parent[i] = i;

    G.clear();
    T.clear();
}

Graph::~Graph() {
    delete[] parent;
}

void Graph::AddWeightedEdge(int u, int v, int w) {
    G.push_back(make_pair(w, edge(u, v)));
}

int Graph::find_set(int i) {
    if (i == parent[i])
        return i;
    else
        return find_set(parent[i]);
}

void Graph::union_set(int u, int v) {
    parent[u] = parent[v];
}

void Graph::kruskal() {
    int i, uRep, vRep;

    sort(G.begin(), G.end());

    for (i = 0; i < G.size(); i++) {
        uRep = find_set(G[i].second.first);
        vRep = find_set(G[i].second.second);

        if (uRep != vRep) {
            T.push_back(G[i]);
            union_set(uRep, vRep);
        }

        if (T.size() == V - 1)
            break;
    }
}

void Graph::print() {
    cout << "Edge : Weight" << endl;

    for (int i = 0; i < T.size(); i++) {
        cout << T[i].second.first + 1 << " - "
             << T[i].second.second + 1 << " : "
             << T[i].first << endl;
    }
}

int main() {
    Graph g(8);

    g.AddWeightedEdge(0, 2, 2);
    g.AddWeightedEdge(2, 7, 4);
    g.AddWeightedEdge(0, 1, 6);
    g.AddWeightedEdge(0, 3, 4);
    g.AddWeightedEdge(1, 3, 6);
    g.AddWeightedEdge(1, 5, 6);
    g.AddWeightedEdge(5, 7, 3);
    g.AddWeightedEdge(5, 4, 4);
    g.AddWeightedEdge(5, 6, 5);
    g.AddWeightedEdge(7, 6, 5);
    g.AddWeightedEdge(3, 4, 1);
    g.AddWeightedEdge(4, 6, 7);

    g.kruskal();
    g.print();

    return 0;
}
```
