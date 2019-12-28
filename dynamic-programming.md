# Dynamic programming

+ [Knapsack problem](#knapsack-problem)
+ [Climbing Stairs](#climbing-stairs)
+ [Coin Change](#coin-change)
+ [Coin Change II](#coin-change-ii)
+ [Longest Increasing Subsequence](#longest-increasing-subsequence)
+ [Longest Common Subsequence](#longest-common-subsequence)
+ [Decode Ways](#decode-ways)
+ [House Robber II](#house-robber-ii)
+ [Jump Game II](#jump-game-ii)
+ [Unique Paths II](#unique-paths-ii)
+ [Word Break](#word-break)

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
 
 ## Longest Increasing Subsequence
https://leetcode.com/problems/longest-increasing-subsequence/
```C++ 
    int lengthOfLIS(vector<int>& nums) {
        if (nums.size() == 0) {
            return 0;
        }
        vector<int> dp(nums.size(), 0);
        dp[0] = 1;
        int maxans = 1;
        for (int i = 1; i < dp.size(); i++) {
            int maxval = 0;
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    maxval = max(maxval, dp[j]);
                }
            }
            dp[i] = maxval + 1;
            maxans = max(maxans, dp[i]);
        }
        return maxans;
    }
 ```
 
  ## Longest Common Subsequence
https://leetcode.com/problems/longest-common-subsequence/
```C++ 
    int longestCommonSubsequence(string text1, string text2) {
        vector<vector<int>> dp (text1.size() + 1, vector<int> (text2.size() + 1, 0));
        for(int i = 0; i < text1.size(); i++) {
            for(int j = 0; j < text2.size(); j++)
                 if(text1[i] == text2[j]) {
                     dp[i + 1][j + 1] = 1 + dp[i][j];
                 } else dp[i + 1][j + 1] = max(dp[i + 1][j], dp[i][j + 1]);
        }
        return dp.back().back();
    }
 ```
   ## Decode Ways
https://leetcode.com/problems/decode-ways/
```C++ 
    int numDecodings(string s) {
        if (s.size() == 1)
            return s[0] == '0' ? 0 : 1;
        int decodeCounts[s.size() + 2] = {}; //set all numbers to zero
        int i = s.size() - 1;
        decodeCounts[s.size() + 1] = 0;
        decodeCounts[s.size()] = 1;
        if (s[0] == '0') 
            return 0;
        while(i >= 0) {
            if (s[i] == '0') {
                if (s[i - 1] < '3' && s[i - 1] != '0') {//for call previous symbol
                    decodeCounts[i] = 0;
                    i--;
                    continue;
                }
                else
                    return 0;
            }
            if (s[i] == '1' || (s[i] == '2' && s[i + 1] <= '6')) {//then it could be double digit word
                decodeCounts[i] += decodeCounts[i + 2];
            }
            decodeCounts[i] += decodeCounts[i + 1];
            i--;
        }
        return decodeCounts[0];
    }
 ```
## Jump Game II
https://leetcode.com/problems/jump-game-ii
```C++ 
    int jump(vector<int>& nums) {
      int res = 0, currEnd = 0;
      int maxJump = 0;
      for (int i = 0; i < nums.size() - 1; i++) {     
        maxJump = max(maxJump, i + nums[i]);
        if (i == currEnd) {
          res++;
          currEnd = maxJump;
        }
      }
      return res;     
    }
 ```
 ## Unique Paths II
https://leetcode.com/problems/unique-paths-ii/
```C++ 
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        long int pathCount[obstacleGrid.size()][obstacleGrid[0].size()] = {};
        if(obstacleGrid[0][0])
            return 0;
        pathCount[0][0] = 1;
        for(int i = 1; i < obstacleGrid[0].size(); i++) {
            if (!obstacleGrid[0][i])
                pathCount[0][i] += pathCount[0][i - 1];
        }
        for(int j = 1; j < obstacleGrid.size(); j++) {
            if (!obstacleGrid[j][0])
                pathCount[j][0] += pathCount[j-1][0];
        }
        for(int j = 1; j < obstacleGrid.size(); j++) {
            for(int i = 1; i < obstacleGrid[0].size(); i++) {
            if (!obstacleGrid[j][i])
                pathCount[j][i] += pathCount[j][i - 1] + pathCount[j - 1][i];
            }
        }
        return pathCount[obstacleGrid.size() - 1][obstacleGrid[0].size() - 1];
    }
 ```
  ## Word Break
https://leetcode.com/problems/word-break/
```C++ 
    bool wordBreak(string s, vector<string>& wordDict) {
      vector<bool> dp(s.length() + 1, false);
      dp[0] = true;

      for (int i = 0; i <= s.length(); i++) {
        for (int j = 0; j < i; j++) {
          if (dp[j] && dictionaryIncludes(s.substr(j, i-j), wordDict)) {
            dp[i] = true;
            break;
          }
        }
      }
      return dp[s.length()];
    }
    
    bool dictionaryIncludes(string word, vector<string>& wordDict) { 
        for (int i = 0; i < wordDict.size(); i++) 
            if (wordDict[i].compare(word) == 0) 
               return true; 
        return false; 
    } 
 ```
