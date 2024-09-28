# 一、数组
### 理论知识
1. 数组是存放在**连续内存空间**上一组**相同类型数据**的集合，下标从0开始
2. 数组元素不能删除只能覆盖
3. 二维数组是否使用连续内存空间根据不同语言有不同情况
### 二分查找
满足两点:**有序递增**数组; **无重复**元素
> 左闭右闭区间二分写法[left, right]
```python
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1  # 定义target在左闭右闭的区间里，[left, right]

        while left <= right:
            middle = left + (right - left) // 2
            
            if nums[middle] > target:
                right = middle - 1  # target在左区间，所以[left, middle - 1]
            elif nums[middle] < target:
                left = middle + 1  # target在右区间，所以[middle + 1, right]
            else:
                return middle  # 数组中找到目标值，直接返回下标
        return -1  # 未找到目标值
```
>左闭右开区间二分写法[left, right)
```python
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums)  # 定义target在左闭右开的区间里，即：[left, right)

        while left < right:  # 因为left == right的时候，在[left, right)是无效的空间，所以使用 <
            middle = left + (right - left) // 2

            if nums[middle] > target:
                right = middle  # target 在左区间，在[left, middle)中
            elif nums[middle] < target:
                left = middle + 1  # target 在右区间，在[middle + 1, right)中
            else:
                return middle  # 数组中找到目标值，直接返回下标
        return -1  # 未找到目标值

```
题目链接
[704. 二分查找 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-search/description/)
[35. 搜索插入位置 - 力扣（LeetCode）](https://leetcode.cn/problems/search-insert-position/description/)
[34. 在排序数组中查找元素的第一个和最后一个位置 - 力扣（LeetCode）](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/description/)
[69. x 的平方根 - 力扣（LeetCode）](https://leetcode.cn/problems/sqrtx/description/)
[367. 有效的完全平方数 - 力扣（LeetCode）](https://leetcode.cn/problems/valid-perfect-square/description/)
### 双指针(移除数组元素)

 - 快慢指针法
 - 对撞指针法

题目链接
[27. 移除元素 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-element/description/)双指针算法
[26. 删除有序数组中的重复项 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/description/)
[283. 移动零 - 力扣（LeetCode）](https://leetcode.cn/problems/move-zeroes/description/)
[844. 比较含退格的字符串 - 力扣（LeetCode）](https://leetcode.cn/problems/backspace-string-compare/description/)
[977. 有序数组的平方 - 力扣（LeetCode）](https://leetcode.cn/problems/squares-of-a-sorted-array/description/)
### 滑动窗口
- 滑动窗口也可以理解为双指针法的一种
- 不断的调节子序列的**起始位置和终止位置（即滑动窗口）**，从而得出我们要想的结果

题目链接
[209. 长度最小的子数组 - 力扣（LeetCode）](https://leetcode.cn/problems/minimum-size-subarray-sum/description/)
[904. 水果成篮 - 力扣（LeetCode）](https://leetcode.cn/problems/fruit-into-baskets/description/)
[76. 最小覆盖子串 - 力扣（LeetCode）](https://leetcode.cn/problems/minimum-window-substring/description/)  复习
### 螺旋矩阵（经典题型非算法）
- 并不涉及算法，就是模拟过程，考察对代码的掌控能力
- 这四条边怎么画，每画一条边都要**坚持一致的左闭右开**，或者左开右闭的原则，这样这一圈才能按照统一的规则画下来

题目链接
[59. 螺旋矩阵 II - 力扣（LeetCode）](https://leetcode.cn/problems/spiral-matrix-ii/description/)优雅简直是太优雅了
[54. 螺旋矩阵 - 力扣（LeetCode）](https://leetcode.cn/problems/spiral-matrix/description/)
### 前缀和
 - 一维前缀和，用于快速统计数组前n项和或者计算区间和
 - 二维前缀和，用于快速统计子矩阵和
# 二、链表
### 理论知识

 - 单链表：head指针作为链表入口，通过指针串联在一起的线性结构，每一个节点由两部分组成，一个是数据域一个是指针域（存放指向下一个节点的指针），最后一个节点的指针域指向null
双链表：每一个节点有两个指针域，一个指向下一个节点，一个指向上一个节点
循环链表：单链表首尾相连，head指针作为链表入口
 - 链表中的节点在内存中不是连续分布的 ，而是散乱分布在内存中的某地址上，分配机制取决于操作系统的内存管理
 - 节点构造函数，删除节点时c、c++要从内存中释放节点，python、Java由对应的内存管理垃圾回收机制，不用释放
```python
class ListNode:
    def __init__(self, val, next=None):
        self.val = val
        self.next = next
```
有些题目需要虚拟头结点有些不需要
题目链接：
[203. 移除链表元素 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-linked-list-elements/description/)优雅地设置dummy_head
[707. 设计链表 - 力扣（LeetCode）](https://leetcode.cn/problems/design-linked-list/description/)再设计单、双链表
[206. 反转链表 - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-linked-list/description/)双指针应用
[24. 两两交换链表中的节点 - 力扣（LeetCode）](https://leetcode.cn/problems/swap-nodes-in-pairs/description/)
[19. 删除链表的倒数第 N 个结点 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/description/)双(快慢)指针应用于链表
[面试题 02.07. 链表相交 - 力扣（LeetCode）](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/description/)优雅
[142. 环形链表 II - 力扣（LeetCode）](https://leetcode.cn/problems/linked-list-cycle-ii/description/)判断是否有环；判断环的入口节点
# 三、哈希表
### 理论知识
 - 哈希表是根据关键码的值而直接进行访问的数据结构。
 - 哈希碰撞有两种解决方法， 拉链法（元素以链表形式存储）和线性探测法。
 -  使用哈希法来解决问题有三种数据结构：数组 、set 集合、map映射
 - 判断一个元素是否出现过的场景也应该第一时间想到哈希法
### 题目链接
[242. 有效的字母异位词 - 力扣（LeetCode）](https://leetcode.cn/problems/valid-anagram/description/)
[349. 两个数组的交集 - 力扣（LeetCode）](https://leetcode.cn/problems/intersection-of-two-arrays/description/)
[454. 四数相加 II - 力扣（LeetCode）](https://leetcode.cn/problems/4sum-ii/) 复杂度最低n²
[383. 赎金信 - 力扣（LeetCode）](https://leetcode.cn/problems/ransom-note/description/)
[15. 三数之和 - 力扣（LeetCode）](https://leetcode.cn/problems/3sum/description/)双指针 + 去重 + 剪枝
[18. 四数之和 - 力扣（LeetCode）](https://leetcode.cn/problems/4sum/description/)
# 字符串
### 题目链接
[344. 反转字符串 - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-string/) 碰撞指针
[541. 反转字符串 II - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-string-ii/description/)
### KMP算法
 - KMP的主要思想是**当出现字符串不匹配时，可以知道一部分之前已经匹配的文本内容，可以利用这些信息避免从头再去做匹配了**
 - **前缀表**是用来回退的，它记录了模式串与主串(文本串)不匹配的时候，模式串应该从哪里开始重新匹配, **记录下标i之前（包括i）的字符串中，有多大长度的相同前缀后缀。**
 - 前缀表的实现使用快慢指针
 - [28. 找出字符串中第一个匹配项的下标 - 力扣（LeetCode）](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/description/)
 - [459. 重复的子字符串 - 力扣（LeetCode）](https://leetcode.cn/problems/repeated-substring-pattern/description/)
# 四、栈与队列
### 题目链接
[232. 用栈实现队列 - 力扣（LeetCode）](https://leetcode.cn/problems/implement-queue-using-stacks/description/)
[225. 用队列实现栈 - 力扣（LeetCode）](https://leetcode.cn/problems/implement-stack-using-queues/description/)
[20. 有效的括号 - 力扣（LeetCode）](https://leetcode.cn/problems/valid-parentheses/description/)栈实现
[1047. 删除字符串中的所有相邻重复项 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/description/)栈实现, 相邻重复项，不是连续重复项
[150. 逆波兰表达式求值 - 力扣（LeetCode）](https://leetcode.cn/problems/evaluate-reverse-polish-notation/description/)
[239. 滑动窗口最大值 - 力扣（LeetCode）](https://leetcode.cn/problems/sliding-window-maximum/description/)单调队列经典题型，可以衍生为求任意子矩阵的极值
[347. 前 K 个高频元素 - 力扣（LeetCode）](https://leetcode.cn/problems/top-k-frequent-elements/)优先级队列，采用小顶堆
# 五、二叉树
### 理论知识
- 满二叉树：深度为k，有2^k-1个节点的二叉树
- 完全二叉树：只有最底层节点没有排满（从左向右排列）
- 二叉搜索（排序）树：(中序遍历能得到一个有序数组)
- -   若它的左子树不空，则左子树上所有结点的值均小于它的根结点的值；
- -   若它的右子树不空，则右子树上所有结点的值均大于它的根结点的值；
-  - 它的左、右子树也分别为二叉排序树
- 平衡二叉搜索树：又被称为AVL树，且具有以下性质：它是一棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。
- 二叉树可以链式存储，也可以顺序存储。
- 二叉树主要有两种遍历方式：深度优先遍历(采用栈结构)和广度优先遍历（采用队列结构）
-   深度优先遍历**这里前中后，其实指的就是中间节点的遍历顺序**
    -   前序遍历（递归法，迭代法**重要** ）
    -   中序遍历（递归法，迭代法**重要**）
    -   后序遍历（递归法，迭代法）
-   广度优先遍历
    -   层次遍历（迭代法）
```python
class TreeNode:
    def __init__(self, val, left = None, right = None):
        self.val = val
        self.left = left
        self.right = right
```
### 二叉树统一迭代法
以前序遍历为例子
```python
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
    if not root:
	    return []
	ans = []
	stack = [root]
	while stack:
		cur = stack.pop()
		if cur != None:
			if cur.right:
				stack.append(cur.right)
			if cur.left:
				stack.append(cur.left)
			stack.append(cur)
			stack.append(None)
		else:
			cur = stack.pop()
			ans.append(cur.val)
	return ans
```
### 二叉树的层次遍历题目链接
[199. 二叉树的右视图 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-right-side-view/description/)
[116. 填充每个节点的下一个右侧节点指针 - 力扣（LeetCode）](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/)
### 题目链接
[101. 对称二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/symmetric-tree/description/)
[617. 合并二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/merge-two-binary-trees/)与上一题类似，但上一题可以用栈完成，本题最好用队列完成
[257. 二叉树的所有路径 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-paths/description/)前序遍历节点，并采用堆栈迭代。也可以递归写
[112. 路径总和 - 力扣（LeetCode）](https://leetcode.cn/problems/path-sum/)与上一题类似
[106. 从中序与后序遍历序列构造二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/description/)**典型递归题目**
[654. 最大二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-binary-tree/submissions/563483982/)与上一题类似
###  二叉搜索树题目链接 
[700. 二叉搜索树中的搜索 - 力扣（LeetCode）](https://leetcode.cn/problems/search-in-a-binary-search-tree/description/)
[98. 验证二叉搜索树 - 力扣（LeetCode）](https://leetcode.cn/problems/validate-binary-search-tree/description/)中序遍历这个二叉搜索树
[501. 二叉搜索树中的众数 - 力扣（LeetCode）](https://leetcode.cn/problems/find-mode-in-binary-search-tree/description/)双指针法＋maxcount+结果数组
[236. 二叉树的最近公共祖先 - 力扣（LeetCode）](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/description/)**优雅地递归**回溯，不适合使用迭代法
[450. 删除二叉搜索树中的节点 - 力扣（LeetCode）](https://leetcode.cn/problems/delete-node-in-a-bst/)**使用递归**做法较为规范
[669. 修剪二叉搜索树 - 力扣（LeetCode）](https://leetcode.cn/problems/trim-a-binary-search-tree/description/)递归
[108. 将有序数组转换为二叉搜索树 - 力扣（LeetCode）](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/description/)
[538. 把二叉搜索树转换为累加树 - 力扣（LeetCode）](https://leetcode.cn/problems/convert-bst-to-greater-tree/description/)关键在于设置一个记录上一个节点和的全局变量
# 六、递归回溯法
### 回溯法解决的问题

-   组合问题：N个数里面按一定规则找出k个数的集合
-   切割问题：一个字符串按一定规则有几种切割方式
-   子集问题：一个N个数的集合里有多少符合条件的子集
-   排列问题：N个数按一定规则全排列，有几种排列方式
-   棋盘问题：N皇后，解数独等等
### 回溯法模板
**所有回溯法解决的问题都可以抽象为树形结构（N叉树）**，因此与树的递归三部曲相同，回溯三部曲为：
-   回溯函数模板返回值以及参数
-   回溯函数终止条件
-   回溯搜索的遍历过程

回溯算法模板框架
```
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
```
**for循环可以理解是横向遍历，backtracking（递归）就是纵向遍历**，这样就把这棵树全遍历完了，一般来说，搜索叶子节点就是找的其中一个结果了
### 题目链接

 1. 组合问题

[77. 组合 - 力扣（LeetCode）](https://leetcode.cn/problems/combinations/description/)
[216. 组合总和 III - 力扣（LeetCode）](https://leetcode.cn/problems/combination-sum-iii/description/)
[17. 电话号码的字母组合 - 力扣（LeetCode）](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/description/)
[39. 组合总和 - 力扣（LeetCode）](https://leetcode.cn/problems/combination-sum/description/)可重复数字的组合数
[40. 组合总和 II - 力扣（LeetCode）](https://leetcode.cn/problems/combination-sum-ii/description/)重复组合数去重操作，**去重的是同一树层上的“使用过”，同一树枝上的都是一个组合里的元素，不用去重**

 2. 分割问题
 
[131. 分割回文串 - 力扣（LeetCode）](https://leetcode.cn/problems/palindrome-partitioning/description/)
[93. 复原 IP 地址 - 力扣（LeetCode）](https://leetcode.cn/problems/restore-ip-addresses/description/)

 3. 子集问题

[78. 子集 - 力扣（LeetCode）](https://leetcode.cn/problems/subsets/description/)与组合分割排列问题不同，组合分割排列问题是要找到树上的叶子节点，子集问题是遍历整棵树找到所有的节点
[90. 子集 II - 力扣（LeetCode）](https://leetcode.cn/problems/subsets-ii/submissions/564925729/)重复子集的去重操作，与40题思想相同
[491. 非递减子序列 - 力扣（LeetCode）](https://leetcode.cn/problems/non-decreasing-subsequences/submissions/564942546/)去重操作不同于上面，因为不是对有序数组操作，所以要借用集合判断元素是否出现过

4. 排列问题

[46. 全排列 - 力扣（LeetCode）](https://leetcode.cn/problems/permutations/description/)增加了数组used, 用来记录数字是否已经被使用
[47. 全排列 II - 力扣（LeetCode）](https://leetcode.cn/problems/permutations-ii/description/)

5. 特殊问题

[332. 重新安排行程 - 力扣（LeetCode）](https://leetcode.cn/problems/reconstruct-itinerary/description/)

6. 棋盘问题
[51. N 皇后 - 力扣（LeetCode）](https://leetcode.cn/problems/n-queens/description/)
[37. 解数独 - 力扣（LeetCode）](https://leetcode.cn/problems/sudoku-solver/description/)
# 七、贪心算法
### 理论基础
1. **贪心的本质是选择每一阶段的局部最优，从而达到全局最优**。
2. 如何验证可不可以用贪心算法呢？
**最好用的策略就是举反例，如果想不到反例，那么就试一试贪心**
3. 题目技巧性强，有时难以看出使用贪心算法
### 题目链接
[376. 摆动序列 - 力扣（LeetCode）](https://leetcode.cn/problems/wiggle-subsequence/description/)需要考虑平谷的多种情况
[53. 最大子数组和 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-subarray/description/)
[122. 买卖股票的最佳时机 II - 力扣（LeetCode）](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/description/)
[55. 跳跃游戏 - 力扣（LeetCode）](https://leetcode.cn/problems/jump-game/description/)
[45. 跳跃游戏 II - 力扣（LeetCode）](https://leetcode.cn/problems/jump-game-ii/description/)与上一题一样都是利用覆盖范围，但是解法不同
[1005. K 次取反后最大化的数组和 - 力扣（LeetCode）](https://leetcode.cn/problems/maximize-sum-of-array-after-k-negations/description/)
[134. 加油站 - 力扣（LeetCode）](https://leetcode.cn/problems/gas-station/description/)
[135. 分发糖果 - 力扣（LeetCode）](https://leetcode.cn/problems/candy/description/)这道题目一定是要确定一边之后，再确定另一边，如果两边一起考虑一定会顾此失彼
[860. 柠檬水找零 - 力扣（LeetCode）](https://leetcode.cn/problems/lemonade-change/description/)
[406. 根据身高重建队列 - 力扣（LeetCode）](https://leetcode.cn/problems/queue-reconstruction-by-height/description/)
[452. 用最少数量的箭引爆气球 - 力扣（LeetCode）](https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/description/)找气球的右边界
[435. 无重叠区间 - 力扣（LeetCode）](https://leetcode.cn/problems/non-overlapping-intervals/description/)
[763. 划分字母区间 - 力扣（LeetCode）](https://leetcode.cn/problems/partition-labels/description/)技巧性强, 看起来切割字母串需要用回溯其实不需要
[738. 单调递增的数字 - 力扣（LeetCode）](https://leetcode.cn/problems/monotone-increasing-digits/description/)从后向前遍历，num[i - 1] = num[i -1] -1且num[i] = 9
[968. 监控二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-cameras/description/)后序遍历+设置节点状态的贪心  难
# 八、动态规划
**五部曲:**
1.  确定dp数组（dp table）以及下标的含义
2.  确定递推公式
3.  dp数组如何初始化
4.  确定遍历顺序
5.  举例推导dp数组

**如果代码写出来了，一直AC不了，灵魂三问：**

1.  这道题目我举例推导状态转移公式了么？
2.  我打印dp数组的日志了么？
3.  打印出来了dp数组和我想的一样么？
### 题目链接
一维dp数组，但是可能存在二维循环
[63. 不同路径 II - 力扣（LeetCode）](https://leetcode.cn/problems/unique-paths-ii/description/)
[343. 整数拆分 - 力扣（LeetCode）](https://leetcode.cn/problems/integer-break/description/)
[96. 不同的二叉搜索树 - 力扣（LeetCode）](https://leetcode.cn/problems/unique-binary-search-trees/description/)推导🔪✍困难🔞

**二维dp数组01背包问题**
[416. 分割等和子集 - 力扣（LeetCode）](https://leetcode.cn/problems/partition-equal-subset-sum/description/)能用回溯，但会超时。是01背包问题的变体
[1049. 最后一块石头的重量 II - 力扣（LeetCode）](https://leetcode.cn/problems/last-stone-weight-ii/description/)与上一题类似，但是很难转换成01背包问题
[494. 目标和 - 力扣（LeetCode）](https://leetcode.cn/problems/target-sum/)完全想不出背包问题的bagsize。。。还要考虑num[i]为0的特殊情况
[474. 一和零 - 力扣（LeetCode）](https://leetcode.cn/problems/ones-and-zeroes/description/)虽然还是用二维dp数组，但是是三维01背包问题

**完全背包问题**
完全背包问题与01背包问题区别就是一维dp数组遍历顺序不一样，01背包逆序遍历dp[], 完全背包正序遍历dp[]
**如果求组合数就是外层for循环遍历物品，内层for遍历背包**
**如果求排列数就是外层for遍历背包，内层for循环遍历物品**
**爬楼梯问题本质上也是完全背包问题**，且是求排列数问题
[518. 零钱兑换 II - 力扣（LeetCode）](https://leetcode.cn/problems/coin-change-ii/description/)求组合数总个数
[377. 组合总和 Ⅳ - 力扣（LeetCode）](https://leetcode.cn/problems/combination-sum-iv/description/)求排列数总个数
[322. 零钱兑换 - 力扣（LeetCode）](https://leetcode.cn/problems/coin-change/description/)没有强调组合数还是排列数，只要求最小个数，所以内外层循环顺序无所谓
[139. 单词拆分 - 力扣（LeetCode）](https://leetcode.cn/problems/word-break/description/)可以用回溯做
### 背包递推公式
问能否能装满背包（或者最多装多少）：dp[j] = max(dp[j], dp[j - nums[i]] + nums[i])
问装满背包有几种方法：dp[j] += dp[j - nums[i]] 
问背包装满最大价值：dp[j] = max(dp[j], dp[j - weight[i]] + value[i]); 
问装满背包所有物品的最小个数：dp[j] = min(dp[j - coins[i]] + 1, dp[j])


------
[213. 打家劫舍 II - 力扣（LeetCode）](https://leetcode.cn/problems/house-robber-ii/description/)环形dp解法
[337. 打家劫舍 III - 力扣（LeetCode）](https://leetcode.cn/problems/house-robber-iii/description/)树形dp = 递归 + dp状态分析
[121. 买卖股票的最佳时机 - 力扣（LeetCode）](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/description/)
[123. 买卖股票的最佳时机 III - 力扣（LeetCode）](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iii/description/)
[188. 买卖股票的最佳时机 IV - 力扣（LeetCode）](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iv/description/)
[309. 买卖股票的最佳时机含冷冻期 - 力扣（LeetCode）](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/description/)关注dp状态
[714. 买卖股票的最佳时机含手续费 - 力扣（LeetCode）](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/description/)

---
[300. 最长递增子序列 - 力扣（LeetCode）](https://leetcode.cn/problems/longest-increasing-subsequence/description/)
[718. 最长重复子数组 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-length-of-repeated-subarray/description/)
[1143. 最长公共子序列 - 力扣（LeetCode）](https://leetcode.cn/problems/longest-common-subsequence/description/)
[1035. 不相交的线 - 力扣（LeetCode）](https://leetcode.cn/problems/uncrossed-lines/)最长公共子序列的变体
[53. 最大子数组和 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-subarray/description/)
[392. 判断子序列 - 力扣（LeetCode）](https://leetcode.cn/problems/is-subsequence/description/)
[115. 不同的子序列 - 力扣（LeetCode）](https://leetcode.cn/problems/distinct-subsequences/description/)
[583. 两个字符串的删除操作 - 力扣（LeetCode）](https://leetcode.cn/problems/delete-operation-for-two-strings/description/)
[72. 编辑距离 - 力扣（LeetCode）](https://leetcode.cn/problems/edit-distance/description/)经典例题，当元素不相等时，找增删改三种操作中操作数最小值
[647. 回文子串 - 力扣（LeetCode）](https://leetcode.cn/problems/palindromic-substrings/description/)
[516. 最长回文子序列 - 力扣（LeetCode）](https://leetcode.cn/problems/longest-palindromic-subsequence/description/)
# 八、单调栈
**通常是一维数组，要寻找任一个元素的右边或者左边第一个比自己大或者小的元素的位置，此时我们就要想到可以用单调栈了**。时间复杂度为O(n)。
[739. 每日温度 - 力扣（LeetCode）](https://leetcode.cn/problems/daily-temperatures/description/)
[496. 下一个更大元素 I - 力扣（LeetCode）](https://leetcode.cn/problems/next-greater-element-i/description/)
[503. 下一个更大元素 II - 力扣（LeetCode）](https://leetcode.cn/problems/next-greater-element-ii/description/)
[42. 接雨水 - 力扣（LeetCode）](https://leetcode.cn/problems/trapping-rain-water/description/)使用列求雨水使用双指针，使用行求雨水使用单调栈
[84. 柱状图中最大的矩形 - 力扣（LeetCode）](https://leetcode.cn/problems/largest-rectangle-in-histogram/description/)要在原height数组前后各加一个0
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwODgxMTQ3NTIsLTIxMzMwMzA4OTAsLT
QxNzA3NDQ5NCw2OTU3OTMwNDQsLTIxOTM5NDg5MiwtMzY2NDU5
MTQsLTEwNjQyODY1MzUsMTcxMTUyNDIzOCw2ODYyOTA0OTIsLT
Q0MTc2NjI1NywtNDE5MDQyMDAyLC0yMDA0NzM1MDc2LC0yMDQ2
OTc4NDI2LC03MTY3ODMwOTYsLTEwNjE0MjEzMjcsLTQyOTQ0ND
Y2NCwtMTQ3NjA5NTQ3OSwtMTg3MTc2MTM1NCwtNTM5OTU1Njk5
LDE5MDQxNTM0Ml19
-->