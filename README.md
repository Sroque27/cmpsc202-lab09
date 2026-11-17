# Lab 09: Exploring D&C Algorithms

**Instructions:** Work in small groups to complete the following exercises. Try sketching out your work on the whiteboards or scratch paper. Everyone should submit their own answers to the exercises, but you are encouraged to discuss your reasoning with your group members.

Name(s): Samuel Roque

---

## Exercise 1: Recurrence Extraction

Consider the following divide-and-conquer algorithm, which takes an array $A$ of $n$ numbers as input and returns a single number.

```text
Procedure Mystery(A, n)
    if n <= 1:
        return A[1]                              // Base case

    k = floor(n / 3)                             // Find the one-third point

    left = Mystery(A[1..k], k)                   // Recur on the left third
    right = Mystery(A[k+1..n], n - k)            // Recur on the rest

    total = 0
    for i = 1 to n:
        total = total + (A[i] * left * right)    // Combine results

    return total
```

### Part A: The Recurrence Relation

The algorithm makes two recursive calls. The first call uses about one-third of the input, so its running time is $T(n/3)$. The second call uses the remaining two-thirds, so its running time is $T(2n/3)$. After the recursive calls, the `for` loop goes through all $n$ elements, which takes $cn$ time for some constant $c$.

Therefore, the recurrence is:

$$
T(n) = T\left(\frac{n}{3}\right) + T\left(\frac{2n}{3}\right) + cn
$$

with the base case:

$$
T(1) = c
$$

### Part B: Visualizing Algorithm Behavior

The recursion tree would be **skewed rather than balanced**. At each node, one recursive call receives about $n/3$ elements while the other receives about $2n/3$ elements. This means the branch that repeatedly takes the $2n/3$ portion will go deeper than the branch that repeatedly takes the $n/3$ portion. Both branches continue getting smaller, but they do not decrease at the same rate.

---

## Exercise 2: Unrolling the Recursion Tree

We are given the recurrence:

$$
T(n) \leq 3T\left(\frac{n}{2}\right) + cn^2
$$

with:

$$
T(1) \leq c
$$

### Part C: Recursion Tree Analysis

#### 1. Root Work

At level 0, there is only one problem of size $n$. The non-recursive work at the root is:

$$
cn^2
$$

Therefore, the total work at level 0 is:

$$
cn^2
$$

#### 2. First Levels

At level 1, there are 3 recursive problems, each with input size $n/2$. The work for each node is:

$$
c\left(\frac{n}{2}\right)^2
$$

Therefore, the total work at level 1 is:

$$
3c\left(\frac{n}{2}\right)^2
$$

$$
= \frac{3}{4}cn^2
$$

At level 2, each of the 3 nodes creates 3 more recursive calls, giving $3^2=9$ nodes. Each problem now has size $n/4$.

The total work is:

$$
9c\left(\frac{n}{4}\right)^2
$$

$$
= \frac{9}{16}cn^2
$$

So the work for the first three levels is:

- **Level 0:** $cn^2$
- **Level 1:** $\frac{3}{4}cn^2$
- **Level 2:** $\frac{9}{16}cn^2$

#### 3. Tree Height

At each level, the input size is divided by 2. At level $i$, the input size is:

$$
\frac{n}{2^i}
$$

The recursion stops when this reaches 1:

$$
\frac{n}{2^i}=1
$$

Therefore:

$$
n=2^i
$$

and:

$$
i=\log_2 n
$$

So the height of the recursion tree is:

$$
\log_2 n
$$

### Bonus: General Formula for Level $i$

At level $i$, there are:

$$
3^i
$$

nodes.

Each node has an input size of:

$$
\frac{n}{2^i}
$$

so the work done by each node is:

$$
c\left(\frac{n}{2^i}\right)^2
$$

Multiplying the number of nodes by the work per node gives:

$$
3^i c\left(\frac{n}{2^i}\right)^2
$$

which simplifies to:

$$
cn^2\left(\frac{3}{4}\right)^i
$$

Therefore, the total work at level $i$ is:

$$
cn^2\left(\frac{3}{4}\right)^i
$$

---

## Exercise 3: Reflection

### 1. During lecture last week, we looked at two different divide-and-conquer algorithms. What were these algorithms and what problems did they solve?

The two divide-and-conquer algorithms we discussed were **Merge Sort** and the **Nearest Neighbors problem in 2D**.

Merge Sort solves the problem of sorting a collection of values. It divides the input into two halves, recursively sorts each half, and then combines the two sorted halves. Its running time is $\Theta(n \log n)$.

The Nearest Neighbors algorithm solves the problem of finding the pair of points with the smallest distance between them. It divides the points into two halves and recursively finds the closest pair on each side. It then checks the region near the dividing line to make sure the closest pair does not have one point on each side. This gives a running time of $O(n \log n)$ instead of the $\Theta(n^2)$ brute-force approach that checks every pair.

### 2. How do the algorithms you analyzed in this lab compare to the ones we discussed in lecture? Specifically, each algorithm differs from the two we discussed in lecture in one important way. For each of the algorithms above, what is that difference?

The main difference in **Exercise 1** is that the recursive subproblems are not equal in size. Merge Sort and the Nearest Neighbors algorithm divide the problem into two roughly equal halves, but `Mystery` divides the input into one subproblem of size $n/3$ and another of size $2n/3$. This makes its recursion tree skewed instead of balanced.

The main difference in **Exercise 2** is the number of recursive subproblems. Merge Sort and the Nearest Neighbors algorithm make two recursive calls on problems of about half the original size. The recurrence in Exercise 2 makes **three** recursive calls of size $n/2$. This means the number of nodes grows by a factor of 3 at each level instead of a factor of 2. Exercise 2 also performs $cn^2$ work at each individual problem before accounting for the recursive calls, rather than the linear combine work used in the lecture examples.