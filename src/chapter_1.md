# Chapter 1

Here is a Markdown document detailing the logic, mathematical foundation, and implementation of your C program.Sorting Triangles by Area in CThis program accepts a list of triangles defined by the lengths of their three sides ($a$, $b$, and $c$), calculates the area of each using Heron's Formula, and sorts them in ascending order based on their area.1. Mathematical Logic: Heron's FormulaTo calculate the area of a triangle when only the lengths of the sides are known, the program uses Heron's Formula.First, the semi-perimeter ($p$) is calculated:$$p = \frac{a + b + c}{2}$$Then, the area ($A$) is derived using:$$A = \sqrt{p(p-a)(p-b)(p-c)}$$2. Code Structure & AnalysisData StructureThe program uses a struct to organize the data. This keeps the three sides of a triangle grouped together as a single unit.Cstruct triangle {
int a;
int b;
int c;
};
The sort_by_area FunctionThis function performs two main tasks:Calculation: It iterates through the array of triangles and computes the area for each, storing the results in a temporary area[] array.Sorting: It uses a nested loop algorithm (similar to Bubble Sort) to reorder the triangles.It compares the areas of the triangles.If a triangle has a smaller area than a preceding triangle, both the area value and the triangle struct are swapped.Memory ManagementIn the main function, dynamic memory allocation is used because the number of triangles ($n$) is input by the user at runtime.malloc(n \* sizeof(triangle)) is used to reserve the exact amount of memory needed.3. Source CodeC#include <stdio.h>
#include <stdlib.h>
#include <math.h>

struct triangle
{
int a;
int b;
int c;
};

typedef struct triangle triangle;

void sort_by_area(triangle* tr, int n) {
/\*\*
* Sort an array a of the length n
\*/
// Note: sorting_index is declared but not used in this logic
int sorting_index[n];
float area[n];

    // Step 1: Calculate Area for all triangles
    for (int i=0; i<n; i++) {
        float p = (tr[i].a + tr[i].b + tr[i].c) / 2.0;
        area[i] = sqrt(p * (p - tr[i].a) * (p - tr[i].b) * (p - tr[i].c));
    }

    float temp;
    triangle onetri;

    // Step 2: Sort based on Area (Bubble-style sort)
    for (int i=1; i<n; i++) {
        for (int j=0; j<i; j++) {
            if(area[j] > area[i]){
                // Swap Area values
                temp = area[j];
                area[j] = area[i];
                area[i] = temp;

                // Swap Triangle structs to match the area order
                onetri = tr[j];
                tr[j] = tr[i];
                tr[i] = onetri;
            }
        }
    }

}

int main()
{
int n;
// Read number of triangles
scanf("%d", &n);

    // Allocate memory dynamically
    triangle *tr = malloc(n * sizeof(triangle));

    // Read sides for each triangle
    for (int i = 0; i < n; i++) {
        scanf("%d%d%d", &tr[i].a, &tr[i].b, &tr[i].c);
    }

    // Perform Sort
    sort_by_area(tr, n);

    // Print Sorted Triangles
    for (int i = 0; i < n; i++) {
        printf("%d %d %d\n", tr[i].a, tr[i].b, tr[i].c);
    }

    return 0;

} 4. Complexity NotesTime Complexity: The sorting algorithm uses nested loops (i from 1 to $n$, j from 0 to $i$). This results in a time complexity of $O(n^2)$. While acceptable for small inputs, algorithms like Quicksort ($O(n \log n)$) are preferred for very large datasets.Space Complexity: The function creates an auxiliary array area[n], making the space complexity $O(n)$.5. Usage ExampleInput:Plaintext3
7 24 25
5 12 13
3 4 5
(Explanation: 3 triangles. The 7-24-25 triangle has area 84. The 5-12-13 triangle has area 30. The 3-4-5 triangle has area 6.)Output:Plaintext3 4 5
5 12 13
7 24 25
(The output is sorted by area: 6, then 30, then 84)
