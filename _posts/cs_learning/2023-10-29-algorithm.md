---
layout: post
title: "代码随想录专题算法一刷记录"
subtitle: "Algorithm note"
date: 2023-10-29
author: "Kenshin"
header-img: "img/post-bg/post-bg-default.png"
tags: 
  - LearnCS
---

[扩展题目](https://www.programmercarl.com/other/ewaishuoming.html)还未做，待补天

## 数组

二分，双指针（快慢，首尾），滑动窗口

## 链表

虚拟头结点，双指针

## 哈希表

set与map

能够退化成数组时更快

## 字符串

双指针，部分翻转+全部翻转，s*2拼接

## 单调队列

用双端队列实现，具体保持单调的方式需要实际分析。eg滑动窗口的最大值

## 优先队列

大小顶堆（前k大，用小顶堆边加边删，nlogk）

## 单调栈

**通常是一维数组，要寻找任一个元素的右边或者左边第一个比自己大或者小的元素的位置，此时我们就要想到可以用单调栈了**。时间复杂度为O(n)。

eg接雨水：

双指针→求左右最大，直接用vector即可

求凹槽/内部最大→递增递减/左右双单调栈 或 单调栈+记录中点

## 二叉树

分类：
    满二叉树、完全二叉树
    二叉搜索树BST、平衡二叉搜索树 AVL（Adelson-Velsky and Landis）树
        C++中map、set、multimap，multiset的底层实现都是平衡二叉搜索树
        BST相关的题要考虑一下可以转化为有序数组

存储：
    链式，数组（i -> 2 * i + 1, 2 * i + 2）

遍历：
    dfs：前序，中序，后序
    bfs：层序

    递归三要素：
        1. 确定递归函数的参数和返回值： 确定哪些参数是递归的过程中需要处理的，那么就在递归函数里加上这个参数， 并且还要明确每次递归的返回值是什么进而确定递归函数的返回类型。
        2. 确定终止条件： 写完了递归算法, 运行的时候，经常会遇到栈溢出的错误，就是没写终止条件或者终止条件写的不对，操作系统也是用一个栈的结构来保存每一层递归的信息，如果递归没有终止，操作系统的内存栈必然就会溢出。
        3. 确定单层递归的逻辑： 确定每一层递归需要处理的信息。在这里也就会重复调用自己来实现递归的过程。

    迭代法：
        栈模拟递归

    层序遍历:
        队列

后序遍历（左右中）就是天然的回溯过程，可以根据左右子树的返回值，来处理中节点的逻辑。

## 图

岛屿数量[力扣链接](https://leetcode.cn/problems/number-of-islands/solutions/13103/dao-yu-shu-liang-by-leetcode/) dfs、bfs、并查集基础题

```cpp
// dfs
class Solution {
public:
    const int dirs[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};

    bool ifDfs(vector<vector<char>>& grid, vector<vector<bool>>& visited, int i, int j) {
        if (i < 0 || i >= grid.size()) return false;
        if (j < 0 || j >= grid[0].size()) return false;
        if (visited[i][j] || grid[i][j] == '0') return false;
        return true;
    }

    void dfs(vector<vector<char>>& grid, vector<vector<bool>>& visited, int i, int j) {
        visited[i][j] = true;
        for (auto dir : dirs) {
            if (ifDfs(grid, visited, i + dir[0], j + dir[1]))
                dfs(grid, visited, i + dir[0], j + dir[1]);
        }
    }

    int numIslands(vector<vector<char>>& grid) {
        int n = grid.size(), m = grid[0].size();
        vector<vector<bool>> visited(n, vector<bool>(m));
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                if (!ifDfs(grid, visited, i, j)) continue;
                dfs(grid, visited, i, j);
                ans++;
            }
        }
        return ans;
    }
};

// bfs
class Solution {
public:
    const int dirs[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};

    bool ifBfs(vector<vector<char>>& grid, vector<vector<bool>>& visited, int i, int j) {
        if (i < 0 || i >= grid.size()) return false;
        if (j < 0 || j >= grid[0].size()) return false;
        if (visited[i][j] || grid[i][j] == '0') return false;
        return true;
    }

    void bfs(vector<vector<char>>& grid, vector<vector<bool>>& visited, int i, int j) {
        queue<pair<int, int>> q;
        visited[i][j] = true;
        q.push({i, j});
        while (!q.empty()) {
            pair<int, int> temp = q.front();
            q.pop();
            int curi = temp.first, curj = temp.second;
            for (auto dir : dirs) {
                int ni = curi + dir[0], nj = curj + dir[1];
                if (!ifBfs(grid, visited, ni, nj)) continue;
                visited[ni][nj] = true;
                q.push({ni, nj});
            }
        }
    }

    int numIslands(vector<vector<char>>& grid) {
        int n = grid.size(), m = grid[0].size();
        vector<vector<bool>> visited(n, vector<bool>(m));
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                if (!ifBfs(grid, visited, i, j)) continue;
                bfs(grid, visited, i, j);
                ans++;
            }
        }
        return ans;
    }
};

// 并查集
class UnionFind {
public:
    UnionFind(vector<vector<char>>& grid) {
        count = 0;
        int m = grid.size();
        int n = grid[0].size();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == '1') {
                    parent.push_back(i * n + j);
                    ++count;
                }
                else {
                    parent.push_back(-1);
                }
                rank.push_back(0);
            }
        }
    }

    int find(int i) {
        return parent[i] == i ? i : parent[i] = find(parent[i]);
    }

    void unite(int x, int y) {
        // parent[find(y)] = find(x);
        int rootx = find(x);
        int rooty = find(y);
        if (rootx != rooty) {
            if (rank[rootx] < rank[rooty]) {
                swap(rootx, rooty);
            }
            parent[rooty] = rootx;
            if (rank[rootx] == rank[rooty]) rank[rootx] += 1;
            --count;
        }
    }

    int getCount() const {
        return count;
    }

private:
    vector<int> parent;
    vector<int> rank;
    int count;
};

class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int nr = grid.size();
        if (!nr) return 0;
        int nc = grid[0].size();

        UnionFind uf(grid);
        int num_islands = 0;
        for (int r = 0; r < nr; ++r) {
            for (int c = 0; c < nc; ++c) {
                if (grid[r][c] == '1') {
                    grid[r][c] = '0';
                    if (r - 1 >= 0 && grid[r-1][c] == '1') uf.unite(r * nc + c, (r-1) * nc + c);
                    if (r + 1 < nr && grid[r+1][c] == '1') uf.unite(r * nc + c, (r+1) * nc + c);
                    if (c - 1 >= 0 && grid[r][c-1] == '1') uf.unite(r * nc + c, r * nc + c - 1);
                    if (c + 1 < nc && grid[r][c+1] == '1') uf.unite(r * nc + c, r * nc + c + 1);
                }
            }
        }

        return uf.getCount();
    }
};
```

对于单源单目标，使用双向dfs/bfs可以节省时间

## 回溯backtrack

### 适用问题

组合问题：N个数里面按一定规则找出k个数的集合
切割问题：一个字符串按一定规则有几种切割方式
子集问题：一个N个数的集合里有多少符合条件的子集
排列问题：N个数按一定规则全排列，有几种排列方式
棋盘问题：N皇后，解数独等等

### 模板

```c
vector<Ty> path;

void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果;
    }
}
```

### trick

求全排列，叶子节点push，循环从0~n-1
求全子集，中间节点也push，循环从i+1~n-1
结果不重复，即同层不重复，层内（回溯函数内）哈希，或排序后判断是否与前一个元素相同（求子序列时不可用，需要用传参增加visited）
去重尽量使用数组，用set的话时间和空间复杂度都高

### 时空复杂度

子集问题分析：
    时间复杂度：O(2^n)，因为每一个元素的状态无外乎取与不取，所以时间复杂度为O(2^n)
    空间复杂度：O(n)，递归深度为n，所以系统栈所用空间为O(n)，每一层递归所用的空间都是常数级别，注意代码里的result和path都是全局变量，就算是放在参数里，传的也是引用，并不会新申请内存空间，最终空间复杂度为O(n)

排列问题分析：
    时间复杂度：O(n!)，这个可以从排列的树形图中很明显发现，每一层节点为n，第二层每一个分支都延伸了n-1个分支，再往下又是n-2个分支，所以一直到叶子节点一共就是 n * n-1 * n-2 * ..... 1 = n!。
    空间复杂度：O(n)，和子集问题同理。

组合问题分析：
    时间复杂度：O(2^n)，组合问题其实就是一种子集的问题，所以组合问题最坏的情况，也不会超过子集问题的时间复杂度。
    空间复杂度：O(n)，和子集问题同理。

N皇后问题分析：
    时间复杂度：O(n!) ，其实如果看树形图的话，直觉上是O(n^n)，但皇后之间不能见面所以在搜索的过程中是有剪枝的，最差也就是O（n!），n!表示n * (n-1) * .... * 1。
    空间复杂度：O(n)，和子集问题同理。

解数独问题分析：
    时间复杂度：O(9^m) , m是'.'的数目。
    空间复杂度：O(n^2)，递归的深度是n^2

## 贪心

贪心：一个新员工一个老员工价值相当，老员工就可以走了，因为新员工被榨取的剩余空间更多。

二维贪心：
    
    正反遍历二次贪心，左最优+右最优=全局最优
    对于有先后/因果关系的两个维度，选择其一进行排序转化为一维贪心

## 动规：背包，股票，子序列

定义子问题，确定dp含义，确定递推公式，边界条件，遍历顺序

### 背包：背包九讲

定义背包容量，定义物品价值，定义物品体积，确定物品数量

#### 01：物品唯一

关于物品和容量循环哪个在里哪个在外→二阶简化为一阶时，注意遍历顺序不要覆盖被简化掉的维度

物品只能被添加一次→对于01背包问题一维dp的遍历，nums放在外循环，target在内循环，且内循环倒序

#### 完全：物品无限

物品可以多次添加→正序遍历

外层物品内层容量→组合数

外层容量内层物品→排列数

无限的问题用贪心容易超时（？

#### 多重：物品有限

01背包的内层循环再加一层遍历物品数量，正序遍历（即：正逆正）

#### 其他

分组：物品按组

混合背包

二维价值背包

### 打家劫舍 + 买卖股票

### 子序列

## 其他

除留取余等价于改步长的循环+余数判断

```cpp
#include <numeric>
#include <iostream>
#include <vector>
#include <iterator>

int main()
{
	std::vector<int> ivec(10);
	std::iota(ivec.begin(), ivec.end(), 4);
	//for (auto &it: ivec)
	//{
	//	std::cout << it << " ";
	//}

	//或者用下面的一句话输出ivec中内容
	std::copy(ivec.begin(), ivec.end(), std::ostream_iterator<int>(std::cout, " "));

	getchar();
	return 0;
}
```