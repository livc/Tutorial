### 题意
**中文描述**  
一个二维矩阵由 `0` 和 `1` 组成，找出矩阵中由 `1` 组成的最大的正方形，求出它的面积。  
举个例子，下面的二维矩阵 `matrix[4][5]`  
```cpp
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0
```
最大的正方形是以 `matrix[1][2]` 为正方形的左上角，以 `matrix[2][3]` 为正方形的右下角。  
它的面积是 `2 * 2 = 4`。  

### 题解
#### 解题思路（1）
应该如何找一个正方形，我们不妨使用最笨的办法，以某个 `1` 为正方形的左上角，以 `matrix[1][2]` 为例，此时正方形的边长 `c = 1`；  
- 接着看这个 `matrix[1][2]` 的右方，下方以及右下方是否都是 `1`，如果都是 `1` ，说明正方形的边长 `c = 2`，很幸运 `matrix[1][3]、matrix[2][2] 和 matrix[2][3]` 确实都是 `1`；  
- 接下来我们要观察 `matrix[1][3]、matrix[2][2] 和 matrix[2][3]` 三个位置各自右方，下方以及右下方是否都是 `1`，一共是 `5` 个位置：`matrix[1][4]、matrix[2][4]、matrix[3][2]、matrix[3][3] 和 matrix[3][4]`。  
- `5` 个位置中有两个位置上的数字不是 `1`，至此我们以 `matrix[1][2]` 为正方形左上角的最大正方形的边长是 `c = 2`。

**时间复杂度**  
这种方法的时间复杂度最大是 `O(m^2 * n^2)`,其中 `m` 是二维矩阵第一维的大小，`n` 是二维矩阵第二维的大小。  
时间复杂度太高了，我们要另想办法。  

#### 解题思路（2）
具体实现在 `solution1.cpp`。  
之前的方法，对于每个 `1` 都要把它当作正方形的左上角来扩展，也就是说我们做了很多冗余的计算，以下方的二维矩阵为例：
```cpp
1 1 1 1 0
0 1 1 1 1
1 1 1 1 1
1 0 0 1 0
```
1. 假设以 `matrix[0][1]` 为正方形的左上角，我们可以扩展正方形的边长到 `c = 3`，正方形右下角为 `matrix[2][3]`；  
2. 我们继续寻找 `1` 作为正方形的左上角来扩展，找到了 `matrix[0][2]`，它可以扩展到 `matrix[1][3]`，正方形的边长为 `c = 2`。  
3. 继续寻找 `matrix[0][3]`，不能扩展，正方形的边长为 `c = 1`；  
4. 继续寻找 `matrix[1][1]`，它可以扩展到 `matrix[2][2]`，正方形的边长为 `c = 2`。  
5. 继续寻找 `matrix[1][2]`，它可以扩展到 `matrix[2][3]`，正方形的边长为 `c = 2`。  
6. 继续寻找 `matrix[1][3]`，它可以扩展到 `matrix[2][4]`，正方形的边长为 `c = 2`。  
7. 继续寻找 `matrix[1][4]`，不能扩展，正方形的边长为 `c = 1`；  
8. 继续寻找 `matrix[2][1]`，不能扩展，正方形的边长为 `c = 1`；  
9. 继续寻找 `matrix[2][2]`，不能扩展，正方形的边长为 `c = 1`；  
10. 继续寻找 `matrix[2][3]`，不能扩展，正方形的边长为 `c = 1`；  

我们来看，第 `2` 步到第 `10` 步，除了第 `6` 步，扩展过程都是与第 `1` 步重复的。  
可以看出扩展过程的重复是造成时间复杂度过大的重要原因，所以我们引入了动态规划（Dynamic Programming）方法来避免重复地扩展。  
**计算顺序**  
计算顺序的选择是动态规划的关键点之一，合理的计算顺序使得在计算当前状态时可以使用之前计算的结果。  
以上面的例子，在以 `matrix[0][1]` 为正方形的左上角扩展之前，我们可以先扩展 `matrix[0][2]、matrix[1][2] 和 matrix[1][1]`。  
同样的逻辑，在扩展 `matrix[0][2]、matrix[1][2] 和 matrix[1][1]` 之前，我们也要先扩展它们的右方，下方以及右下方。  
直到边界。  
**状态转移方程**  
`dp[i][j]` 表示以 `matrix[i][j]` 为正方形的左上角扩展所能达到的最大边长。  
`dp[i][j] = 1 + min(dp[i][j+1], dp[i+1][j], dp[i+1][j+1])`,   
其中 `dp[i][j+1], dp[i+1][j], dp[i+1][j+1]` 分别代表以 `matrix[i][j]` 的右方、下方和右下方三个位置作为正方形左上角所能扩展达到的最大边长，它们中的边长最小值再加上 `1` 就是 `matrix[i][j]` 所能扩展的最大正方形边长。  
**另一种计算顺序**  
之前我们是确定正方形的左上角进行扩展，现在我们确定正方形的右下角进行扩展，想一想计算顺序是怎样的，状态转移应该怎么写。  
这个问题留给读者思考，在本文的最后，我会留下状态转移方程，请读者务必自己实现代码。

#### 以上面的例子走一下算法流程
我们采用确定正方形右下角的方式扩展正方形。  
要计算以当前位置作为右下角时正方形的边长可以扩展到多少，我们需要知道它的上方、左方和左上方三个位置作为正方形右下角所能扩展达到的最大边长。  
因此，计算顺序可以是从第一行开始到最后一行，每一行内从左到右；按照这种顺序，我们可以使用二维矩阵本身记录每个位置所能扩展的最大边长。  
原矩阵：
```cpp
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0
```
我省略了的边长为 `1` 的正方形的计算过程。
- 计算完第一行之后：
```cpp
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0
```
- 计算完第二行之后：
```cpp
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0
```
- 在第三行内：  
`matrix[2][3] = 1 + min(matrix[1][3], matrix[2][2], matrix[1][2])= 1 + min(1, 1, 1) = 2`;  
`matrix[2][4] = 1 + min(matrix[1][4], matrix[2][3], matrix[1][3])= 1 + min(1, 2, 1) = 2`;  
计算完第三行之后：
```cpp
1 0 1 0 0
1 0 1 1 1
1 1 1 2 2
1 0 0 1 0
```
- 计算完第四行之后：
```cpp
1 0 1 0 0
1 0 1 1 1
1 1 1 2 2
1 0 0 1 0
```

最大的正方形的边长为 `2`，面积为 `4`。

### 思考题答案
状态转移方程：`dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])`。  
`dp[i-1][j], dp[i][j-1], dp[i-1][j-1]` 分别代表以 `matrix[i][j]` 的上方、左方和左上方三个位置作为正方形右下角所能扩展达到的最大边长，它们中的边长最小值再加上 `1` 就是 `matrix[i][j]` 所能扩展的最大正方形边长。