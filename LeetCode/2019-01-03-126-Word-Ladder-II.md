# LeetCode 126. Word Ladder II

## Description

Given two words (beginWord and endWord), and a dictionary's word list, find all shortest transformation sequence(s) from beginWord to endWord, such that:

Only one letter can be changed at a time
Each transformed word must exist in the word list. Note that beginWord is not a transformed word.
Note:

Return an empty list if there is no such transformation sequence.
All words have the same length.
All words contain only lowercase alphabetic characters.
You may assume no duplicates in the word list.
You may assume beginWord and endWord are non-empty and are not the same.
Example 1:

```python
Input:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

Output:
[
  ["hit","hot","dot","dog","cog"],
  ["hit","hot","lot","log","cog"]
]
```

```python
Example 2:

Input:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

Output: []
```

## 描述

给定两个单词（beginWord 和 endWord）和一个字典 wordList，找出所有从 beginWord 到 endWord 的最短转换序列。转换需遵循如下规则：

每次转换只能改变一个字母。
转换过程中的中间单词必须是字典中的单词。
说明:

如果不存在这样的转换序列，返回一个空列表。
所有单词具有相同的长度。
所有单词只由小写字母组成。
字典中不存在重复的单词。
你可以假设 beginWord 和 endWord 是非空的，且二者不相同。
示例 1:

```python
输入:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

输出:
[
  ["hit","hot","dot","dog","cog"],
  ["hit","hot","lot","log","cog"]
]
```

```python
示例 2:

输入:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

输出: []
```

解释: endWord "cog" 不在字典中，所以不存在符合要求的转换序列。
Explanation: The endWord "cog" is not in wordList, therefore no possible transformation.

### 思路

* 此题目使用BFS+DFS的方法

```python
# -*- coding: utf-8 -*-
# @Author:             何睿
# @Create Date:        2019-01-03 11:50:12
# @Last Modified by:   何睿
# @Last Modified time: 2019-01-04 15:24:54

from collections import deque
from pprint import pprint

class Solution:
    def findLadders(self, beginWord, endWord, wordList):
        """
        :type beginWord: str
        :type endWord: str
        :type wordList: List[str]
        :rtype: List[List[str]]
        """
        wordset = set(wordList)
        if endWord not in wordset:
            return []
        level = self.ladderLength(beginWord, endWord, wordList)
        print(level)
        if beginWord in wordset:
            wordset.remove(beginWord)
        res = []
        self.dfs(res, wordset, beginWord, endWord, [beginWord], [], 0, level)
        return res

    def dfs(self, res, wordset, word, endWord, used, path, curlevel, level):
        if curlevel >= level:
            return
        if word == endWord:
            res.append(path+[word])
            return
        for i in range(len(word)):
            for j in range(0, 26):
                newword = word[0:i]+chr(j+97)+word[i+1:]
                if newword in wordset and newword not in used   :
                    self.dfs(res, wordset, newword, endWord, used +[newword], path+[word], curlevel+1, level)

    def ladderLength(self, beginWord, endWord, wordList):
        """
        :type beginWord: str
        :type endWord: str
        :type wordList: List[str]
        :rtype: int
        """
        wordset = set(wordList)
        if endWord not in wordset:
            return 0
        res = 1
        left, right = set(), set()
        left.add(beginWord)
        right.add(endWord)
        while left or right:
            newset = set()
            left, right = right, left
            for word in left:
                for i in range(len(word)):
                    for j in range(0, 26):
                        newword = word[0:i]+chr(j+97)+word[i+1:]
                        if newword in wordset:
                            newset.add(newword)
                            wordset.remove(newword)
                        if newword in right:
                            return res+1
            res += 1
            left = newset
        return 0
```

源代码文件在[这里](https://github.com/ruicore).
©本文是原创文章，欢迎转载，转载需保留[文章来源](https://www.ruicore.cn/)，作者信息和本声明.
©本文首发于[何睿的博客](https://www.ruicore.cn/)，欢迎转载，转载需保留文章来源，作者信息和本声明.
