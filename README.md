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

# Pseudocode
```
define a struct to represent a street
{
    // store the connection details:
    // - building a
    // - building b
    // - length c
}
create global arrays to store the list of streets and the parent of each building

function to compare two streets for sorting
{
    // check two streets: e1 and e2
    // if e1 has a smaller length c than e2, return -1 (comes first)
    // if e1 has a larger length c than e2, return 1 (comes later)
    // otherwise, they have equal lengths, return 0
}

function to find the root representative of a building
{
    // if a building is its own parent, it is the root, so return it
    // path compression: find the root, point the building directly to it, and return it
}

main entry point of the program
{
    // read total number of testcases t
    // if reading fails, exit program

    loop through each testcase until none are left
    {
        // setup variables for price p, buildings n, and streets m
        // read input data values for price p, buildings n, and streets m

        loop through every single street from index 0 up to m - 1
        {
            // read and store building a, building b, and length c for the current street
        }

        // sort all elements in the streets array by length c using the compare function

        loop through every single building from index 1 up to n
        {
            // set each building to be its own parent initially (separate sets)
        }

        // initialize total street length and used street count to 0

        loop through the sorted list of streets one by one
        {
            // find the root of building a
            // find the root of building b

            if the root of building a is NOT the same as the root of building b
            {
                // union operation: point the root of building a to the root of building b

                // add the current length c to the total street length
                // increment the count of used streets

                if the count of used streets is exactly equal to buildings n minus one
                {
                    // exit the loop early because the buildings are fully connected
                }
            }
        }

        // multiply the total street length by the price p
        // print the final calculated result
    }

    end the program successfully
}
```
