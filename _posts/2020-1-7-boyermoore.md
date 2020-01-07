---
layout:     post
title:      "Boyer-Moore算法"
subtitle:   "Boyer-Moore多数投票算法介绍"
date:       2020-1-7
author:     "tomylee"
header-img: "img/t015c96c4462fd21afb.jpg"
catalog: true
tags:
    - Leetcode 
    - C++
    - 算法
---

### 一、引入问题

#### 在做Leetcode时，遇到了一个求众数的问题，题目序号为169，难度为easy，题目中英文链接如下：[169.多数元素](https://leetcode.com/problems/majority-element/)，[169.Majority Element](https://leetcode.com/problems/majority-element/)。

#### 1.题目描述：给定一个大小为 n 的数组，找到其中的多数元素。多数元素是指在数组中出现次数大于`⌊ n/2 ⌋`的元素。你可以假设数组是非空的，并且给定的数组总是存在多数元素。
#### 2.示例：
```
输入：[3,2,3]
输出：3
```
```
输入: [2,2,1,1,1,2,2]
输出: 2
```

#### 3.自己的解：想到了直接使用sort排序和使用哈希表的方法。题目中已经明确说了存在所求元素，这两个方法是很容易想到的，也很直观。

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        /*//排序
        sort(nums.begin(),nums.end());        
        return nums[nums.size()/2];
        */
        //哈希表
        map<int,int> mp;
        for(int i=0;i<nums.size();i++) {
            mp[nums[i]]++;
            if(mp[nums[i]]>nums.size()/2) return nums[i];
        }
        return -1;
    }
};
```

#### 4.使用Boyer-Moore多数投票法。此题最为关键的就是注意这里的众数定义是一个数的出现次数大于一半，等于都不行，必须是大于一半，所以可以这样想如果众数为1，而其他数令为-1，则所有数之和一定大于0.因此，我们可以采取一种类似于同归于尽的方式（Boyer-Moore 投票算法）。即，我们从头开始令target为目标数字，每找到一个数，我们就令其个数为1，然后遍历之后的数字，如果与之相同则数目加一，如果不同则数目减一，此时应该注意，减一之后应该判断这个数字的个数是否为0如果为0就说明该数目已经抵消一个数字。然后重新选择一个目标数字target以此循环，到最后这个target一定为众数。因为数字之间相互抵消，众数是大于一半的，一个众数抵消一个其他数字也能把其他数字抵消完，最后剩下的一定是众数。同时应该注意，此题已经告诉众数已经存在，如果没有这个条件还需要在最后再遍历一遍判断target的数目是否过了一半。来自[同归于尽-Monica的小甜甜](https://leetcode-cn.com/problems/majority-element/solution/tong-gui-yu-jin-by-vailing/)

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int target = nums[0];
        int count = 1;
        for(int i = 1;i<nums.size();i++)
            if(nums[i]!=target)  
            {  
                count--;
                if(count==0)
                {
                    target = nums[i];
                    count = 1;
                }
            }else count++;
      return target;
    }
};
```

### 二、Boyer-Moore多数投票算法介绍

#### 1.*The majority vote problem is to determine in any given sequence of votes whether there is a candidate with more votes than all the others, and if so, to determine this candidate.  The Boyer-Moore majority vote algorithm solves the problem in time linear in the length of the sequence and constant memory. It does so in two repetitions. The first repetition eliminates all candidates butone. The second repetition verifies whether or not the remaining candidate holdsa majority. Given the postcondition of the first repetition, it is easy to implement the second repetition, and to verify its correctness.* --Wim H. Hesselink
#### 2.该算法空间复杂度为O(1)，时间复杂度为O(N)，只需要对原数组进行两趟扫描。第一趟扫描得到一个候选节点candidate，第二趟扫描判断candidate出现的次数是否大于`⌊ n/2 ⌋`。在第一个过程中生成一个单一的候选值，如果存在一个多数，这个候选值就是多数值。第二个过程只是计算该值的频率来确认是否正确。

第一趟扫描中需要记录2个值：
```
candidate，任意初始值；
count，初始值为0；
```
之后，对于数组中每一个元素，首先判断count是否为0，若为0，则把candidate设置为当前元素。之后判断candidate是否与当前元素相等，若相等则count+=1，否则count-=1。
#### 3.原理解析：为了解析算法的原理，我们只需考虑存在多数元素的情况，因为第二趟扫描可以检测出不存在多数元素的情况。
```
假设输入数组为[5, 5, 0, 0, 0, 5, 0, 0, 5]，0是多数元素。
首先，candidate被设置为第一个元素5，count为1。
由于5不是多数元素，必须找到另一个值与我们目前看到的每5个值配对，所以当扫描到数组某个位置时，count一定会减为0。
在例子中，当扫描到第四个位置时，count变成0。

count 值变化过程：
[1,2,1,0……
```
#### 当count变成0时，对于每一个出现的5，都用一个0与其进行抵消，所以消耗掉了与其一样多的0，而0是多数元素，这意味着当扫描到第四个位置时，我们已经最大程度的消耗掉了多数元素。

#### 然而，对于数组从第五个位置开始的剩余部分，0依然是其中的多数元素(注意，多数元素出现次数大于`⌊ n/2 ⌋`，而我们扫描过的部分中多数元素只占一半，那剩余部分中多数元素必然还是那个数字)。如果之前用于抵消的元素中存在非多数元素，那么数组剩余部分包含的多数元素就更多了。
#### 类似的，假设第一个数字就是多数元素，那么当count减为0时，我们消耗掉了与多数元素一样多的非多数元素，那么同样道理，数组剩余部分中的多数元素数值不变。

#### 这两种情况证明了关键的一点：数组中从candidate被赋值到count减到0的那一段可以被去除，余下部分的多数元素依然是原数组的多数元素。
#### 我们可以不断重复这个过程，直到扫描到数组尾部，那么count必然会大于0，而且这个count对应的candinate就是原数组的多数元素。


#### 4.参考资料

[多数投票算法(Boyer-Moore Algorithm)详解](https://blog.csdn.net/kimixuchen/article/details/52787307)

[Majority Voting Algorithm](https://gregable.com/2013/10/majority-vote-algorithm-find-majority.html)

[The Boyer-Moore Majority Vote Algorithm](http://www.cs.rug.nl/~wim/pub/whh348.pdf)
