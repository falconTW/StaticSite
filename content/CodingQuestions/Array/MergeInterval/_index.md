---
title: "MergeInterval"
date: 2019-06-22T14:25:56+08:00
draft: false
---

Given a collection of intervals, merge all overlapping intervals.

```
Example 1:

Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
Example 2:

Input: [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
NOTE: input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.
```


```C++
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
         vector<vector<int>> result;
         if(intervals.empty()){
             return result;
         }        
        
        sort(intervals.begin(),intervals.end(),[](const vector<int> &a,vector<int> &b){
            return a[0]<b[0];
        });
        
        result.push_back(intervals[0]);
        for(int i=1;i<intervals.size();i++){
            if(result.back()[1]<intervals[i][0]){
                result.push_back(intervals[i]);
            }else{
                result.back()[1]=max(result.back()[1],intervals[i][1]);
            }
        }
        return result;
    }
};
```