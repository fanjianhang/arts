# 第一周打卡

## Preview

很开心接受到研究生同学 @小丢 邀请加入的左耳学习交流活动，通过该活动获益匪浅，一方面可以学习同学们的优秀技术分享，了解各方面的技术，另外一方面，可以分享自己的所学内容，用这种方式把自己学到的知识与大家分享，共同讨论，即加深了对知识的理解，又可以让大家了解一些新知识。

## Algorithm

很久没接触算法了，也是较薄弱的一项，从简到难吧 : )

### 题目：455. Assign Cookies

Assume you are an awesome parent and want to give your children some cookies. But, you should give each child at most one cookie. Each child i has a greed factor gi, which is the minimum size of a cookie that the child will be content with; and each cookie j has a size sj. If sj >= gi, we can assign the cookie j to the child i, and the child i will be content. Your goal is to maximize the number of your content children and output the maximum number.

**Note:**
You may assume the greed factor is always positive. 
You cannot assign more than one cookie to one child.

**Example 1:**

```
Input: [1,2,3], [1,1]

Output: 1

Explanation: You have 3 children and 2 cookies. The greed factors of 3 children are 1, 2, 3. 
And even though you have 2 cookies, since their size is both 1, you could only make the child whose greed factor is 1 content.
You need to output 1.
```

**Example 2:**

```
Input: [1,2], [1,2,3]

Output: 2

Explanation: You have 2 children and 3 cookies. The greed factors of 2 children are 1, 2. 
You have 3 cookies and their sizes are big enough to gratify all of the children, 
You need to output 2.
```

### 解题思路：

先对greed以及factor排序，分别用i,j标志位置序号，依次对比是否满足size>=factor条件，如果满足，则max++，移动size序号，直到size遍历完或者factor遍历完即表示完成。输出max。

代码如下：

```java
class Solution {
    public int findContentChildren(int[] g, int[] s) {
        int max = 0;
        // sort greed and size
        Arrays.sort(g);
        Arrays.sort(s);
        int i = 0, j = 0;
        while (j < s.length && i < g.length) {
            if (s[j] >= g[i]) {
                i++;
                j++;
                max++;
            } else {
                j++;
            }
        }
        return max;
    }
}
```

## Review

- 原文地址：https://medium.com/@cscalfani/goodbye-object-oriented-programming-a59cda4c0e53

- 文中讲了对象编程的三大特性：封装性、继承性、多态性，并生动地表达了如果你想用面向对象思想编程会越来越困惑，例如你声明了一个类，你会想是不是需要再对其声明一个父类，然后又会想是不是还需要声明这个父类的一个父类，父类的父类。。。

- > The problem with object-oriented languages is they’ve got all this implicit environment that they carry around with them. You wanted a banana but what you got was a gorilla holding the banana and the entire jungle.    **Joe Armstrong**(The creator of Erlang)

- 引文举例香蕉、猴子、丛林，如果用面向对象编程思想的话，你想要一个香蕉，但你得到的是一只拿着香蕉和整个丛林的大猩猩。
- 作者建议我们使用函数式编程，https://en.wikipedia.org/wiki/Functional_programming

## Tip

最近在做一个项目，其中使用到了一个老版本开发的Node.js程序，而我本地又有其他项目使用了新版的Node.js，这时想“鱼和熊掌可兼得”，搜索一番，发现NVM可以方便的管理Node.js版本，分享给大家。

NVM(Node Version Manager)是一个Node.js版本管理器，windows版本开源地址：https://github.com/coreybutler/nvm-windows。有Linux/Windows/Darwin版本，可以在不同环境下方便进行Node.js版本管理，可以针对不同项目的Node.js版本随意切换，非常实用。

## Share

- A decentralized, peer-to-peer sharing economy, SQL database with Blockchain features. https://github.com/CovenantSQL/CovenantSQL