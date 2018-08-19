---
author: Fengrui Zhang
comments: true
date: 2018-08-19 20:30:00+00:00
layout: post
title: Leetcode Valid-Sudoku 有效的数独
categories:
- ACM
tags:
- Easy Problems
- C
- Leetcode
---



# Leetcode Valid-Sudoku 有效的数独

[题目链接](https://leetcode-cn.com/problems/valid-sudoku/description/)

###题目 有效的数独
```
判断一个 9x9 的数独是否有效。只需要根据以下规则，验证已经填入的数字是否有效即可。
数字 1-9 在每一行只能出现一次。
数字 1-9 在每一列只能出现一次。
数字 1-9 在每一个以粗实线分隔的 3x3 宫内只能出现一次。
```

![GitHub set up](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)
*上图是一个部分填充的有效的数独。*

数独部分空格内已填入了数字，空白格用 `.` 表示.

**示例 1:**

```输入:
[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
输出: true
```

**示例 2:**


```输入:
[
  ["8","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
输出: false
解释: 除了第一行的第一个数字从 5 改为 8 以外，空格内其他数字均与 示例1 相同。
     但由于位于左上角的 3x3 宫内有两个 8 存在, 因此这个数独是无效的。
```

**说明:**

```一个有效的数独（部分已被填充）不一定是可解的。
只需要根据以上规则，验证已经填入的数字是否有效即可。
给定数独序列只包含数字 1-9 和字符 '.' 。
给定数独永远是 9x9 形式的。
```

##思路

根据数独规则（Row,Column,Cell中1-9只能出现一次），设置三个二维数组存放标记，初始化为0，若某行/列/3*3单元出现过某数，则将其值更改为1。

```c
int rowMark[9][10]={0};
int colMark[9][10]={0};
int celMark[9][10]={0};
```
遍历初始二维数组，若非`'.'`，则将其与`'0'`的ASCII差值（1-9）赋值给临时变量`temp`

``` c
for(int i=0;i<9;i++)
        for(int j=0;j<9;j++){
            if(board[i][j]!='.'){
                int temp=board[i][j]-'0';
                ...
```
若某行/列/3*3单元出现过`temp`，即`row/col/celMark[][temp]==1`,则此数独无解，返回 `false`。否则更改相应元素为1后继续。（此部分使用了[两数之和](https://leetcode-cn.com/problems/two-sum/description/)的方法，利用下标识别数字）

```c 
if(rowMark[j][temp]) return false;
                else rowMark[j][temp]=1;
                if(colMark[i][temp]) return false;
                else colMark[i][temp]=1;
                int k=i/3*3+j/3;
                if(celMark[k][temp]) return false;
                else celMark[k][temp]=1;
```

若成功遍历数组，则返回`true`

```c
    return true
```

**完整代码如下:**


```c
bool isValidSudoku(char** board, int boardRowSize, int boardColSize) {
    int rowMark[9][10]={0};
    int colMark[9][10]={0};
    int celMark[9][10]={0};
    for(int i=0;i<9;i++)
        for(int j=0;j<9;j++){
            if(board[i][j]!='.'){
                int temp=board[i][j]-'0';
                if(rowMark[j][temp]) return false;
                else rowMark[j][temp]=1;
                if(colMark[i][temp]) return false;
                else colMark[i][temp]=1;
                int k=i/3*3+j/3;
                if(celMark[k][temp]) return false;
                else celMark[k][temp]=1;
            }
        }
    return true;
}
```

**执行用时：** *8ms 排名100%* 

<!-- Add Disqus comments. --> <div id="disqus_thread"></div> <script type="text/javascript"> /* * * CONFIGURATION VARIABLES: EDIT BEFORE PASTING INTO YOUR WEBPAGE * * */ var disqus_shortname = '<USERNAME>'; // required: replace example with your forum shortname var disqus_identifier = "/works/tech/2016/06/07/%E5%9C%A8github%20pages%E7%BD%91%E7%AB%99%E4%B8%8B%E7%94%A8jekyll%E5%88%B6%E4%BD%9C%E5%8D%9A%E5%AE%A2%E6%95%99%E7%A8%8B.html"; /* * * DON'T EDIT BELOW THIS LINE * * */ (function() { var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true; dsq.src = '//' + disqus_shortname + '.disqus.com/embed.js'; (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq); })(); </script> <noscript>Please enable JavaScript to view the <a href="http://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript> <a href="http://disqus.com" class="dsq-brlink">comments powered by <span class="logo-disqus">Disqus</span></a>









