\\# -Smart-Delivery-Planning-Fractional-Knapsack
#include <stdio.h> 
#include <limits.h> 
#define MAXV 20 #define MAXE 100 
typedef struct { int u, v, w; } Edge; 

Edge edges[MAXE]; int dist[MAXV]; int parent[MAXV]; 

void addEdge(int u, int v, int w, int *E) { 
edges[*E].u = u; edges[*E].v = v; edges[*E].w = w; (*E)++; 
} 

void bellmanFord(int V, int E, int source)
{ 
int i, j, updated; int negativeCycle = 0; 
for (i = 0; i < V; i++) 
{ 
  dist[i] = INT_MAX; 
   parent[i] = -1; 
} 
dist[source] = 0; 
for (i = 1; i <= V - 1; i++) 
{ 
   updated = 0; 
   for (j = 0; j < E; j++) 
   { 
       int u = edges[j].u; 
       int v = edges[j].v; 
       int w = edges[j].w;  
       if (dist[u] != INT_MAX && 
           dist[u] + w < dist[v]) 
           { 
           dist[v] = dist[u] + w; 
           parent[v] = u; 
           updated = 1; 
       } 
   } 
    if (!updated) 
       break; 
} 
 for (j = 0; j < E; j++) { 
   int u = edges[j].u; 
   int v = edges[j].v; 
   int w = edges[j].w; 
 
   if (dist[u] != INT_MAX && 
       dist[u] + w < dist[v]) { 
       negativeCycle = 1; 
       break; 
   } 
} 
if (negativeCycle) { 
   printf("\nERROR: Reachable negative cycle detected.\n"); 
   printf("Shortest routes cannot be reconstructed reliably.\n"); 
   return; 
} 
printf("\nShortest Routes from source %d:\n", source); 
 
for (i = 0; i < V; i++) { 
   if (dist[i] == INT_MAX) { 
       printf("Vertex %d : UNREACHABLE\n", i); 
   } 
   else { 
       int path[MAXV]; 
       int count = 0; 
       int current = i; 
        while (current != -1) { 
           path[count++] = current; 
           current = parent[current]; 
       } 
       printf("Vertex %d : Distance = %d, Route = ", 
              i, dist[i]); 
       while (count > 0) { 
           printf("%d", path[--count]); 
 
           if (count > 0) 
               printf(" -> "); 
       } 
       printf("\n"); 
   } 
} 
} 

int main() 
{ 
int V, E = 0, source; int i, u, v, w; 
printf("Enter number of vertices: "); 
scanf("%d", &V); 
printf("Enter number of directed edges: "); 
scanf("%d", &E); 
printf("Enter edges (source destination cost):\n"); 
for (i = 0; i < E; i++) { 
   scanf("%d %d %d", &u, &v, &w); 
   edges[i].u = u; 
   edges[i].v = v; 
   edges[i].w = w; 
} 
printf("Enter source vertex: "); 
scanf("%d", &source); 
bellmanFord(V, E, source); 
return 0; 
} 
