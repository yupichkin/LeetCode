# Dynamic programming

+ [Knapsack problem](#knapsack-problem)
+ [Climbing Stairs](#climbing-stairs)
+ [Coin Change](#coin-change)
+ [Coin Change II](#coin-change-ii)
+ [Knapsack problem](#knapsack-problem)
+ [Knapsack problem](#knapsack-problem)
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

## Climbing Stairs
https://leetcode.com/problems/climbing-stairs/
Fibonacci numbers like
```C++ 
    int climbStairs(int n) {
        int prev =0;
        int tmp= 1;
        while(n--){
            tmp=tmp+prev;
            prev=tmp-prev;
        }
        return tmp;
    }
 ```
 
 ## Coin Change
https://leetcode.com/problems/coin-change/
```C++ 
    int coinChange(vector<int>& coins, int amount) {
        int maxPossible = amount + 1;
        vector<int> dp(amount + 1, maxPossible);
        dp[0] = 0;
        for (int i = 1; i <= amount; i++) {
          for (int j = 0; j < coins.size(); j++) {
            if (coins[j] <= i) {
              dp[i] = min(dp[i], dp[i - coins[j]] + 1);
            }
          }
        }
        return dp[amount] > amount ? -1 : dp[amount];
    }
 ```
 
## Coin Change II
https://leetcode.com/problems/coin-change-2/
```C++ 
    int change(int amount, vector<int>& coins) {
        vector<int> waysCount(amount + 1, 0);
        waysCount[0] = 1;
        for (int i = 0; i < coins.size(); i++) {
          for (int j = coins[i]; j <= amount; j++) {
              waysCount[j] += waysCount[j - coins[i]];
          }
        }
        return waysCount[amount];
    }
 ```
