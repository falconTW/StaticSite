---
title: "InsertInterval"
date: 2019-06-24T00:06:15+08:00
draft: false
---

Given a set of non-overlapping intervals, insert a new interval into the intervals (merge if necessary).

You may assume that the intervals were initially sorted according to their start times.

```
Example 1:

Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
Example 2:

Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
NOTE: input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.
```

## Solution 1

```
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        vector<vector<int>> result;
		
		int cur = 0;

		for (int i = 0; i < intervals.size(); ++i) {
			if (intervals[i][1] < newInterval[0]) {
				result.push_back(intervals[i]);
				cur++;
			}
			else if (intervals[i][0]>newInterval[1]) {
				result.push_back(intervals[i]);
			}
			else {
				newInterval[0] = min(intervals[i][0], newInterval[0]);
				newInterval[1] = max(intervals[i][1], newInterval[1]);
			}
		}

		result.insert(result.begin() + cur, newInterval);
		
		return result;

    }
};
```