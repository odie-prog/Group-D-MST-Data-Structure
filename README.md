# Group D MST - Data Structure

|    NRP     |           Nama             |
| :--------: |       :-------------------:       |
| 5025251259 | A Deraya Meuthia Toja                   |
| 5025251168 | Lina Fatima Azzahra Badr                   |
| 5025251160 | Vania Aisha Rochmawati                    |
| 5025251258 | Naila Sa'ada Cahyani                |

# SPOJ - Cobbled Streets
https://spoj.com/problems/CSTREET/

``` C
#include <stdio.h>
#include <stdlib.h>

struct edge
{
    int a; // starting building
    int b; // ending building
    long long c; // length of street
};

struct edge edges[300005]; // global array to prevent stack overflow
int parent[1005];          // since m <= 300000 n <= 1000

// comparator function to help qsort sort edges by c length
int compare(const void *x, const void *y)
{
    struct edge *e1 = (struct edge *)x;
    struct edge *e2 = (struct edge *)y;
    if (e1->c < e2->c) return -1;
    if (e1->c > e2->c) return 1;
    return 0;
}

// DSU FIND function with path compression for fast lookups
int find (int i)
{
    if (parent[i] == i)
        return i;
    // implement path compression by attaching node i directly to the root
    return parent[i] = find(parent[i]);
}

int main()
{
    int t;
    if (scanf("%d", &t) != 1) return 0;

    // process each testcase
    while (t--)
    {
        long long p; // price to pave 1 furlong
        int n; // number of main buildings
        int m; // number of streets

        scanf("%lld", &p);
        scanf("%d", &n);
        scanf("%d", &m);

        // for all m streets
        for (int i = 0; i < m; i++)
        {
            scanf("%d %d %lld", &edges[i].a, &edges[i].b, &edges[i].c);
        }

        // 1. sort all streets by their c lengths
        qsort(edges, m, sizeof(struct edge), compare);

        // 2. initialize parent array for DSU
        // start with every building being its own separate tree
        for (int i = 1; i<= n; i++)
        {
            parent[i] = i;
        }

        long long mst_len = 0;
        int edges_used = 0;

        // 3. kruskal's algorithm
        for (int i = 0; i < m; i++)
        {
            // variables for each building's roots
            int root_a = find(edges[i].a);
            int root_b = find(edges[i].b);

            // determine their roots are different
            if (root_a != root_b)
            {
                // connect them if they are
                parent[root_a] = root_b;

                // add street length to the total MST length
                mst_len += edges[i].c;
                edges_used++;

                // knowing that a spanning tree of n nodes always has exactly n-1 edges,
                // we can stop once we reach n-1 edges
                if (edges_used == n - 1)
                {
                    break;
                }
            }
        }

        // 4. print smallest cost
        long long cost_total = mst_len * p;
        printf("%lld\n", cost_total);
    }

    return 0;
}
```
