title: 从3 sum问题说起
tags:
  - 3 sum problem
  - 最长公共子序列
id: 90
categories:
  - 算法设计
date: 2013-05-10 17:51:20
---

3sum<span style="font-family: 宋体;">问题就是</span><span style="font-family: 'Times New Roman';">3</span><span style="font-family: 宋体;">数求和问题，可以简单描述如下：给定数据集</span><span style="font-family: 'Times New Roman';">S</span><span style="font-family: 宋体;">，找出数据集中的三个元素</span><span style="font-family: 'Times New Roman';">a,b,c</span><span style="font-family: 宋体;">，满足</span><span style="font-family: 'Times New Roman';">a+b+c=k</span><span style="font-family: 宋体;">。直接可能没什么思路，其实</span><span style="font-family: 'Times New Roman';">3</span><span style="font-family: 宋体;">数求和问题可以看成是下面这个问题的扩展：给定数据集</span><span style="font-family: 'Times New Roman';">S</span><span style="font-family: 宋体;">，找出所有数据对</span><span style="font-family: 'Times New Roman';">a,b</span><span style="font-family: 宋体;">，满足</span><span style="font-family: 'Times New Roman';">a+b=k</span><span style="font-family: 宋体;">。</span>

其实对于原始问题的求解时间复杂度可以达到<span style="font-family: 'Times New Roman';">O(n)</span><span style="font-family: 宋体;">，如果数据集是已排好序的。分别在头部和尾部设置两个指针，求其和，如果大于</span><span style="font-family: 'Times New Roman';">K</span><span style="font-family: 宋体;">，则尾部的指针向头部移动，否则头部的指针向后移动。用</span><span style="font-family: 'Times New Roman';">C</span><span style="font-family: 宋体;">语言简单实现如下。</span>

`
void find2sum(int dataSet[], int itemNum, int key)
{
    int left=0;
    int right = itemNum-1;

    while(left<right){
        if(dataSet[left]+dataSet[right] == key)
            printf("The pair (%d %d) bingo!", left, right);
        else if(dataSet[left]+dataSet[right] < key)
            left ++;
        else
            right --;
    }
}
`

对上面的方法进行扩展就可以得到<span style="font-family: 'Times New Roman';">3</span><span style="font-family: 宋体;">数求和问题的解答了。</span>

`
void find3sum(int dataSet[], int itemNum, int key)
{
    int i, left, right;
    sort(dataSet);
    for(i=0; i<itemNum-2; i++){
        left    = i+1;
        right   = itemNum-1;
        while(left<right){
            if(sum(i, left, right) == key)
                printf("triplet %d %d %d bingo!\n", i, left, right);
            else if(sum(i, left, right) < key)
                left ++;
            else
                right ++;
        }
    }
}
`

如果文章从这儿就结束了，那也没有写的必要了。

前面求解两数求和问题的时候提到了数组是排好序的，利用这一特性我们可是可以大做文章了。例如：如何求解两个已排序数组的公共子序列？

首先说明下，公共子序列和公共子串是不同的。公共子串是连续的，而公共子序列则没有这一限制。比如｛<span style="font-family: 'Times New Roman';">1</span><span style="font-family: 宋体;">，</span><span style="font-family: 'Times New Roman';">2</span><span style="font-family: 宋体;">，</span><span style="font-family: 'Times New Roman';">3</span><span style="font-family: 宋体;">，</span><span style="font-family: 'Times New Roman';">4</span><span style="font-family: 宋体;">，</span><span style="font-family: 'Times New Roman';">5</span><span style="font-family: 宋体;">，</span><span style="font-family: 'Times New Roman';">6</span><span style="font-family: 宋体;">｝的子序列可以是｛</span><span style="font-family: 'Times New Roman';">1</span><span style="font-family: 宋体;">，</span><span style="font-family: 'Times New Roman';">3</span><span style="font-family: 宋体;">，</span><span style="font-family: 'Times New Roman';">6</span><span style="font-family: 宋体;">｝。</span>

我们不妨设置两个数组分别为<span style="font-family: 'Times New Roman';">A</span><span style="font-family: 宋体;">，</span><span style="font-family: 'Times New Roman';">B</span><span style="font-family: 宋体;">，长度分别为</span><span style="font-family: 'Times New Roman';">m</span><span style="font-family: 宋体;">，</span><span style="font-family: 'Times New Roman';">n</span><span style="font-family: 宋体;">。结合上文，很容易可以得到时间复杂度为</span><span style="font-family: 'Times New Roman';">O(m+n)</span><span style="font-family: 宋体;">的解法。分别设置两个指针指向两个数组，然后比较，将较小的指针往前移动即可。</span>

另外说到排序好的数组，一个自然反应应该就是二分查找了。对于数组<span style="font-family: 'Times New Roman';">A</span><span style="font-family: 宋体;">中的每个元素，对数组</span><span style="font-family: 'Times New Roman';">B</span><span style="font-family: 宋体;">进行二分查找，时间复杂度为</span><span style="font-family: 'Times New Roman';">O(mlgn)</span><span style="font-family: 宋体;">。其实这个讨论是有必要的，当</span><span style="font-family: 'Times New Roman';">m&lt;&lt;n</span><span style="font-family: 宋体;">时，这种方法明显优于上面的。</span>

其实个人觉得效率还可以更高，只要将上面两种方法结合起来，定性描述如下。第一种方法的思想，当找到<span style="font-family: 'Times New Roman';">A[i]=B[j]</span><span style="font-family: 宋体;">的时候，下一次对</span><span style="font-family: 'Times New Roman';">B</span><span style="font-family: 宋体;">进行二分查找的时候，只需要查找</span><span style="font-family: 'Times New Roman';">B[j+1, n-1]</span><span style="font-family: 宋体;">。</span>

`
void commonSeq(int A[], int B[], int m, int n)
{
    int index_A, index_B, result;

    index_A = index_B = 0;
    while(index_A<m && index_B<n){
        result = bisearch(B, index_B, n, A[index_A]);
        if(-1 == result)    /*使没查找到时返回-1*/
            index_A ++;
        else{
            printf("%d\t", A[index_A]);
            index_A ++;
            index_B = result+1;
        }
    }
    printf("\n");
}
`
还有一些，以后再说吧！
see you!