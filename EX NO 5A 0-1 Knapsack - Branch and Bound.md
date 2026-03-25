
# EX 5A 0/1 Knapsack Problem - Branch&Bound 
## DATE:20-03-2026

## AIM:

To Write a Java program to solve 0/1 Knapsack problem using Branch and Bound Approach.
You are heading a college entrepreneurship cell that can invest in up to N student‑startups.

For each startup i you know: cost[i]  — the amount (in ₹ lakh) required to join the showcase profit[i] — the estimated profit (in ₹ lakh) you’ll gain if it succeeds You have a total budget of B ₹ lakh. Pick a subset of startups so that the sum of costs ≤ B and the sum of profits is maximised.

Because N can be as large as 50, a plain exhaustive search (2^N) is too slow.

The recommended approach is Branch & Bound with a fractional‑knapsack upper bound (but any algorithm that meets the constraints is accepted). 

Input Format

N

B

cost[1] cost[2] … cost[N]

profit[1] profit[2] … profit[N]

1 ≤ N ≤ 50

1 ≤ B ≤ 1 000 000

1 ≤ cost[i], profit[i] ≤ 10 000 

Output Format

maxProfit

For example:




## Algorithm:

1. Read the number of startups **N**, the available budget **B**, and the arrays **cost[]** and **profit[]**.
2. Sort the startups in descending order of **profit-to-cost ratio** to improve bound tightness.
3. Use a **Depth-First Search Branch & Bound** approach:

   * Maintain current cost, current profit, and current index.
4. Compute an **upper bound** for each node using the **fractional knapsack** idea:

   * Add full projects while possible.
   * Add fractional part of the next project to estimate maximum possible profit.
5. If the upper bound ≤ current best profit → **prune** the branch.
6. Recursively explore:

   * Including the current project (if cost ≤ B)
   * Excluding the current project
7. Keep track of the **best maximum profit** found.
8. Print the maximum achievable profit.  

## Program:
```
/*
Program to implement Reverse a String
Developed by: Naveenkumar M
Register Number:  212224230182
*/
```
```
import java.util.*;

public class StartupShowcaseOptimizer {

    // ---------- Global data ----------
    static int N, B;
    static int[] c, p;          // cost, profit after sorting by ratio
    static int best = 0;        // incumbent best profit

    // ---------- Fractional upper bound ----------
    static double bound(int idx, int cw, int cv) {
    
        double value = cv;
        int weight = cw;
    
        for (int i = idx; i < N; i++) {
    
            if (weight + c[i] <= B) {
                weight += c[i];
                value += p[i];
            } 
            else {
                int remain = B - weight;
                value += (double)p[i] / c[i] * remain;
                break;
            }
        }
    
        return value;
    }
    
    
    // ---------- DFS Branch & Bound ----------
    static void dfs(int idx, int cw, int cv) {
    
        if (idx == N) {
            best = Math.max(best, cv);
            return;
        }
    
        if (bound(idx, cw, cv) <= best)
            return;
    
        if (cw + c[idx] <= B) {
            dfs(idx + 1, cw + c[idx], cv + p[idx]);
        }
    
        dfs(idx + 1, cw, cv);
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        N = sc.nextInt();
        B = sc.nextInt();
        int[] cost = new int[N];
        int[] prof = new int[N];
        for (int i = 0; i < N; i++) cost[i] = sc.nextInt();
        for (int i = 0; i < N; i++) prof[i] = sc.nextInt();
        sc.close();

        // Sort by profit/cost ratio descending → tighter bounds
        Integer[] idx = new Integer[N];
        Arrays.setAll(idx, i -> i);
        Arrays.sort(idx, Comparator.comparingDouble(i -> -(double) prof[i] / cost[i]));

        c = new int[N];
        p = new int[N];
        for (int i = 0; i < N; i++) {
            c[i] = cost[idx[i]];
            p[i] = prof[idx[i]];
        }

        dfs(0, 0, 0);
        System.out.println(best);
    }
}

```
## Output:

<img width="361" height="235" alt="image" src="https://github.com/user-attachments/assets/d54dbb46-a5e3-4154-bd2e-38f0c793b1fb" />


## Result:
The program successfully solved 0/1 Knapsack problem using branch & bound and output is verified. 
