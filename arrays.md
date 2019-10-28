# Two Sum

https://leetcode.com/problems/two-sum/
## with using hashmap
```C++ 
class Solution {
public:
  vector<int> twoSum(vector<int>& nums, int target) {
    unordered_map <int, int> map; //map with value and index;
    map[nums[0]] = 0;
    int i;
    for (i = 1; i < nums.size(); i++) {
        auto it = map.find(target - nums[i]);
        if (it != map.end())
            return {i, it->second};
        else
            map[nums[i]] = i;
    }
    return {}; //empty vector
  }
};
 ```
 
 ## just with iterate all elements
```C++ 
class Solution {
public:
  vector<int> twoSum(vector<int>& nums, int target) {
    int size = nums.size();
    int sumMember; //for not appealing nums[i] every cycle
    std::vector<int> indexOfTwo;
    for (int i = 0; i < size - 1; i++) {
      sumMember = nums[i];
      for (int j = i + 1; j < size; j++) {
        if (target == sumMember + nums[j]) {
          indexOfTwo.push_back(i);
          indexOfTwo.push_back(j);
          break;
        }
      }
      if (indexOfTwo.size() > 0)
        break;
    }
    return indexOfTwo;
  }
};
 ```
 
 # 3Sum

https://leetcode.com/problems/3sum/
```C++ 
bool Sorting (int a, int b) {
    return a < b;
}
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        if (nums.size() < 3)
            return {};
        int left;
        int right;
        int target = -32768;
        vector<vector<int>> sums;
        sort(nums.begin(), nums.end(), Sorting);
        for(int i = 0; i < nums.size() - 2; i++) {
            if (nums[i] == -target) //(-target = nums[i-1])
                continue;
            target = -nums[i];
            left = i + 1;
            right = nums.size() - 1;
            while (left < right) {
                if (nums[left] + nums[right] > target)
                    right--;
                else if (nums[left] + nums[right] < target)
                    left++;
                else {
                    sums.push_back({nums[i], nums[left], nums[right]});
                    cout << left << endl;
                    cout << right << endl;
                    left++;
                    right--; 
                    while (left < right && nums[left] == nums[left - 1] && nums[right] == nums[right + 1]) { //pass all same pairs
                        left++;
                        right--; 
                    }
                        
                }
            }
        }
        return sums;
    }
};
 ```
 
 # Subarray Sum Equals K
 
https://leetcode.com/problems/subarray-sum-equals-k/
```C++ 
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int sum;
        int i = 0;
        int j = 0;
        int size = nums.size();
        int count = 0;
        while (j < size) {
            i = j;
            sum = 0;
            while (i < size) {
                sum += nums[i];
                if (sum == k)
                    count++;
                i++;
            }
            j++;
        }
        return count;
    }
};
 ```
