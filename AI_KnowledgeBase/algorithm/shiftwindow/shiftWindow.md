# 枚举右与维护左 --滑动窗口浅析
## 滑动窗口

滑动窗口是一种用于解决数组或字符串相关问题的算法技巧。核心思想是维护一个窗口，通过该窗口在数据结构上滑动搜寻满足条件的最优解。一般分为可变长度的窗口与不可变长度的窗口。

## 枚举右

滑动窗口问题中，一般的思路是通过循环，不断枚举右端点：

    for right, n in enumerate(nums):

在循环的过程中去判断窗口内的数据是否符合条件

## 维护左

仅仅移动右端点是不足以使得窗口遍历到整个数据结构的，在枚举右端点的循环过程中需要不断维护左端点：

    while codition:
        move left

具体如何维护当然需要根据具体问题的条件决定。

## Instance
### 长度最小的子数组 leetcode209

    class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        tempSum = 0
        left = 0
        ans = inf
        for right, n in enumerate(nums): # 枚举右
            tempSum += n
            while tempSum >= target:
                L = right - left + 1
                ans = min(ans, L)
                tempSum -= nums[left]
                left += 1 # 维护左
        return ans if ans <= target else 0

### 无重复字符的最长子串 leetcode003

    class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:

        n = len(s)
        if n == 1:
            return 1
        left = 0
        hash = set()
        ans = 0
        for right, c in enumerate(s): # 枚举右
            while c in hash:
                hash.remove(s[left])
                left += 1 # 维护左
            hash.add(c)

            ans = max(ans, right - left + 1)
        return ans

### 乘积小于 K 的子数组 leetcode713

    class Solution:
    def numSubarrayProductLessThanK(self, nums: List[int], k: int) -> int:
        if k <= 1:
            return 0
        product = 1
        left = 0
        ans = 0
        for right, n in enumerate(nums):# 枚举右
            product *= n
            while product >= k:
                product /= nums[left]
                left += 1 # 维护左
            ans += right - left + 1
        return ans