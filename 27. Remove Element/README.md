# 题目分析

求数组去掉所有值为val的元素后，剩余数组和其大小。要求空间复杂度为常数，不能使用额外的数组。

## 解题思路

维护两个变量l和r，l代表答案数组的最大索引。r从0开始遍历数组，如果nums[r] !=val，那么就把nums[r]赋值给nums[l]，然后l自增1。