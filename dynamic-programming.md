# Dynamic programming

+ [Knapsack problem](#knapsack-problem)


## Knapsack problem
https://stepik.org/lesson/13259/step/5?thread=solutions&unit=3444

```C++ 
int maxWeight(int backpackSize, vector<int> weights) {
    vector<vector<int>> dp(weights.size() + 1, vector<int>(backpackSize + 1, 0));
    for (int i = 1; i <= weights.size(); i++) {
        for (int w = 1; w <= backpackSize; w++) {
            if (weights[i - 1] <= w) {
                dp[i][w] = max(dp[i - 1][w], dp[i - 1][w - weights[i - 1]] + weights[i - 1]);
            }
            else {
                dp[i][w] = dp[i - 1][w];
            }
        }
    }
    return dp.back().back();
}
 ```
