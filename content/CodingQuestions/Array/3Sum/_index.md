---
title: "3Sum"
date: 2018-10-13T19:27:00+08:00
---

[Question Link](https://leetcode.com/problems/3sum)

Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

Note:

The solution set must not contain duplicate triplets.

## Solution 1
Using pair and hashtable with the same strategy in TwoSum can solve this question too.

However, it will lead to time limit excession in some large cases.

```C++
class Solution {

private:

	void _parseResult(int leadNumber, vector<pair<int, int>> input)
	{
		unordered_set<vector<int>, myVectorHash> result;
		for (int i = 0; i < input.size(); ++i)
		{
			for (auto iterator = input.begin(); iterator != input.end(); ++iterator)
			{
				vector<int> output = { leadNumber,iterator->first,iterator->second };
				
				result.insert(output);
			}
		}
		
		for (auto i = result.begin(); i != result.end(); ++i)
		{
			totalResult.push_back(*i);
		}
	}

public:
    
    struct myVectorHash
	{
		size_t operator()(vector<int>_val)const
		{
			return static_cast<size_t>(_val.size() + _val.back() - _val.front());
		}
	};

	struct myPairHash
	{
		size_t operator()(pair<int, int>_val)const
		{
			return static_cast<size_t>((_val.first+10) * 2 + _val.second);
		}
	};

	vector<vector<int>> totalResult;
	const bool isExist = true;

	vector<vector<int>> threeSum(vector<int>&nums)
	{
        
        sort(nums.begin(), nums.end());
        
		threeSumHelper(nums);
		return totalResult;
	}

    
    void threeSumHelper(vector<int>&nums)
    {
        vector<pair<int, int>> pairResult;
		for (int i = 0; i < nums.size(); ++i)
		{
			if (i>0 && nums[i]==nums[i-1])
			{
				continue;
			}

			int leadNumber = nums[i];
            int target = leadNumber*-1;
			vector<int> targetVec = vector<int>(nums.begin() + i+1, nums.end());
			pairResult = twoSum(targetVec, target);

			if (!pairResult.empty())
			{
				_parseResult(leadNumber, pairResult);
			}

		}
    }
    

	vector<pair<int, int>> twoSum(vector<int>& nums, int target)
	{
		unordered_set <pair<int, int>, myPairHash> result;
		unordered_map<int, int> map;

		for (int i = 0; i < nums.size(); ++i)
		{     
			auto iterator = map.find(target - nums[i]);
			if (iterator == map.end())
			{
				map[nums[i]] = i;
			}
			else
			{
				result.insert(make_pair(nums[iterator->second],nums[i]));
			}
		}
		return vector<pair<int, int>>(result.begin(), result.end());
	}
};
```

## Solution 2
We sort the array first to keep it in ascending order.

Then we take every element from the start and find the other two from the next element.

Here we use dubble pointer to squeeze out the answers, since the whole array is in ascending order.

We can avoid doing some dupicate operations.

```C++
vector<vector<int>> threeSum(vector<int>& nums) 
    {
         vector<vector<int>> result;
	 sort(nums.begin(), nums.end());

	 if (nums.size() < 3)
	 {
		 return result;
	 }

	 for (int i = 0; i < nums.size()-2; ++i)
	 {
         if(nums[i]>0)
         {
             break;
         }
         
		 if (i > 0 && nums[i] == nums[i - 1])continue;

		 int start = i + 1;
		 int end = nums.size()-1;

		 while (start < end)
		 {
			 int target = nums[i] * -1;
			 int curSum = nums[start] + nums[end];

			 if (target == curSum)
			 {
                  vector<int> res;
				 res.push_back(nums[i]);
				 res.push_back(nums[start]);
				 res.push_back(nums[end]);
                  result.push_back(res);
				 ++start;
				 --end;
				 while (nums[start] == nums[start - 1])++start;
				 while (nums[end] == nums[end + 1])--end;
			 }
			 else if (target > curSum)
			 {
				 ++start;
				 while (nums[start] == nums[start - 1])++start;
			 }
			 else
			 {
				 --end;
				 while (nums[end] == nums[end + 1])--end;
			 }
		 }
	 }

	 return result;
    }
```