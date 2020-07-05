

### Manacher
https://www.jianshu.com/p/116aa58b7d81

### 

下面是一篇面经文章，作者是一个在校生，应聘字节跳动 AI lab 的算法工程师岗位的暑期实习。

面试时，就是三道算法题[1]。第一题是这样的：

10个小球随机分到12个盒子里，求恰好10个盒子都为空的概率，要求用 Python 程序模拟十万次，暴力求出该概率。

第一眼看到这道题，你可能觉得不难，就是随机模拟10万次，统计其中10个盒子为空的次数。这种暴力求解，属于蒙特卡洛模拟。

但是难点在于，12个盒子恰好有10个为空，是一个很低很低的概率，而10万次模拟次数又太少，导致模拟结果总是0，无法求出概率。作者发现了这一点以后，苦苦思索还是没有想出怎么解。

具体的解法可以参考这篇文章[2]，剧透一下，要使用条件概率公式。我觉得这道题偏难了，但是对于复习概率知识和 Python 随机模拟很有帮助。

[1] 三道算法题: https://www.nowcoder.com/discuss/395924
[2] 这篇文章: https://medium.com/@data.scientist/solving-the-interesting-bytedance-interview-question-bb30b31cdf5
[3] 程序员写简历的几个注意点: https://dev.to/gemography/common-mistakes-in-dev-cvs-2a17