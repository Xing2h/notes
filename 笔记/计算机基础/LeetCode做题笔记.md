# 难度：简单

## 26. 删除有序数组中的重复项

**题目要求：**

给你一个 升序排列 的数组 nums ，请你 原地 删除重复出现的元素，使每个元素 只出现一次 ，返回删除后数组的新长度。元素的 相对顺序 应该保持 一致 。

由于在某些语言中不能改变数组的长度，所以必须将结果放在数组nums的第一部分。更规范地说，如果在删除重复项之后有 k 个元素，那么 nums 的前 k 个元素应该保存最终结果。

将最终结果插入 nums 的前 k 个位置后返回 k 。

不要使用额外的空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

**思路：**从前往后查找，把重复元素用后面的非重复元素替换，查找完以后将重复元素数组后部的重复序列删掉，这里需要一个变量来记录前面非重复序列的尾部；

>   注：向量好像只能从最后面删，所以需要一个循环来pop数组末尾的元素

**代码：**

```C++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int i,k=0;
        for(i=1;i<nums.size();i++){//遍历序列
            if(nums[i]!=nums[k])//非重复元素
                nums[++k]=nums[i];//换到前面
        }
        int n=nums.size()-k-1;//非重复序列的长度
        for(int j=0;j<n;j++){//依次删除末尾元素
            nums.pop_back();
        }
        return nums.size();//返回数组大小
    }
};
```

# 27. 移除元素

**题目要求：**

给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

**思路：**

与第26题删除重复元素类似，只不过把要删除的重复元素改成了给定的元素值。

所以依旧是从头开始遍历，用后边不等于 val 的元素值补充前面等于 val 的元素的位置，用变量记录前面序列的尾部，查找完以后开始从后往前删元素。

**代码：**

```C++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int i,k=0;
        for(i=0;i<nums.size();i++){
            if(nums[i]!=val){
                nums[k++]=nums[i];
            }
        }
        int n=nums.size()-k;
        for(int j=0;j<n;j++){
            nums.pop_back();
        }
        return nums.size();
    }
};
```

# 35. 搜索插入位置

**题目：**

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 O(log~2~n)的算法。

**思路：**

题目要求算法的时间复杂度为O(log~2~n)，所以选择折半查找，至于有没有更好的查找算法我现在还不清楚，书上的算法还没弄明白。

折半查找思路：

-   nums[mid] == target，说明找的就是这个元素，直接返回这个元素的位置就好

-   nums[mid] > target，说明要查找的元素在序列的左边部分,这时候就得分情况往下找
    1.  mid == 0 ,也就是已经找到了最左边了，第一个元素还是大于target，这时候就需要进行插入，但是题目没要求执行插入操作，所以返回target要插入的位置就好了，因为当前的mid已经指向第一个元素了，再往前也没了，所以应该把target插入到第一个位置，所以返回mid就行
    2.  nums[mid-1] < target，这种情况就是 nums[mid-1] < target < nums[mid]，数组中没有target这个元素，并且很明显可以看出target应该插入到mid和mid-1之间，但是在数组中，target应该插入到较大的位置，也就是返回mid
    3.  mid既不在边界也没办法判断target是否存在，就只能继续进行折半查找，查左边部分，high=mid-1
-   nums[mid] < target，说明target如果存在的话，应该在数组的右半部分，和上面一样，分情况
    1.  mid == len-1，也就是到了数组的尾部，再后面没东西了，那就直接插入到数组尾部后面，返回mid+1
    2.  nums[mid+1] > target，从nums[mid] < target < nums[mid+1]可以知道target不在数组中，而且如果要插入的话就插到mid和mid+1之间，所以返回mid+1
    3.  mid既不在边界也不能直接判断target是否存在，所以继续向右半部分进行折半查找。

**代码：**

```C++
class Solution {
public:
    int binary_search(vector<int>& nums,int low,int high,int target){
        while(low<=high){
            int mid=(low+high)/2;
            if(nums[mid]==target)
                return mid;
            else if(nums[mid]>target){
                if(mid==0)
                    return mid;
                else if(nums[mid-1]<target)
                    return mid;
                else
                    high=mid-1;
            }else if(nums[mid]<target){
                if(mid==nums.size()-1)
                    return mid+1;
                else if(nums[mid+1]>target)
                    return mid+1;
                else    
                    low=mid+1;
            }
        }
        return -1;
    }
    int searchInsert(vector<int>& nums, int target) {
        return binary_search(nums,0,nums.size()-1,target);
    }
};
```

# 58. 最后一个单词的长度

**题目：**

给你一个字符串 s，由若干单词组成，单词前后用一些空格字符隔开。返回字符串中 最后一个 单词的长度。

单词 是指仅由字母组成、不包含任何空格字符的最大子字符串。

**思路：**

从前往后扫描，记录单词长度

**代码：**

```C++
class Solution {
public:
    int lengthOfLastWord(string s) {
        int len;//记录最后一个单词长度
        int count=0;//记录当前单词的长度
        for(int i=0;i<s.size();i++){
            if(s[i]==' '&&count==0)
                continue;
            if(s[i]==' '&&count!=0){
                len=count;
                count=0;
                continue;
            }
            if(s[i]!=' '){
                count++;
                len=count;
            }
                
        }
        return len;
    }
};
```

# 66. 加一

**题目：**

给定一个由 整数 组成的 非空 数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储单个数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。

**示例：**

```
输入：digits = [1,2,3]
输出：[1,2,4]
解释：输入数组表示数字 123。
```

**思路：**

如果末尾元素不是9，即不需要进位，直接末尾元素+1。

如果末尾元素是9，把末尾元素修改成0，前面元素+1

-   如果前面的元素变成了10，则继续进位，直到某个元素不为10
-   如果前面没有元素了，即第一个元素变成10，需要对数组进行修改，将所有元素后移一位，第一位插入1

因为修改数组的操作要写两遍，所以我直接把它抽出来写成了一个单独的方法。

**代码：**

```C++
class Solution {
public:
    void kuochong(vector<int>& digits){//修改数组
        int temp1=digits[0];
        int temp2;
        digits[0]=1;
        for(int i=1;i<digits.size();i++){
            temp2=digits[i];
            digits[i]=temp1;
            temp1=temp2;
        }
        digits.push_back(temp1);
    }
    vector<int> plusOne(vector<int>& digits) {
        int len=digits.size();//数组长度
        int i=len-2;
        if(digits[len-1]!=9){//末尾元素不等于9
            digits[len-1]++;
        }else{
            digits[len-1]=0;
            if(len-2<0){
                kuochong(digits);
            }else
                digits[len-2]++;
        }
        while(i>=0&&digits[i]==10){
            digits[i]=0;
            if((i-1) < 0){
                kuochong(digits);
                break;
            }else
                digits[i-1]++;
            i--;
        }
        return digits;
    }
};
```

