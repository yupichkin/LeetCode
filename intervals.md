# Insert Interval

https://leetcode.com/problems/insert-interval/
```C++ 
  vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        if (intervals.size() == 0 || newInterval[1] < intervals[0][0] ) {
            intervals.insert(intervals.begin(), newInterval);
            return intervals;
        }
        if (newInterval[0] > intervals[intervals.size() - 1][1]) {
            intervals.insert(intervals.begin() + intervals.size(), newInterval);
            return intervals;
        }
        int deleteIndexBegin;
        int deleteIndexEnd;
        int size = intervals.size();
        vector<int> bufInterval;
        for(int i = 0; i < size; i++){
            if(newInterval[0] <= intervals[i][0]) {
                bufInterval.push_back(newInterval[0]); 
                deleteIndexBegin = i;
                break;
            }
            if(newInterval[0] <= intervals[i][1]) {
                bufInterval.push_back(intervals[i][0]);   
                deleteIndexBegin = i;
                break;
            }
        }
        
        for(int i = size - 1; i >= 0; i--){
            if(newInterval[1] >= intervals[i][1]) {
                bufInterval.push_back(newInterval[1]); 
                deleteIndexEnd = i + 1;
                break;
            }
            if(newInterval[1] >= intervals[i][0]) {
                bufInterval.push_back(intervals[i][1]);   
                deleteIndexEnd = i + 1;
                break;
            }
        }
        intervals.erase(intervals.begin() + deleteIndexBegin, intervals.begin() + deleteIndexEnd);
        intervals.insert(intervals.begin() + deleteIndexBegin, bufInterval);
        
        return intervals;
    }
 ```
 # Merge Intervals

https://leetcode.com/problems/merge-intervals/
```C++ 
bool CompareIntervalLeftBorders(vector<int>& interval1, vector<int>& interval2) {
    return (interval1[0] < interval2[0]);
}
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        if(intervals.empty())
            return intervals;
        sort(intervals.begin(), intervals.end(), CompareIntervalLeftBorders); 

        int size = intervals.size(); 
        int i = 0;
        int newIntervalsSize = 0;
        vector<vector<int>> newIntervals;
        vector<int> buffer;
        //newIntervals.reserve(intervals.size()*sizeof(int) * 2);
        
        while (i < size) {
            newIntervals.push_back(intervals[i]);
            i++;
            while(i < size && Crosses(newIntervals[newIntervals.size() - 1], intervals[i])) {
                
                buffer = Merge(newIntervals[newIntervals.size() - 1], intervals[i]);
                newIntervals[newIntervals.size() - 1] = buffer;
                i++;
            }
        }

        return newIntervals;
    }
private:
    vector<int> Merge(vector<int> interval1, vector<int> interval2) {
        vector<int> mergedInterval;
        mergedInterval.push_back(interval1[0] < interval2[0] ? interval1[0] : interval2[0]);
        mergedInterval.push_back(interval1[1] > interval2[1] ? interval1[1] : interval2[1]);
        return mergedInterval;
    }
    bool Crosses(vector<int> interval1, vector<int> interval2) {
        if (interval1[1] < interval2[0])
            return false;
        if (interval2[1] < interval1[0])
            return false;
        return true;
    }
    void SetRightBorder(vector<vector<int>>& intervals) { //sets to end interval with max right border
        int size = intervals.size();
        int position = 0;
        vector<int> buffer;
        for(int i = 1; i < size; i++) {
            if (intervals[position][1] < intervals[i][1])
                position = i;
        }
        buffer = intervals[position];
        intervals[position] = intervals[size - 1];
        intervals[size - 1] = buffer;
    }
};
 ```
 # Non-overlapping Intervals

https://leetcode.com/problems/non-overlapping-intervals/
```C++ 
  int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        if (intervals.empty())
            return 0;
        sort(intervals.begin(), intervals.end(), [](vector<int>& a, vector<int>& b){return a[1] < b[1];});
        int i = 1;
        int count = 1; //at least 1 interval does not intersect with others
        int previousEnd = intervals[0][1];
        while(i < intervals.size()) {
            if (intervals[i][0] >= previousEnd) {//no crosses
                count++;
                previousEnd = intervals[i][1];
            }    
            i++;
        }
        return intervals.size() - count;
    }
 ```
