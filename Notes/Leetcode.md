# Leetcode notes

[toc]
## Leetcode 新旧题目计划
|Old_Number|New_Number|Finish Date|
|:-----:|:-----:|:-----:|
|10|127|2020-08-30|



## leetcode思路
### 10 Regular Expression Matching
困扰我的地方是以下两段代码不等价，有时间研究，这道题正统解法还是dp
```c++
a = cal(a);
if (a):
    return a;
else:
    b = cal(b);
    return b;
```

```c++
return cal(a) || cal(b);
```


### 127 Word Ladder
遇到的第一道BFS题目, 目前还不太理解


