---
layout: post
title: "计算机数据结构"
subtitle: "Data Structure"
date: 2022-12-20
author: "Kenshin"
header-img: "img/post-bg/post-bg-default.png"
tags:
  - LearnCS
---

# Category

- [绪论](#绪论)
- [线性表](#线性表)
  - [栈 FILO](#栈-filo)
  - [队列 FIFO](#队列-fifo)
  - [串的模式匹配](#串的模式匹配)
  - [数组，矩阵，广义表](#数组矩阵广义表)
- [树](#树)
  - [二叉树](#二叉树)
  - [存储方法](#存储方法)
  - [树和森林的互相转换](#树和森林的互相转换)
  - [二叉排序树与平衡二叉树（查找）](#二叉排序树与平衡二叉树查找)
  - [Huffman 树与 Huffman 编码](#huffman-树与-huffman-编码)
- [图](#图)
  - [存储结构](#存储结构)
  - [遍历算法](#遍历算法)
  - [关键路径](#关键路径)
- [排序](#排序)
  - [插入类排序](#插入类排序)
  - [选择类排序](#选择类排序)
  - [二路归并排序](#二路归并排序)
  - [二叉排序树 BST](#二叉排序树-bst)

## 绪论

- 数据结构的基本概念
  - 数据：客观事物的符号表示
  - 数据元素：基本单位（数据项：构成数据元素的最小单位，当数据元素为结构体时）
  - 数据对象：性质相同的数据元素的集合
  - 数据类型：一个值的集合和定义在此集合上一组操作的总称
    - 原子类型：其值不可再分
    - 结构类型：其值可以再分解为若干分量
    - 抽象数据类型：抽象数据组织和与之相关的操作
  - 数据结构：相互之间存在特定关系的数据元素的集合「逻辑结构+存储结构+对数据的运算（数据操作）」
    - Data-Structure=(D, S)，D是数据元素的有限集，S是D上关系的有限集
  - 数据的逻辑结构：线性结构，非线性结构
    - 线性结构（一对一）：首，尾，前驱，后继
    - 集合
    - 树型结构（一对多）
    - 图状结构/网状结构（多对多）
  - 数据的物理结构
    - 顺序存储
    - 链式存储
    - 索引存储：索引表<关键字，地址>
    - 散列存储：顺序存储的扩展，散列函数
      算法的基本概念
  - 数据的运算
    - 运算的定义针对逻辑结构，指出运算的功能
    - 运算的实现针对存储结构，指出运算的具体操作步骤
- 算法是对特定问题求解方法的一种描述，是指令的有限序列
  - 算法的描述：自然语言、形式语言、计算机程序设计语言
  - 算法的特性：有穷，确定，可行，输入，输出
  - 设计目标：正确（最重要最基本），可读，健壮（容错），效率（时间，存储量）（，通用性）
- 抽象数据类型 ADT：数据对象集，数据关系集，操作集
  - 要素：状态+操作
  - ADT=(D, S, P)，D是数据对象，S是D上的关系集，P是对D的基本操作集

## 线性表

具有相同特性数据元素的有限序列

- 顺序表：随机访问
  - 判断下标合法性
- 链表：动态分配
  - 单链表，双链表
  - 循环单链表，循环双链表
  - 静态链表（数组存储，以游标替代next指针）
- 存储密度
  - 顺序表=1，链表<1

### 栈 FILO

C++标准库是有多个版本的，要知道我们使用的STL是哪个版本，才能知道对应的栈和队列的实现原理。

那么来介绍一下，三个最为普遍的STL版本：

    HP STL 其他版本的C++ STL，一般是以HP STL为蓝本实现出来的，HP STL是C++ STL的第一个实现版本，而且开放源代码。

    P.J.Plauger STL 由P.J.Plauger参照HP STL实现出来的，被Visual C++编译器所采用，不是开源的。

    SGI STL 由Silicon Graphics Computer Systems公司参照HP STL实现，被Linux的C++编译器GCC所采用，SGI STL是开源软件，源码可读性甚高。

栈是以底层容器完成其所有的工作，对外提供统一的接口，底层容器是可插拔的（也就是说我们可以控制使用哪种容器来实现栈的功能）。

所以STL中栈往往不被归类为容器，而被归类为container adapter（容器适配器）。

栈的底层实现可以是vector，deque(默认)，list 都是可以的， 主要就是数组和链表的底层实现。

队列中先进先出的数据结构，同样不允许有遍历行为，不提供迭代器, SGI STL中队列一样是以deque为缺省情况下的底部结构。

- 顺序栈
- 栈空`top=-1`，栈满（动态顺序栈满使用realloc扩容），上溢（出错），下溢（可能正常，作为控制转移条件）

```c
stack[++top]=x;      //进栈
x=stack[top--];      //出栈
```

- 链栈（头插）
- 栈空`lst->next==NULL`、栈满（不存在）
   
```c
p->next=lst->next;lst->next=p;                        //进栈
p=lst->next;x=p->data;lst->next=p->next;free(p);      //出栈
```

- n 个元素进栈并可在任意时刻出栈，所获得的的元素排列的数目 N 满足 Catalan( )函数：$N=\frac{1}{n+1}C_{2n}^{n}$
- 后缀式：数值入栈；前缀式：符号入栈

### 队列 FIFO

- 队头指可进行删除的一端（出队），队尾指可进行插入的一端（入队）
- 顺序队—循环队列

```c
qu.rear=qu.front;                                    //队空
(qu.rear+1)%maxSize==qu.front;                       //队满
qu.rear=(qu.rear+1)%maxSize;qu.data[qu.rear]=x;      //入队
qu.front=(qu.front+1)%maxSize;x=qu.data[qu.front];   //出队
```

- 顺序队列存在“假溢出”现象 -> 循环队列
- 循环队列是指顺序存储的队列，而不是逻辑上的循环，如循环单链表表示的队列不能称为循环队列
- 链队：队尾 rear 队头 front
- 共享栈、双端队列
  > eg.循环队列存储在一位数组 A[0, ···, n-1]中，且队列非空时 front 和 rear 分别指向队头和队尾元素。初始时队列为空，要求第一个进队的元素存储在 A[0]，则初始时 front 和 rear 的值分别是`0, n-1`。

### 串的模式匹配

```c
strassign(Str& str, char* ch);                    //赋值
strlength(Str str);                               //取串长
strcompare(Str str1, Str str2);                   //比较
concat(Str& str, Str str1,Str str2);              //连接
substring(Str& substr, Str str,int pos, int len); //求子串
clearstring(Str& str);                            //清空
```

- next 数组

  - 手工方法：计算某个字符对应的 next 值，就是看这个字符之前的字符串中有多大长度的相同前缀后缀

- 匹配失败，模式串右移 j - next[j]位
  - 代码：

```c
void GetNext(char* p,int next[]){
	int pLen = strlen(p);
	next[0] = -1;
	int k = -1, j = 0;
	while (j < pLen - 1)
		//p[k]表示前缀，p[j]表示后缀
        //k是前缀末尾下标，j是后缀末尾的后一个下标即当前位置
		if (k == -1 || p[j] == p[k])
			next[++j] = ++k;
		else
			k = next[k];
}
```

- 改进的 next 数组

```c
void GetNextval(char* p, int next[]){
	int pLen = strlen(p);
	next[0] = -1;
	int k = -1, j = 0;
	while (j < pLen - 1)
		//p[k]表示前缀，p[j]表示后缀
		if (k == -1 || p[j] == p[k]){
			++j; ++k;
			if (p[j] != p[k])
				next[j] = k;
			else
				//只要出现了p[next[j]] = p[j]的情况，则把next[j]的值再次减小
				next[j] = next[k];
		}else
			k = next[k];
}
```

对于优化后的 next 数组，如果模式串的后缀跟前缀相同，那么它们的 next 值也是相同的

- KMP 算法

```c
int KmpSearch(char* s, char* p){
	int i = 0, j = 0;
	int sLen = strlen(s);
	int pLen = strlen(p);
	while (i < sLen && j < pLen)
		if (j == -1 || s[i] == p[j]){
			++i; ++j;
		}else
			j = next[j];
	if (j == pLen)
		return i - j;
	else
		return -1;
}
```

优势：i 不需要回溯，对于规模较大字符串可以分段匹配，减少 I/O 操作，提高效率

### 数组，矩阵，广义表

- 数组：数组是存放在**连续内存空间**上的**相同类型**数据的**集合**。
  - 一维数组
  - 二维数组：行优先，列优先

- 矩阵

  - 特殊矩阵：对称，上/下三角，对角

  - 稀疏矩阵

    - 顺序存储：三元组（值，行下标，列下标），伪地址(n _ 2, n _ (i - 1) + j)

    - 链式存储：邻接表（行号；值，列号）

      ​ 十字链表（行头，列头；行号，列号，数据域，下指针，右指针）

- 广义表：表元素可以是原子或者广义表的一种线性表的扩展结构
  - 长度：表中最上层元素的个数
  - 深度：表中括号的最大层数
  - 广义表非空时，第一个元素为表头，**其余元素组成的表是表尾**
  - 原子结点：标记域，数据域；广义表结点：标记域，头指针域，尾指针域7
  - 标记域为 0，原子；1，广义表
  - 扩展线性表……

## 树

- 树的度：树中各结点度的最大值。结点的度：拥有的子树个数或者分支的个数。
- 单结点既是根结点又是叶子结点
- 顺序存储结构：双亲存储结构——Kruskal 算法应用
- 链式存储结构：孩子存储结构——图的邻接表；孩子兄弟存储结构
- 表示形式：倒悬树、嵌套集合、广义表、凹入法

### 二叉树

- 满二叉树，完全二叉树……

- 性质
  - 叶子结点数 n~0~，单分支结点数 n~1~，双分支结点数 n~2~
    - n~0~ = n~2~ + 1
    - 总结点数 n~0~+n~1~+n~2~
    - 总分支数 n~1~+2n
  - 第 i 层上最多有 2^i-1^个结点
  - 高度为 k 的二叉树最多有 2^k^ - 1 个结点
  - n 个结点的完全二叉树，若 i 为某结点的编号
    - 1 开始编号：双亲结点$\lfloor \frac{i}{2} \rfloor$，左孩子$2i$，右孩子$2i+1$
    - 0 开始编号：双亲结点$\lceil \frac{i}{2} \rceil-1$，左孩子$2i$+1，右孩子$2i+2$
  - Catalan( )函数：n 个结点能构成 h(n)种不同的二叉树，$h(n)=\frac{C_{2n}^{n}}{n+1}$
  - 具有 n 个结点的完全二叉树的高度为$\lfloor log_2n\rfloor+1$或$\lceil log_2(n+1)\rceil$
  - `n`个结点的二叉链表中含有`n + 1`个空链域
- 存储结构
  - 顺序存储：适合完全二叉树
  - 链式存储：<data, lchild, rchild>
- 遍历算法——递归法，非递归法，线索二叉树

  - 先序遍历，中序遍历，后序遍历
  - 若前序队列对后序队列刚好相反，则不存在一个结点同时有左右孩子
  - 叶子结点在三种序列中的相对顺序不变
  - 先序遍历序列即为各结点入栈时打印所得的序列，中序遍历序列即为各结点出栈时打印所得的序列

```cpp
// 递归法
class Solution {
public:
    void traversal(TreeNode* cur, vector<int>& vec) {
        if (cur == NULL) return;
        vec.push_back(cur->val);    // 中，前序
        traversal(cur->left, vec);  // 左
        // vec.push_back(cur->val);    // 中，中序
        traversal(cur->right, vec); // 右
        // vec.push_back(cur->val);    // 中，后序
    }
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        traversal(root, result);
        return result;
    }
};
// 迭代法
// 前序遍历中访问节点（遍历节点）和处理节点（将元素放进result数组中）可以同步处理，但是中序就无法做到同步！
// 前序遍历
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> result;
        if (root == NULL) return result;
        st.push(root);
        while (!st.empty()) {
            TreeNode* node = st.top();                       // 中
            st.pop();
            result.push_back(node->val);
            if (node->right) st.push(node->right);           // 右（空节点不入栈）
            if (node->left) st.push(node->left);             // 左（空节点不入栈）
        }
        return result;
    }
};
// 中序遍历
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> st;
        TreeNode* cur = root;
        while (cur != NULL || !st.empty()) {
            if (cur != NULL) { // 指针来访问节点，访问到最底层
                st.push(cur); // 将访问的节点放进栈
                cur = cur->left;                // 左
            } else {
                cur = st.top(); // 从栈里弹出的数据，就是要处理的数据（放进result数组里的数据）
                st.pop();
                result.push_back(cur->val);     // 中
                cur = cur->right;               // 右
            }
        }
        return result;
    }
};
// 后序遍历
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> result;
        if (root == NULL) return result;
        st.push(root);
        while (!st.empty()) {
            TreeNode* node = st.top();
            st.pop();
            result.push_back(node->val);
            if (node->left) st.push(node->left); // 相对于前序遍历，这更改一下入栈顺序 （空节点不入栈）
            if (node->right) st.push(node->right); // 空节点不入栈
        }
        reverse(result.begin(), result.end()); // 将结果反转之后就是左右中的顺序了
        return result;
    }
};
// 统一的迭代法。加标记指针
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> st;
        if (root != NULL) st.push(root);
        while (!st.empty()) {
            TreeNode* node = st.top();
            if (node != NULL) {
                st.pop(); // 将该节点弹出，避免重复操作，下面再将右中左节点添加到栈中
                // st.push(node); st.push(NULL); // 前序
                if (node->right) st.push(node->right);  // 添加右节点（空节点不入栈）
                st.push(node); st.push(NULL); // 添加中节点。中节点访问过，但是还没有处理，加入空节点做为标记。
                if (node->left) st.push(node->left);    // 添加左节点（空节点不入栈）
                // st.push(node); st.push(NULL); // 后序
            } else { // 只有遇到空节点的时候，才将下一个节点放进结果集
                st.pop();           // 将空节点弹出
                node = st.top();    // 重新取出栈中元素
                st.pop();
                result.push_back(node->val); // 加入到结果集
            }
        }
        return result;
    }
};
```

- 带中序序列的两个序列可以唯一确定一棵二叉树

  - 层序遍历——队列

- 逆后序遍历序列是先序遍历过程中对左右子树遍历顺序交换所得到的的结果
- K 叉树中编号为 i 的结点从右往左数，第二个孩子结点的编号为 K\*i（根结点编号为 1）
- 线索二叉树`lchild | ltag | data | rtag | rchild`

  - 把树中的空指针利用起来作为寻找当前结点前驱或后继的线索，消除了递归，也不必使用栈(后序仍要)

  - ltag=0，lchild 为指针，指向结点的左孩子，左子树的最右侧结点为前驱结点

  - ltag=1，lchild 为线索，指向结点的直接前驱

  - rtag=0，rchild 为指针，指向结点的右孩子，右子树的最左侧结点为后继结点

  - rtag=1，rchild 为指针，指向结点的直接后继

  - **0 孩 1 索**

  - 以中序线索二叉树为例

    ```c
    void InThread(TBTNode *p, TBTNode *&pre){
        if(p!=NULL){
        InThread(p->lchild, pre);
        if(p->lchild==NULL){
            p->lchild=pre;
            p->ltag=1;
        }
        if(pre!=NULL&&pre->rchild==NULL){
            pre->rchild=p;
            pre->rtag=1;
        }
        pre=p;
        InThread(p->rchild, pre);
        }
        }
        void createInThread(TBTNode *root){
        TBTNode *pre=NULL;
        if(root!=NULL){
        InThread(root, pre);
        pre->rchild=NULL;
        pre->rtag=1;
        }
    }
    ```
    ```c
    TBTNode *First(TBTNode *p){
        while(p->ltag==0)
        p=p->lchild;
        return p;
    }
    TBTNode *Next(TBTNode *p){
        if(p->rtag==0)
        return First(p->rchild);
        else
        return p->rchild;
    }
    void Inorder(TBTNode *root){
        for(TBTNode *p=First(root);p!=NULL;p=Next(p))
        Visit(p);
    }
    ```

  - 对于后序线索二叉树

    - 根结点的后继为空
    - 是右孩子，或是左孩子且双亲没有右子树，后继为双亲结点
    - 左孩子且双亲有右子树，则后继为双亲右子树上按后序遍历出的第一个结点

  - 二叉树线索化后仍不能有效求解的问题是后续线索二叉树求后序后继：前序遍历（中左右）、中序遍历（左中右）的最后访问的节点都是左或右叶节点，叶节点是没有子树的，所以两个指针域空出来了，可以存放线索指针。但是后续遍历（左右中），最后访问的子树的根节点，子树根节点的两个指针域都指向子树了，所以不能空出来存放线索信息。

### 存储方法

- 双亲表示法：顺序结构，数组存储每个节点，增设伪指针指示双亲结点在数组中的位置，根结点下标为 0，伪指针为-1。查找子女复杂
- 孩子表示法：每个节点的每个孩子用单链表连接，查找子女直接，查找双亲复杂（类似哈希表的链地址法）
- 孩子兄弟表示法：结点值 data，第一个孩子结点 firstchild，下一个兄弟结点 nextsibling。查找双亲麻烦

### 树和森林的互相转换

- 树 → 二叉树：孩子兄弟表示法
- 二叉树 → 树：分层
- 森林 → 二叉树：孩子兄弟表示法中根结点一定没有右兄弟，即没有右孩子，后一个树作为前一个树的右子树
- 二叉树 → 森林：不断将根结点有右孩子的二叉树右孩子链接断开，然后二叉树 → 树
- 先序遍历和后序遍历的对应
    |树  |二叉树 | 森林 |
    | - | - | - |
    |先根 | 先序 | 先序 |
    |后根 | 中序 | 中序 |

### 二叉排序树与平衡二叉树（查找）

### Huffman 树与 Huffman 编码

- 默认二叉树分析，又称最优二叉树
  - 路径长度指路径上的分支数目
  - 树的路径长度指根到每个结点的路径长度之和
  - 结点的带权路径长度：该结点到根之间的路径长度乘以结点的权值
  - 树的带权路径长度（WPL）：树中所有叶子结点的带权路径长度之和
- 特点

  - 权值越大的结点，距离根结点越近
  - 树中没有度为 1 的结点，这类树又称正则（严格）二叉树
  - 树的带权路径长度最短

- Huffman 编码—最节省空间的存储方式（最短前缀码）
  - 前缀码：任一字符的编码串都**不是**另一字符编码串的前缀
  - 根通往任一叶子结点的路径都不可能是通往其余叶子结点的子路径
- Huffman n 叉树
  - 结点数目 ≥2 的待处理序列都可以构造 Huffman 二叉树
  - N 个结点需要构建 N-1 个分支节点，总结点数 2N-1
  - 无法构造 Huffman n 叉树时，补上若干权值为 0 的结点$n-(1+N)%n$

## 图

### 存储结构

- 邻接矩阵—顺序存储

  - (i, j)元素含义：顶点 i 到顶点 j 之间长度为 1 的路径长度(当无直达边存在表示为 0 时)。表现为有边

    ```c
    typedef struct{
        int no;
        char info;
    }VertexType;
    typedef struct{
        int edges[maxSize][maxSize];
        int n,e;
        VertexType vex[maxSize];
    }MGraph;
    ```

- 邻接表—链式存储

    ```c
    typedef struct ArcNode{
        int adjvex;
        struct ArcNode *nextarc;
        int info;
    }ArcNode;
    typedef struct{
        char data;
        ArcNode *firstarc;
    }VNode;
    typedef struct{
        VNode adjlist[maxSize];
        int n,e;
    }AGraph;
    ```

- 十字链表——用于有向图

  - 顶点结点：data firstin firstout
    - 指向以该顶点为弧头或弧尾的第一个弧结点

  - 弧结点：tailvex headvex hlink tlink info
    - hlink, tlink 指向弧头、弧尾相同的下一条弧

- 邻接多重表—用于无向图

  - 顶点：data firstedge
  - 边表：mark ivex ilink jvex jlink info
  - mark 为搜索标志域
    - ilink 指向下一条依附于顶点 ivex 的边
    - jlink 指向下一条依附于顶点 jvex 的边

### 遍历算法

- 假设用邻接表作为图的存储结构

- ```c
  void dfs(AGraph *g){                //bfs...
    int i;
    for(i=0;i<g->n;++i)
      if(visit[i]==0)
        DFS(g,i);                     //BFS(g,i,visit);
  }
  ```

- ```c
  int visit[maxSize];
  void DFS(AGraph *G, int v){
    ArcNode *p;
    //Visit(v);
    visit[v]=1;
    p=G->adjlist[v].firstarc;
    while(p!=NULL){
      if(visit[p->adjvex]==0)
        DFS(G, p->adjvex);
      p=p->nextarc;
    }
  }
  void DFS_S(AGraph *G, int v){
  ArcNode *p;
    int stack[maxSize], top=-1;
    int i, k;
    //Visit(v);
    visit[v]=1;
    stack[++top]=v;
    while(top!=-1){
      k=stack[top];
    	p=G->adjlist[v].firstarc;
    	while(p!=NULL&&visit[p->adjvex]==1)
      	p=p->nextarc;
    	if(p==NULL)
        --top;
      else{
        //Visit(p->adjvex);
        visit[p->adjvex]=1;
        stack[++top]=p->adjvex;
      }
    }
  }
  ```
- ```c
  void BFS(AGraph *G, int v, int visit[maxSize]){
    ArcNode *p;
    int que[maxSize], front=0, rear=0;
    int j;
    //Visit(v);
    visit[v]=1;
    rear=(rear+1)%maxSize;
    que[rear]=v;
    while(front!=rear){
      front=(front+1)%maxSize;
      j=que[front];
      p=G->adjlist[j].firstarc;
      while(p!=NULL){
        if(visit[p->adjvex]==0){
          //Visit(p->adjvex);
          visit[p->adjvex]=1;
          rear=(rear+1)%maxSize;
          que[rear]=p->adjvex;
        }
        p=p->nextarc;
      }
    }
  }
  //顶点表入队遍历，搜索邻接顶点过程中每条边至少访问一次，邻接表O(n+e)，邻接矩阵O(n²)，DFS情况相同
  //对于同一个图，邻接矩阵遍历得到的BFS和DFS序列唯一，基于邻接表的结果可能不唯一
  ```

### 最小生成树（均针对无向图） 

- ```c
  void Prim(MGraph g, int v0, int &sum){                    //O(n²)
    int lowcost[maxSize], vset[maxSize], v;
    int i, j, k, min;
    v=v0;
    for(i=0;i<g.n;++i){
        lowcost[i]=g.edges[v0][i];
        vset[i]=0;
    }
    vset[v0]=1;
    sum=0;
    for(i=0;i<g.n-1;++i){
        min=INF;
        for(j=0;j<g.n;++j)                                  //侯选边中的最小者
            if(vset[j]==0&&lowcost[j]<min){
            min=lowcost[j];
            k=j;
            }
        vset[k]=1;
        v=k;
        sum+=min;
        for(j=0;j<g.n;++j)                                  //更新侯选边
            if(vset[j]==0&&g.edges[v][j]<lowcost[j])
            lowcost[j]=g.edges[v][j];
    }
  }
  ```

- ```c
  typedef struct{
    int a, b;
    int w;
  }Road;
  Road road[maxSize];
  int v[maxSize];
  int getRoot(int a){
    while(a!=v[a]) a=v[a];
    return a;
  }
  void Kruskal(MGraph g, int &sum, Road road[]){            //O(ElogE)利用并查集，适用于稀疏图
    int i;
    int N, E, a, b;
    N=g.n;
    E=g.e;
    sum=0;
    for(i=0;i<N;++i) v[i]=i;
    sort(road, E);
    for(i=0;i<E;++i){
      a=getRoot(road[i].a);
      b=getRoot(road[i].b);
      if(a!=b){
        v[a]=b;
        sum+=road[i].w;
      }
    }
  }
  ```

- 所有权值均不相等，或者有相等的边，但是在构造最小生成树的过程中权值相等的边都被并入生成树的图，其最小生成树唯一。充分条件：当图中各边权值互不相等时，MST 是唯一的

- Prim 适用于边稠密图，O(V^2^); Kruskal 适用于边稀疏而顶点较多的图 O(logE)

- 连通分量是无向图的极大连通子图，生成树是包含所有顶点的极小连通子图，极大极小是针对边的数目而言的

  - 连通分量的生成树构成非连通图的生成森林

  - 含有 n 个顶点的无向图，最多有 n 个连通分量

  - 在无向图中, 若从顶点 v1 到顶点 v2 有路径, 则称顶点 v1 与 v2 是连通的。如果图中任意一对顶点都是连通的,则称此图是**连通图**。

  - 强连通和弱连通的概念只在**有向图**中存在。

    **强连通图：**在有向图中, 若对于每一对顶点 v1 和 v2, 都存在一条从 v1 到 v2 和从 v2 到 v1 的**路径**,则称此图是强连通图。

    **弱连通图：**将有向图的所有的有向边替换为无向边，所得到的图称为原图的基图。如果一个有向图的基图是连通图，则有向图是弱连通图。

  - 其他概念
    - 简单图，多重图(有无重边和回边)
    - 完全图：任意两顶点之间都有边或两条方向相反的弧
    - 简单路径：顶点不重复。简单回路：除起点终点外顶点不重复

### 最短路径

- Dijkstra 用于某一顶点到其余各顶点的最短路径 O(n^2^)，Floyd 用于任意一对顶点间的最短路径，O(n^3^)

- Floyd 算法允许图中有带负权值的边，但不允许有包含带负权值的边组成的回路

- 求最长路径可以通过边权全部取负后求解最短路径

- ```c
  void Dijkstra(MGraph g, int v, int dist[], int path[]){//O(n²)
    int set[maxSize];
    int min, i, j, u;
    for(i=0;i<g.n;++i){
      dist[i]=g.edges[v][i];
      set[i]=0;
      if(g.edges[v][i]<INF)
        path[i]=v;
      else
        path[i]=-1;
    }
    set[v]=1;
    path[v]=-1;
    for(i=0;i<g.n-1;++i){
      min=INF;
      for(j=0;j<g.n;++j){
        if(set[j]=0&&dist[j]<min){
          u=j;
          min=dist[j];
        }
      }
      set[u]=1;
      for(j=0;j<g.n;++j){
        if(set[j]==0&&dist[u]+g.edges[u][j]<dist[j]){
          dist[j]=dist[u]+g.edges[u][j];
          path[j]=u;
        }
      }
    }
  }
  //手写表格：已并入的顶点|剩余的顶点|dist[]|path[]
  //path[]双亲表示法，借助栈逆向输出路径
  ```
- ```c
  void Floyd(MGraph *g, int Path[][maxSize], int A[][maxSize]){
    int i, j, k;
    for(i=0;i<g->n;++i)
      for(j=0;j<g->n;++j){
        A[i][j]=g->edges[i][j];
        Path[i][j]=-1;
      }
    for(k=0;k<g->n;++k)
      for(i=0;i<g->n;++i)
        for(j=0;j<g->n;++j)
          if(A[i][j]>A[i][k]+A[k][j]){
            A[i][j]>A[i][k]+A[k][j];
            Path[i][j]=k;
          }
  }
  void printPath(int u, int v, int path[][max], int A[][max]){
    if(A[u][v]==INF)

    else{
      if(path[u][v]==-1)
        //直接输出边<u, v>;
      else{
        int mid=path[u][v];
        printPath(u, mid, path);
        printPath(mid, v, path);
      }
    }
  }
  ```

### 拓扑排序

- AOV 网：Activity On Vertex network，以顶点表示活动，以边表示先后次序的有向无环图(DAG)

- ```c
    typedef struct{
      char data;
      int count;              //统计入度
      ArcNode *firstarc;
    }VNode;
    int TopSort(AGraph *G){//O(n+e)
      int i, j, n=0;
      int stack[maxSize], top=-1;
      ArcNode *p;
      for(i=0;i<G->n;++i)
        if(G->adjlist[i].count==0)
          stack[++top]=i;
      while(top!=-1){
        i=stack[top--];
        ++n;
        cout<<i<<" ";
        p=G->adjlist[i].firstarc;
        while(p!=NULL){
          j=p->adjvex;
          --(G->adjlist[j].count);
          if(G->adjlist[j].count==0)
            stack[++top]=j;
          p=p->nextarc;
        }
      }
      if(n==G->n) return 1;
      else return 0;
    }
    //使用C++STL
    queue<int>q;
    //priority_queue<int,vector<int>,greater<int>>q;
  //优先队列的话，会按照数值大小有顺序的输出
    //此处为了理解，暂时就用简单队列
    int topo(){//O(n+e)
      for(inti=1;i<=n;i++)
        if(indegree[i]==0)
          q.push(i);
        int temp;
        while(!q.empty()){
          temp=q.front();//如果是优先队列，这里可以是top()
          printf("%d->",temp);
          q.pop();
          for(inti=1;i<=n;i++)//遍历从temp出发的每一条边，入度--
            if(map[temp][i]){
              indegree[i]--;
              if(indegree[i]==0)
                q.push(i);
            }
        }
    }
  ```

- dfs 实现：顶点退栈序列即逆拓扑序列

### 关键路径

- AOE 网：Activity On Edge network，边表示活动，权值表示持续时间，顶点表示事件（新活动开始或旧活动结束的标志），DAG(Directed Acyclic Graph)

- 简单路径指序列中顶点不重复出现的路径

- 关键路径：源点到汇点所有路径中，最大路径长度&完成整个工期的最短时间，可能不唯一

- 核心算法

  - 事件的最早发生时间：源点到顶点 k 的路径中的最长者（ve，拓扑序列）

    指向它的边的权值加上发出这条边的事件的最早发生时间，多条取最大值

  - 事件的最迟发生时间：不推迟整个工程的前提下，该事件最迟必须发生的时间（vl，逆拓扑序列）

    由它所发出的边所指向的事件的最迟发生时间减去这条边的权值

  - 事件的最早发生时间就是由这个事件所发出的活动的最早发生时间（ve(边的起点)）

  - 事件的最迟发生时间减去以它为结束点的活动的持续时间就是活动的最迟发生时间（vl(边的终点) - 权值）

  - 活动的剩余时间 = 最迟发生时间 - 最早发生时间，剩余时间为 0 的活动就是关键活动

  - ve→vl→e→l→d=l-e

## 排序

- 分类
  - 插入类排序：直接插入排序，折半插入排序，希尔排序
  - 交换类排序：冒泡排序，快速排序
  - 选择类排序：简单选择排序，堆排序
  - 归并类排序：二路归并排序
  - 基数类排序：多关键字排序

### 插入类排序

- ```c
  //直接插入排序，时间复杂度最坏O(n²)、最好O(n)，空间O(1)
  void InsertSort(int R[], int n){
    int temp;
    for(i=1;i<n;++i){
      temp=R[i];
      j=i-1;
      while(j>=0&&temp<R[j]){
        R[j+1]=R[j];
        --j;
      }
      R[j+1]=temp;
    }
  }
  ```

- 折半插入排序：在直接插入排序基础上，用折半查找来查找插入位置，最好 O(nlogn)，最坏 O(n²)，空间 O(1)

  - 两者移动次数都取决于初始序列，相同；折半插入比较次数与序列初态无关，为 O(nlogn)，直接插入与序列初态有关，为 O(n)~O(n^2^)

- 希尔排序：又称缩小增量排序，按增量分割序列，使其基本有序，组内采取**直接插入排序**，增量选取规则：

  - 希尔(Shell)：$\lfloor n/2\rfloor...\lfloor n/2^k\rfloor...2、1$ O(n²)
  - 帕佩尔诺夫&斯塔舍维奇(Papernov & Stasevich)：$2^k+1...65、33、17、9、5、3、1$ O(n^1.5^)
  - 注意：增量序列的最后一个值一定取 1；尽量没有除 1 之外的公因子
  - 空间复杂度 O(1)

### 交换类排序

```c
//冒泡排序，最好O(n)，最坏O(n²)，空间O(1)
void BubbleSort(int R[], int n){
  int i, j, flag, temp;
  for(i=n-1;i>=1;--i){
    flag=0;
    for(j=1;j<=i;++j)
      if(R[j-1]>R[j]){
        temp=R[j];
        R[j]=R[j-1];
        R[j-1]=temp;
        flag=1;
      }
    if(flag==0)
      return;           //在一趟排序过程中没有发生关键字交换
  }
}
```

```c
  //快速排序，选择枢轴，最好O(nlogn)，最坏O(n²)，空间O(logn)，最坏O(n)
  //排序趟数和初始序列有关，序列有序情况下退化成冒泡，越有序越慢
  //基本操作执行次数的多项式最高次项为X*nlogn，快速排序的X最小，即在同级别算法中最好
  void QuickSort(int R[], int low, int high){
    int temp;
    int i=low, j=high;
    if(low<high){
      temp=R[low];
      while(i<j){
        while(j>i&&R[j]>=temp) --j;
        if(i<j){
          R[i]=R[j];
          ++i;
        }
        while(i<j&&R[i]<temp) ++i;
        if(i<j){
          R[j]=R[i];
          --j;
        }
      }
      R[i]=temp;
      QuickSort(R, low, i-1);
      QuickSort(R, i+1, high);
    }
  }
```

### 选择类排序

- ```c
  //简单选择排序，时间O(n²)，空间O(1)。稳定性：交换版（数组）不稳定（默认），插入版（链表）稳定
  void SelectSort(int R[], int n){
    int i, j, k;
    int temp;
    for(i=0;i<n;++i){
      k=i;
      for(j=i+1;j<n;++j)
        if(R[k]>R[j])
          k=j;
      temp=R[i];
      R[i]=R[k];
      R[k]=temp;
    }
  }
  ```

- ```c
  //堆排序，时间O(nlogn)，空间O(1)，用于逻辑上的完全二叉树，实际上用线性表存储
  //建堆、插入向上调整，筛选、删除向下调整，排序=建堆+删除堆顶
  void Sift(int R[], int low, int high){
    int i=low, j=2*i;
    int temp=R[i];
    while(j<=high){
      if(j<high&&R[j]<R[j+1])
        ++j;
      if(temp<R[j]){
        R[i]=R[j];
        i=j;
        j=2*i;
      }else
        break;
    }
    R[i]=temp;
  }
  void heapSort(int R[], int n){
    int i;
    int temp;
    for(i=n/2;i>=1;--i)
      Sift(R, i, n);
    for(i=n;i>=2;--i){
      temp=R[1];
      R[1]=R[i];
      R[i]=temp;
      Sift(R, 1, i-1);
    }
  }
  ```

### 二路归并排序

- ```c
  void mergeSort(int A[], int low, int high){//时间O(nlogn)，空间O(n)
    if(low<high){
      int mid=(low+high)/2;
      mergeSort(A, low, mid);
      mergeSort(A, mid+1, high);
      merge(A, low, mid, high);
    }
  }
  //merge()实际上通过辅助空间实现双指针选最小值填充原数组
  ```

### 基数排序

- eg. 桶排序
- 时间 O(d(n+r~d~))，空间 O(r~d~)，d 关键字位数，r~d~关键字基的个数
- 适用于序列中的关键字很多，但组成关键字的关键字的取值范围较小，通常用低位优先 LSD
- 最高位优先法 MSD，先根据最高位排成若干子序列，然后分别对这些子序列进行直接插入排序

### 外部排序

- 将内存作为工作空间来辅助外存数据的排序，常用归并排序，k 路归并算法
- 置换-选择排序：FI 读记录到 WA，选取最小值作为 MINIMAX 输出，再选比 MINIMAX 大的最小关键字作为新的 MINIMAX...直到选不出。得到一个初始归并段
- 最佳归并树：减少归并次数，I/O 次数=带权路径长度\*2→ 赫夫曼树，路径长度=其参加归并的次数
  - 添加虚段数规则与哈夫曼树不同：对于严格 m 叉树，n~0~=(m-1)n~m~+1，当(n~0~-1)%(m-1)=u=0 不需要添加，否则要添加**m-1-u**个空归并段
  - 总缓冲区数目要考虑需要一个输出缓冲区
- 败者树：提高选最值的效率（是一颗二叉树，高度不包含最上层选出的结点）
  - 叶子结点值为从当前参与归并段中读入的段首记录
  - 非叶子结点，度都为 2，值为叶子结点的序号：不仅可以找到记录值，还可以找到其所在的归并段
  - 建立和调整过程……
- 时间复杂度：
  1.  m 个初始归并段进行 k 路归并，归并的趟数为$\lceil log_km\rceil$
  2.  每一次归并，所有记录都要进行两次 I/O 操作
  3.  置换-选择排序中，所有记录都要进行两次 I/O 操作，选最值时间复杂度根据选择算法而定
  4.  k 路归并败者树的高度为$\lceil log_2k\rceil+1$，选最值需要$\lceil log_2k\rceil$次比较，即时间复杂度为 O(log~2~k)
  5.  k 路归并败者树的建树时间复杂度为 O(klog~2~k)
- 空间复杂度：O(1)
- 为减少外存读写次数->增大归并路数和减少归并段个数
  - 利用败者树增大归并路数
  - 利用置换选择排序增大归并段长度来减少归并段个数
  - 由长度不等的归并段，进行多路平衡归并，需要构造最佳归并树

### 总结

- ![img](/img/in-post/cs_learning/2022-12-20-postgraduate-exam/sorting.png)
- 不稳定的算法：**快**速，**希**尔，简单**选**择，**堆**
- 时间复杂度 O(nlog~2~n)的排序算法：**快**速，**希**尔，**归**并，**堆**。其他都是 O(n²)
- 空间复杂度：除快速 O(log~2~n)（栈空间），归并 O(n)，基数 O(r~d~)，其他都是 O(1)
- 直接插入排序和冒泡排序在初始序列已经有序的情况下可以达到 O(n)
- **经过一趟排序能保证一个关键字到达最终位置：交换类和选择类**
- 排序趟数和原始序列有关：交换类排序
- 元素比较次数和原始序列无关：简单选择排序和折半插入排序，基数排序
- 元素移动次数和原始序列无关：归并排序，基数排序
- 时间与原始序列无关：选择排序，堆排序，归并排序，基数排序
- 借助于“比较”进行排序的算法，最坏情况下的时间复杂度至少为 O(nlog~2~n)

## 查找

- 对关键词的平均比较次数：平均查找次数$ASL=\sum_{i=1}^{n}{p_i*c_i}$

- ```c
  //顺序查找略，成功ASL=(n+1)/2，失败ASL=n，时间复杂度均为O(n)
  //折半查找要求线性表有序（有序顺序表）
  int Bsearch(int R[], int low, int high, int k){
    int mid;
    while(low<=high){
      mid=(low+high)/2;
      if(R[mid]==k)
        return mid;
      else if(R[mid]>k)
        high=mid-1;
      else
        low=mid+1;
    }
    return 0;
  }
  ```

- 描述折半查找的判定树：mid 记录作为树根，左右子表分别作为左右子树，叶子结点代表查找不成功的位置。折半查找的比较次数即为从根结点到待查找元素所经过的结点数（包括根和自身），故时间复杂度用树的高度表示，O(log~2~n)，ASL 近似为 log~2~(n+1)-1，失败 ASL 一般不把对空指针的比较次数计算在内；折半查找法在查找不成功时和给定值进行比较的关键字个数最多为$\lfloor log_2n\rfloor+1$，即折半查找树的高度

- 分块查找：又称索引顺序查找，索引项由键值分量和链值分量组成，键值分量存放对应块的最大关键字，链值分量存放指向本块第一个元素和最后一个元素的指针，索引表中所有的索引项按照其关键字递增顺序排列。二分查找块+块内顺序查找，总 ASL=两次查找的 ASL 之和

### 二叉排序树 BST

- 构造：建立空树，逐个插入

- 查找，插入

- 删除：叶子结点……；单儿子……；既有左子树，又有右子树：用左子树的最右结点或右子树的最左结点(中序序列的前驱或后继)代替原删除结点，然后删除该结点

- 中序遍历序列非递减有序

- ```c
  //判断是否是二叉排序树
  int predt=INF;
  int judBST(BTNode *bt){
    int b1, b2;
    if(bt==NULL)
      return 1;
    else{
      b1=judBST(bt->lchild);
      if(b1==0||predt>bt->data)
        return 0;
      predt=bt->data;
      b2=judBST(bt->rchild);
      return b2;
    }
  }
  ```

### 平衡二叉树 AVL 树

- 左右子树高度之差的绝对值不超过 1 的**BST(**降低树的高度，提高查找效率)
- 平衡因子 = 左子树的高度 - 右子树的高度
- 只需调整最小不平衡子树
- LL, RR, LR, RR
- e.g. RR 调整中，中间结点的左子树挂到发现结点的右子树上

### B-树（B 树）

- B-树的阶 m=树中所有结点孩子结点个数的最大值，m≥3
  - 阶数是人为规定，而不是当前 B-树中结点所含有的最大分支数（未指明则用此值）
  - 每个结点最多有 m 个分支（子树），非空根结点至少有两个分支，非根非叶结点至少$\lceil m/2\rceil$个分支
  - n 个分支的结点有 n-1 个关键字，互不相等且按递增顺序排列
  - 结点中关键字个数的范围为$\lceil m/2\rceil-1$ ~ $m-1$
  - 叶结点处于同一层，可以用空指针表示，实际有结点，是查找失败到达的位置
  - B-树是要求所有叶结点在同一层的平衡 m 叉查找树(所有节点的平衡因子均等于0)
  - 高度是否包括叶子结点层？已知高度求结点数则要把叶子结点算在内
  - 下层结点内的关键词取值总是落在上层结点关键字所划分的区间内
- 查找：二叉排序树查找的扩展，结点内可使用**折半查找**提高效率
- 新关键字总是插入在终端结点上；连锁反应：插入一个关键字后多次拆分，根结点分裂则树高+1
- 插入操作只会使得 B-树逐渐变高而不会改变**叶子结点在同一**层的特性
- 删除关键字（在终端结点）
  - 结点内关键字个数大于$\lceil m/2\rceil-1$：直接删除
  - 关键字个数等于$\lceil m/2\rceil-1$，且其左右兄弟结点中存在关键字个数大于$\lceil m/2\rceil-1$的结点：向兄弟结点借关键字；否则进行合并，不同的合并方法产生不同的 B-树，使父结点关键字减少一个可能产生连锁反应
  - 非终端结点，用相邻关键字（左子树中值最大的关键字或右子树中值最小的关键字）取代后删除终端结点

### B+树

- B+树是应文件系统需要产生的 B-树的变形，更加适用于操作系统的文件索引和数据库索引，因为磁盘读写代价更低，查询效率更加稳定

- 具有 n 个关键字的结点含有 n 个分支

  | 结点中关键字个数 n 的取值范围 | B-树                               | B+树                           |
  | ----------------------------- | ---------------------------------- | ------------------------------ |
  | 根结点                        | $1\leq n\leq m-1$                  | $2\leq n\leq m$                |
  | 其他节点                      | $\lceil m/2\rceil-1\leq n\leq m-1$ | $\lceil m/2\rceil\leq n\leq m$ |

- B+树中叶子结点包含信息，且包含了全部关键字，叶子结点引出的指针指向记录

- B+树中，所有非叶子结点仅起到索引作用（包含子树的最大关键字和指向子树的指针，不含关键字对应记录的存储地址）；B-树中每个关键字对应一个记录的存储地址

- B+树上有一个指针指向关键字最小的叶子结点，所有叶子结点链接成一个线性链表

### 散列表（哈希表）

- 根据给定的关键字来计算出关键字在表中的地址

- 发生冲突：key1≠key2，而 H(key1)=H(key2)，即 key1 和 key2 是 Hash 函数 H 的同义词

- Hash 函数构造方法

  - 直接定址法：线性函数 H(key)=key 或 H(key)=a\*key+b
  - 数字分析法：事先知道可能出现的关键字，可选取关键字的若干数位组成 Hash 地址，要求所选数位上的数字尽可能是随机的，使得到的 Hash 地址尽量减少冲突
  - 平方取中法：取关键字平方后的中间几位作为 Hash 地址（和数的每一位都相关），位数由表长决定
  - 除留余数法：H(key)=key mod p，（p≤m）p 一般选择小于或等于表长的最大素数

- 冲突处理方法—尽量减少，不可能完全避免；边建表边检测处理冲突

  - 开放定址法：以冲突 Hash 地址为自变量，通过某种冲突解决函数得到新的空闲地址，**容易产生聚集现象**
    - 线性探测法：增量 1，H~i~(k)=(H(k)+i) Mod m (1≤i≤m-1)，容易产生堆积问题（不可避免!!!）
    - 平方探测法：增量 0, +1², -1², +2², -2², ... , ±k^2^不能保证探查到 Hash 表上的所有单元，但至少能探查到一半单元，k≤m/2，m 必须是一个可表示成 4k+3 的质数
    - 伪随机序列法，再 Hash 法 H(H(k))
  - 链地址法/拉链法：把所有的同义词用单链表连接起来，Hash 表每个单元中存放表头指针，**不会产生聚集现象**，适用于经常插入和删除的情况
  - 溢出区法：建立公共溢出区

- 性能分析：ASL~1~=找到表中已有表项的平均比较次数。ASL~2~=在表中找不到待查的表项，但找到插入位置的平均比较次数；在表中所有可能散列到的地址上插入新元素时，为找到空位置而进行探查的平均次数
- 哈希表的查找效率取决于散列函数、处理冲突的方法和装填因子(冲突概率与装填因子大小成正比)
- ASL~1~针对关键字计算，ASL~2~针对地址计算（空位置的比较次数计算在内）

  - ASL~2~的分母为散列函数的模，与散列表长无关，如 H=key mod 7，表长为 10，则分母取 7

- 顺序存储与空位置比较计算，链式存储与空指针比较不算

  | 装填因子 a=关键字个数/表长度 | ASL~1~        | ASL~2~         |
  | ---------------------------- | ------------- | -------------- |
  | 线性探查法                   | [1+1/(1-a)]/2 | [1+1/(1-a)²]/2 |
  | 平方探查法                   | -(1/a)ln(1-a) | 1/(1-a)        |
  | 链地址法                     | 1+a/2         | a+e^a^≈a       |

- 常见哈希结构：数组，set，map

    set：

    | 集合 | 底层实现 | 是否有序 | 数值是否可以重复 | 能否更改数值 | 查询效率 | 增删效率 |
    | --- | --- | --- | --- | --- | --- | --- |
    | std::set | 红黑树 | 有序 | 否 | 否 | O(log n) | O(log n) |
    | std::multiset | 红黑树 | 有序 | 是 | 否 | O(logn) | O(logn) |
    | std::unordered_set | 哈希表 | 无序 | 否 | 否 |O(1) | O(1) |

    std::unordered_set底层实现为哈希表，std::set 和std::multiset 的底层实现是红黑树，红黑树是一种平衡二叉搜索树，所以key值是有序的，但key不可以修改，改动key值会导致整棵树的错乱，所以只能删除和增加。

    map:

    | 映射 | 底层实现 | 是否有序 | 数值是否可以重复 | 能否更改数值 | 查询效率 | 增删效率 |
    | --- | --- | --- | --- | --- | --- | --- |
    | std::map | 红黑树 | key有序 | key不可重复 | key不可修改 | O(logn) | O(logn) |
    | std::multimap | 红黑树 | key有序 | key可重复 | key不可修改 | O(log n) | O(log n) |
    | std::unordered_map | 哈希表 | key无序 | key不可重复 | key不可修改 | O(1) | O(1) |

    std::unordered_map 底层实现为哈希表，std::map 和std::multimap 的底层实现是红黑树。同理，std::map 和std::multimap 的key也是有序的（这个问题也经常作为面试题，考察对语言容器底层的理解）。

## 其他

```c
// 汉诺塔分治法
void Han(int x, int y, int z, int n){     //代表将n个盘子分治地从x移到z上，y为辅助柱
  if(n==1) move(x,z);                     //n为1可以直接解决
  else{
    Han(x,z,y,n-1);                       //分治处理n-1个盘子，从x移动到y上，z为辅助柱
    move(x,z);                            //直接移动x上的一个盘子到z上
    Han(y,x,z,n-1);                       //分治处理n-1个盘子，从y移动到z上，x为辅助柱
  }
}
```
