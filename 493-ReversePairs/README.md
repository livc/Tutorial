## 题目分析：

给定一个数组，问：有多少组(i, j)满足i < j并且nums[i] > 2*nums[j]？

### 解题思路（1）
分治。将数组等分成两部分l和r，排序后，我们可以O(n)复杂度内计算 i∈l，j∈r时的组数。总的时间复杂度为O(nlognlogn)。代码易写。
### 解题思路（2）
考虑同正的情况。我们可以先将以数组按(nums[i], i)打包成pair排序。从大到小扫描，维护一个下标集合S，S中的值x都满足nums[x] > 2*当前值。那么我们只需要负责统计可行的下标值有多少个即可。这些操作都可以通过树状数组或线段树在O(logn)内完成。那么总的时间复杂度即为O(nlogn)。还要考虑同负的情况，异号的情况等。代码量略大。
#### 算法的正确性：
思路一很显然。关键是思路二的正确性。因为我们按(nums[i], i)打包成pair排序，从大到小扫描，那么我们维护一个指针就可以使得被扫描过的值都满足nums[i] > 2*nums[j]，也没有遗漏的。那么就只剩下处理第一个约束了，即统计i < j的对数。运用线段树或树状数组就能轻松完成。当然，这只处理了全部为正的情况。我们还需要处理全部为负的情况，以及异号的情况。注意int型转为long long型来判断，否则会有溢出发生。
如7, 3, 5, 1. 按(nums[i], i)打包排序后，是(1, 4), (3, 2), (5, 3), (7, 1).
逆序扫描。其中，第二个指针不包含当前所指向的pair。
当指针一指向(7, 1)的时候，指针二指向第4个(7, 1)。7 > 2*7不成立。题干中的j是1，满足小于1的个数为0.
当指针一指向(5, 3)的时候，指针二指向第4个(7, 1)。7 > 2*5不成立。题干中的j是3，满足小于3的个数为0.
当指针一指向(3, 2)的时候，指针二指向第4个(7, 1)，7 > 2*3成立，插入1，指针二前移一个单位指向第3个(5, 3)。5 > 2*3不成立。题干中的j是2，满足小于2的个数为1.
当指针一指向(1, 4)的时候，指针二指向第3个(5, 3)，5 > 2*1成立，插入3，指针二前移一个单位指向第2个(3, 2)。3 > 2*1成立，插入2，指针二前移一个单位指向第1个(1, 4) 1 > 2*1不成立。题干中的j是4，满足小于4的个数为3.
故答案为1+3 = 4.
