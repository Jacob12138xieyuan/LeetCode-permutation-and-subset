# LeetCode-permutation-and-subset

## permutation
```
class Solution(object):
    def permute(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        # choose, explore, un-choose
        if len(nums) <= 1:
            return [nums]
        self.result = []
        used = [0]*len(nums) # 
        self.backTrack(nums, [], used)
        return self.result
        
    def backTrack(self, nums, array, used):
        if len(array) == len(nums):
            self.result.append(array)
            return
        for i in range(len(nums)):
            if used[i]: continue # this number be used
            # if haven't be used
            used[i] = 1 # choose
            self.backTrack(nums, array+[nums[i]], used) # explore
            used[i] = 0 # un choose
            
# []
# [1]
# [1, 2]
# [1, 2, 3]
# [1, 3]
# [1, 3, 2]
# [2]
# [2, 1]
# [2, 1, 3]
# [2, 3]
# [2, 3, 1]
# [3]
# [3, 1]
# [3, 1, 2]
# [3, 2]
# [3, 2, 1] 
```
or 
```
def permuteUnique(self, nums):
    res = []
    nums.sort()
    self.dfs(nums, [], res)
    return res
    
def dfs(self, nums, path, res):
    if not nums:
        res.append(path)
    for i in xrange(len(nums)):
        self.dfs(nums[:i]+nums[i+1:], path+[nums[i]], res)
```

## permutation 2
```
class Solution(object):
    def permuteUnique(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        self.result = []
        used = [0]*len(nums)
        nums.sort()
        self.backTrack(nums, [], used)
        return self.result
    
    def backTrack(self, nums, array, used):
        if len(array) == len(nums):
            self.result.append(array)
            return
        for i in range(len(nums)):
            if used[i]: continue
            if i > 0 and nums[i] == nums[i-1] and (not used[i-1]): continue # skip first duplicate number eg. [3,3,0,3] -> [0,3,3,3], skip first two 3 
            used[i] = 1
            self.backTrack(nums, array+[nums[i]], used)
            used[i] = 0
```
or
```
def permuteUnique(self, nums):
    res = []
    nums.sort()
    self.dfs(nums, [], res)
    return res
    
def dfs(self, nums, path, res):
    if not nums:
        res.append(path)
    for i in xrange(len(nums)):
        if i > 0 and nums[i] == nums[i-1]:
            continue
        self.dfs(nums[:i]+nums[i+1:], path+[nums[i]], res)
```


## subset 
```
class Solution(object):
    def subsets(self, nums):
        self.result = []
        self.backTrack(nums, [], 0)
        return self.result
    
    def backTrack(self, nums, array, index): # for every index, we can choose it or not choose it
        if index == len(nums): # all element is visited
            self.result.append(array)
            return
        self.backTrack(nums, array, index+1) # not choosing current number
        self.backTrack(nums, array+[nums[index]], index+1) # choosing current number
```


## subset 2
```
class Solution(object):
    def subsetsWithDup(self, nums):
        self.result = []
        nums.sort() # gather duplicate
        self.backTrack(nums, [], 0)
        return self.result
    
    def backTrack(self, nums, array, index): # for every index, we can choose it or not choose it
        if index == len(nums): # all element is visited
            self.result.append(array)
            return
        self.backTrack(nums, array+[nums[index]], index+1) # choosing current number
        # if above visited first duplicate, e.g. first '2', then only visit last duplicate
        while index+1 < len(nums) and nums[index] == nums[index+1]:
            index += 1
        self.backTrack(nums, array, index+1) # not choosing current number
        
        # no dup: [[1,2,2],[1,2],[1,2],[1],[2,2],[2],[2],[]]
        # with dup: [[1,2,2],[1,2],[1],[2,2],[2],[]]
```
