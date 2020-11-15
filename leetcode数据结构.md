# 链表练习



## 19.删除链表的倒数第n个节点

给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。

示例：

给定一个链表: 1->2->3->4->5, 和 n = 2.

当删除了倒数第二个节点后，链表变为 1->2->3->5

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */


struct ListNode* removeNthFromEnd(struct ListNode* head, int n){
    struct ListNode *dummey = (struct ListNode *)malloc(sizeof(struct ListNode));
    dummey->val = 0;
    dummey->next = head;
    struct ListNode *fast = head, *slow = dummey;
    if (head == NULL) return head;
    while (n--) {
        fast = fast->next;
    }
    while(fast != NULL) {
        fast = fast->next;
        slow = slow->next;
    }
    slow->next = slow->next->next;
    struct ListNode *ans = dummey->next;
    free(dummey);
    return ans;
}
```



## 24.两两交换链表的节点

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

**你不能只是单纯的改变节点内部的值**，而是需要实际的进行节点交换。

```c
struct ListNode* swapPairs(struct ListNode* head){
    if (head == NULL || head->next == NULL) return head;
    struct ListNode *newnode = head->next;
    head->next = swapPairs(newnode->next);
    newnode->next = head;
    return newnode;
}
```



## 83.删除排序链表中的重复元素

给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。

示例 1:

输入: 1->1->2
输出: 1->2
示例 2:

输入: 1->1->2->3->3
输出: 1->2->3

```c
struct ListNode* deleteDuplicates(struct ListNode* head){
    if (head == NULL || head->next == NULL) return head;
    struct ListNode *p = head;
    while (p != NULL && p->next != NULL) {
        if (p->val == p->next->val) {
            p->next = p->next->next;
            continue;
        }
        p = p->next;
    }
    return head;
}
```





## 141.环形链表

给定一个链表，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

如果链表中存在环，则返回 true 。 否则，返回 false 。

```c
bool hasCycle(struct ListNode *head) {
    if (head == NULL || head->next == NULL) return false;
    struct ListNode *slow = head;
    struct ListNode *fast = head->next;
    while (slow != fast) {
        if (fast == NULL || fast->next == NULL) return false;
        slow = slow->next;
        fast = fast->next->next;
    }
    return true;
}
```



## 160.相交链表

编写一个程序，找到两个单链表相交的起始节点。

如下面的两个链表**：**

![image-20201109201229553](../linux系统编程笔记/image/image-20201109201229553.png)



在节点 c1 开始相交。

```c
struct ListNode *getIntersectionNode(struct ListNode *headA, struct ListNode *headB) {
    struct ListNode *nodeA = headA;
    struct ListNode *nodeB = headB;
    while (nodeA != nodeB) {
        nodeA = nodeA ? nodeA->next : headB;
        nodeB = nodeB ? nodeB->next : headA;
    }
    return nodeA;
}
```



## 202.快乐数

编写一个算法来判断一个数 n 是不是快乐数。

「快乐数」定义为：对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和，然后重复这个过程直到这个数变为 1，也可能是 无限循环 但始终变不到 1。如果 可以变为  1，那么这个数就是快乐数。

如果 n 是快乐数就返回 True ；不是，则返回 False 。

```c
nt num_pow(int n) {
    int m = 0;
    while (n) {
        m += (n % 10) * (n % 10);
        n /= 10;
    }
    return m;
}

bool isHappy(int n){
    if (n == 1) return true;
    int fast = n, slow = n;
    do {
        fast = num_pow(fast);
        fast = num_pow(fast);
        slow = num_pow(slow);
    } while(fast != slow);
    return slow == 1;
}
```



## 203.移除链表元素

删除链表中等于给定值 ***val\*** 的所有节点。

**示例:**

```
输入: 1->2->6->3->4->5->6, val = 6
输出: 1->2->3->4->5
```

```c
struct ListNode *removeElement(struct ListNode *head, int val) {
    if (head == NULL) return NULL;
    struct ListNode *dummey = (struct ListNode *)malloc(sizeof(struct ListNode));
    dummey->next = head;
    struct ListNode *p = head, *q = dummey;
    while (p) {
        if (p->val == val) {
            q->next = p->next;
        } else {
            q = p;
        }
        p = p->next;
    }
    p = dummey;
    free(dummey);
    return p;
}
```



## 206.反转链表

反转一个单链表。

**示例:**

```
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

