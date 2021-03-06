##题目分析

给出三个串a,b,c，问c是否是串b中插入串a得到的。
比如，`a="abc"`,`b="def"`，那么`c="adebcf"`就是合法的，可以通过把`"de"`插入到`"ab"`中间，然后把`"f"`插入到`"c"`后面得到串c。

##题解分析

类似于经典的最公共子序列问题，这题也是用动态规划解决。首先状态设计就是用`DP[i][j]`表示串a下标i**以前**的串和串b下标j**以前**的串已经插入完成是否可以构成c串的前`i+j`位，这是一个布尔型的值。那么转移就很简答了：下一位是c串的`i+j`位，它只可能是a串的第`i`位或者是b串的第`j`位，于是只需要判断`c[i+j]`和`a[i]`或者`b[j]`是否相等便能转移到`DP[i+1][j]`或者`DP[i][j+1]`了。

我们还是用题目分析中的三个串作为例子：
初始化所有的DP值都为flase，`DP[0][0]=1`，那么我们可以通过它转移到`DP[1][0]=true (a[0]==c[0])`，然后接着转移到`DP[1][1]=true (b[0]==c[1])`......最终我们可以得到`DP[3][3]=true`，证明串a和b可以组成串c。

时间复杂度：
过程需要完成一个二维的DP数组，第一维长度等于串a，第二维长度等于串b，故时空复杂度都是`O(nm)`。

