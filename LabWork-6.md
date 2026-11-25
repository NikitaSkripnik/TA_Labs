## Дейкстра

```csharp

using System;
using System.Collections.Generic;

class Edge
{
    public int Vertex;
    public int Weight;

    public Edge(int vertex, int weight)
    {
        Vertex = vertex;
        Weight = weight;
    }
}

class Program
{
    static void Dijkstra(Dictionary<int, List<Edge>> graph, int start)
    {
        int n = graph.Count;

        int[] pred = new int[n];
        int[] dist = new int[n];
        bool[] visited = new bool[n];

        for (int i = 0; i < n; i++)
        {
            pred[i] = -1;
            dist[i] = int.MaxValue;
            visited[i] = false;
        }

        dist[start] = 0;

        for (int i = 0; i < n; i++)
        {
            int u = -1;

            for (int j = 0; j < n; j++)
            {
                if (!visited[j] &&
                    (u == -1 || dist[j] < dist[u]))
                {
                    u = j;
                }
            }

            if (u == -1 || dist[u] == int.MaxValue)
                break;

            visited[u] = true;

            foreach (Edge edge in graph[u])
            {
                int v = edge.Vertex;
                int weight = edge.Weight;

                if (!visited[v] && dist[v] > dist[u] + weight)
                {
                    dist[v] = dist[u] + weight;
                    pred[v] = u;
                }
            }
        }

        Console.WriteLine("Найкоротші шляхи від вершини " + start + ":\n");

        for (int v = 0; v < n; v++)
        {
            Console.Write("До вершини " + v + ": ");

            if (dist[v] == int.MaxValue)
            {
                Console.WriteLine("Шляху не існує");
                continue;
            }

            List<int> path = new List<int>();
            int current = v;

            while (current != -1)
            {
                path.Add(current);
                current = pred[current];
            }

            path.Reverse();

            Console.WriteLine(
                string.Join(" -> ", path) +
                " | Довжина: " + dist[v]);
        }
    }

    static void Main()
    {
        Dictionary<int, List<Edge>> graph =
            new Dictionary<int, List<Edge>>();

        graph[0] = new List<Edge>
        {
            new Edge(1, 6),
            new Edge(2, 2),
            new Edge(3, 4)
        };

        graph[1] = new List<Edge>
        {
            new Edge(0, 6),
            new Edge(3, 6),
            new Edge(5, 6)
        };

        graph[2] = new List<Edge>
        {
            new Edge(0, 2),
            new Edge(7, 4)
        };

        graph[3] = new List<Edge>
        {
            new Edge(0, 4),
            new Edge(1, 6),
            new Edge(4, 1)
        };

        graph[4] = new List<Edge>
        {
            new Edge(3, 1),
            new Edge(5, 4),
            new Edge(6, 7)
        };

        graph[5] = new List<Edge>
        {
            new Edge(4, 4),
            new Edge(1, 6),
            new Edge(6, 5),
            new Edge(7, 3)
        };

        graph[6] = new List<Edge>
        {
            new Edge(4, 7),
            new Edge(5, 5),
            new Edge(7, 5)
        };

        graph[7] = new List<Edge>
        {
            new Edge(2, 4),
            new Edge(5, 3),
            new Edge(6, 5)
        };

        int start = 0;

        Dijkstra(graph, start);
    }
}

```

## Флойд

```csharp

using System;

class Program
{
    static void Floyd(int[,] W)
    {
        int n = W.GetLength(0);
        int[,] D = new int[n, n];

        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < n; j++)
            {
                D[i, j] = W[i, j];
            }
        }

        for (int k = 0; k < n; k++)
        {
            for (int i = 0; i < n; i++)
            {
                for (int j = 0; j < n; j++)
                {
                    if (D[i, k] != int.MaxValue &&
                        D[k, j] != int.MaxValue &&
                        D[i, j] > D[i, k] + D[k, j])
                    {
                        D[i, j] = D[i, k] + D[k, j];
                    }
                }
            }
        }

        Console.WriteLine("Матриця найкоротших шляхів:\n");

        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < n; j++)
            {
                if (D[i, j] == int.MaxValue)
                    Console.Write("INF\t");
                else
                    Console.Write(D[i, j] + "\t");
            }

            Console.WriteLine();
        }
    }

    static void Main()
    {
        int INF = int.MaxValue;

        int[,] W =
        {
            { 0,   6,   2,   4,   INF, INF, INF, INF },
            { 6,   0,   INF, 6,   INF, 6,   INF, INF },
            { 2,   INF, 0,   INF, INF, INF, INF, 4   },
            { 4,   6,   INF, 0,   1,   INF, INF, INF },
            { INF, INF, INF, 1,   0,   4,   7,   INF },
            { INF, 6,   INF, INF, 4,   0,   5,   3   },
            { INF, INF, INF, INF, 7,   5,   0,   5   },
            { INF, INF, 4,   INF, INF, 3,   5,   0   }
        };

        Floyd(W);
    }
}
```