```c
struct ListNode* reverseList(struct ListNode* head){
    if (head == NULL || head->next == NULL) return head;
    struct ListNode *cur = NULL, *per = head;
    while (per != NULL) {
        struct ListNode *next = per->next;
        per->next = cur;
        cur = per;
        per = next;
    }
    return cur;
}
```



## 234.回文链表

请判断一个链表是否为回文链表。

**示例 1:**

```
输入: 1->2
输出: false
```

**示例 2:**

```
输入: 1->2->2->1
输出: true
```

```c
bool isPalindrome(struct ListNode* head){
    int vals[50001], vals_num = 0;
    struct ListNode *p = (struct ListNode *)malloc(sizeof(struct ListNode));
    p = head;
    while (p != NULL) {
        vals[vals_num++] = p->val;
        p = p->next;
    }
    for (int i = 0, j = vals_num - 1; i < j; i++, j--) {
        if(vals[i] != vals[j]) {
            return false;
        }
    }
    return true;
}
```



## 237.删除链表中的节点


请编写一个函数，使其可以删除某个链表中给定的（非末尾）节点。传入函数的唯一参数为 **要被删除的节点** 。

 

现有一个链表 -- head = [4,5,1,9]，它可以表示为:

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/01/19/237_example.png)

 

**示例 1：**

```
输入：head = [4,5,1,9], node = 5
输出：[4,1,9]
解释：给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9.
```

```c
void deleteNode(struct ListNode* node) {
    node->val = node->next->val;
    node->next = node->next->next;
}
```





# 栈与队列练习

## 20.有效的括号

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。

示例 1:

输入: "()"
输出: true
示例 2:

输入: "()[]{}"
输出: true
示例 3:

输入: "(]"
输出: false
示例 4:

输入: "([)]"
输出: false
示例 5:

输入: "{[]}"
输出: true

```c
typedef struct Stack {
    int *data;
    int size, top;
} Stack;

char pairs(char c) {
    if (c == ')') return '(';
    if (c == ']') return '[';
    if (c == '}') return '{';
    return 0;
}

bool isValid(char * s){
    if (s == NULL) return false;
    int n = strlen(s);
    if (n % 2) return false;
    int str[n + 1], top = 0;
    for (int i = 0; i < n; i++) {
        char ch = pairs(s[i]);
        if (ch) {
            if (top == 0 || str[top - 1] != ch) {
                return false;
            }
            top--;
        } else {
            str[top++] = s[i];
        }
    }
    return top == 0;
}
```



42.



84.

221.

239.

232.

225.



# 树与二叉树

## 100.相同的树

给定两个二叉树，编写一个函数来检验它们是否相同。

如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。

示例 1:

输入:       1          1
               / \         / \
              2   3     2   3

        [1,2,3],   [1,2,3]

输出: true
示例 2:

输入:      1          1
               /           \
              2             2

        [1,2],     [1,null,2]

输出: false



```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */


bool isSameTree(struct TreeNode* p, struct TreeNode* q){
    if (p == NULL && q == NULL) {
        return true;
    } else if (p == NULL || q == NULL) {
        return false;
    } else if (p->val != q->val) {
        return false;
    } else {
        return isSameTree(p->left, q->left) && isSameTree(p->right, q->right);
    }
}
```



## 101.对称二叉树

给定一个二叉树，检查它是否是镜像对称的。

 

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。

    1
   / \
  2   2
 / \ / \
3  4 4  3


但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

    1
   / \
  2   2
   \   \
   3    3

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */

int isMirror (struct TreeNode *lchild, struct TreeNode *rchild) {
    if (lchild == NULL && rchild == NULL) return 1;
    if (lchild == NULL || rchild == NULL) return 0;
    if (lchild->val == rchild->val) {
        return isMirror(lchild->left, rchild->right) && isMirror(lchild->right, rchild->left);
    } else {
        return 0;
    }
}

bool isSymmetric(struct TreeNode* root){
    return isMirror(root, root);
}
```

