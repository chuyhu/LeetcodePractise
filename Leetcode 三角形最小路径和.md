# Leetcode 三角形最小路径和

## 1.首先是理解题目

#### ***1.给定一个三角形，从上到下求最小路径和。每一步都可以移动到下面一行的相邻数字。（这个三角形是以数组的形式给出）***

##  2.解题思路及具体方法：

## 方法1：

#### 首先考虑的就是暴力破解，我们发现如果遍历所有的路径的话，那时间复杂度太高了。而用贪婪算法的话却不能正确解题，因为贪婪算法可以带来局部最小，但是局部最小却不能保证全局最小，可能再其他的分支上的底层数字突然变得超级小，但是贪婪算法却把其他的分支都剪掉了，这么就不能得到正确答案，所以先采用DP算法。直接在triangle数组上实现复用。

#### ***状态转移方程为：triangle[i][j] = min(triangle[i - 1][j - 1], triangle[i - 1][j])***

```cpp
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        for (int i = 1; i < triangle.size(); ++i) {
            for (int j = 0; j < triangle[i].size(); ++j) {
                if (j == 0) {
                    triangle[i][j] += triangle[i - 1][j];
                } else if (j == triangle[i].size() - 1) {
                    triangle[i][j] += triangle[i - 1][j - 1];
                } else {
                    triangle[i][j] += min(triangle[i - 1][j - 1], triangle[i - 1][j]);
                }
            }
        }
        return *min_element(triangle.back().begin(), triangle.back().end());
    }
};
```

这个虽然可以实现，但是有一个缺点就是修改了原始数组triangle，并不太理想，故还有一种DP方法

```cpp
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        vector<int> dp(triangle.back());
        for (int i = (int)triangle.size() - 2; i >= 0; --i) {
            for (int j = 0; j <= i; ++j) {
                dp[j] = min(dp[j], dp[j + 1]) + triangle[i][j];
            }
        }
        return dp[0];
    }
};
```

![1566531046727](C:\Users\1742311241\AppData\Roaming\Typora\typora-user-images\1566531046727.png)

以上的方法都是DP算法，第二个方法笔第一个方法所用时间较短。

同时使用的vector容器，这是一个比较好用的高效的容器。《《需要注意的是vector的size函数返回的vector的大小，因为size返回值是无符号类型所以 vector.size()的越界问题。》》

[vector][]

这样的问题如果使用java的话就比较好操作

```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
         for (int i = triangle.size() - 2; i >= 0; i--){
             for (int j = 0; j <= i; j++)       {
                  triangle.get(i).set(j,triangle.get(i).get(j)+ Math.min(triangle.get(i + 1).get(j),  triangle.get(i + 1).get(j + 1)));   
             }
         }        
        return triangle.get(0).get(0);   
    }
}
```

操作思路大同小异，如果使用c语言的话

```c
int minimumTotal(int** triangle, int triangleRowSize, int *triangleColSizes) 
{
    int n=triangleRowSize;
    int* tmp=triangle[n-1];
    for(int i=n-2;i>=0;i--)
    {
        int m=triangleColSizes[i];
        for(int j=0;j<m;j++)
        {
            int min=tmp[j]<tmp[j+1]?tmp[j]:tmp[j+1];
            tmp[j]=triangle[i][j]+min;
        }
    }
    return tmp[0];   
}

```

















