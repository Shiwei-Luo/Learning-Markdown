## 202. Happy Number(Easy)

平方数的和最终会产生循环，可将其视为环形链表，可以考虑使用快慢指针。

1. 快慢指针行进过程中出“1”时，目标数符合要求。
2. 快慢指针行进过程不出“1”且快慢指针所指数相同时（快慢指针重合），目标数不符合要求，依据此排除。

```java
class Solution {
    public boolean isHappy(int n) {
        int slow = n;
        int fast = n;
        do{                    
            slow = dgsqsum(slow);
            fast = dgsqsum(fast);
            fast = dgsqsum(fast);
            if (slow==1||fast==1){
                return true;
            }
        }while(slow!=fast);
        return false;        
    }
    public int dgsqsum(int n ){
        int sum = 0;
        while (n>0){
            int temp = n%10;
            sum += temp*temp;
            n /= 10;
        }
        return sum;
    }
}
```

### Tags: ==Fast & Slow pointers==,Hash Table

### reference：https://www.cnblogs.com/xiaochuan94/p/10048354.html

### 还有多种解法

***

## 141. Linked List Cycle(Easy)

快慢指针判断链表中是否含有循环

```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        do{
            if (fast==null||slow==null||fast.next==null){
                return false;
            }
            fast = fast.next;
            fast = fast.next;
            slow = slow.next;                 
        }while(fast!=slow);
        return true;        
    }
}
```

### Tags: ==Fast & Slow pointers==,Linked List

### reference:https://www.cnblogs.com/xiaochuan94/p/9981272.html



***

## 876. Middle of the Linked List(Easy)

快慢指针确定链表的中间位置

```java
class Solution {
    public ListNode middleNode(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while(fast.next!=null&&fast.next.next!=null){
            fast = fast.next;
            fast = fast.next;
            slow = slow.next;
        }
        if (fast.next==null){
            return slow;
        }else{
            return slow.next;
        }
    }
}
```

### Tags: ==Fast & Slow pointers==,Linked List

***

## 142. Linked List Cycle II(Medium)

快慢指针确定链表中循环入口，当快慢指针相遇时，设慢指针移动步数为n,则快指针移动步数为2n,此时n即为链表环的长度。（设链表起始点到循环入口长度为a，循环入口到相遇点长度为b，环的长度为c,则慢指针走过的距离n=a+b，快指针走过的距离为2n=a+c+b，两式相减得n=c，易得n=a+b=c，a=n-b=c-b，则使两指针分别同时从相遇点和链表起点开始遍历链表，最终相遇点即为链表循环部分入口。）==推荐画图分析==

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if(head==null||head.next==null){
            return null;
        }
        ListNode fast = head;
        ListNode slow = head;
        int count = 0;
        while(fast!=null && fast.next!=null){
            fast = fast.next.next;
            slow = slow.next;
            count += 1;
            if(fast==slow){
                break;
            }
        }
        if(fast==null || fast.next==null){
            return null;
        }
        if (slow==head){
            return slow;
        }
        fast = head;
        while(fast != slow){
            slow = slow.next;
            fast = fast.next;
        }
        return slow;
    }
}
```

### Tags: ==Fast & Slow pointers==,Linked List

### reference:https://www.bilibili.com/video/BV1kb411g7DZ

***

## 83.Remove Duplicates from Sorted List(Easy)

双指针删除链表中的重复元素

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if (head==null || head.next ==null){
            return head;
        }
        ListNode fast=head.next, slow=head;
        while(fast.next!=null){
            if (fast.val==slow.val){
                fast = fast.next;
                slow.next = fast;
            }else{
                fast = fast.next;
                slow = slow.next;
            }
        }
        if(fast.val == slow.val){
            slow.next = null;
        }
        return head;
    }
}
```

### Tags: ==Two pointers==,Linked List

***

## 19.Remove Nth Node From End of List(Medium)

让faster指针从起始点往后跑n步，然后让slower指针和faster指针一起跑，直到faster\==null（faster指针指出链表尾）时，slower指针恰好指向要删除的元素，通常删除元素需要添加一个prev指针指向被删除元素的前一个元素，在本题中将faster指针调整至faster.next\==null时，即faster指针指向链表末尾元素时，slower=slower.next.next删除元素，还有一种特殊情况，即要删除的元素时链表头元素时，此时faster已经跑到链表尾，此时直接返回head.next

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        if (head==null || head.next == null){
            return null;
        }
        ListNode pointer1 = head;
        //faster指针先跑
        for(int i=0;i<n;i++){
            pointer1 = pointer1.next;
        }
        ListNode pointer2 = head;
        //特殊情况处理
        if (pointer1 == null){
            return head.next;
        }
        while(pointer1.next!=null){
            pointer1 = pointer1.next;
            pointer2 = pointer2.next;
        }
        pointer2.next = pointer2.next.next;
        return head;
    }
}
```

### Tags: ==Two pointers==,Linked List

### reference：https://www.cnblogs.com/springfor/p/3862219.html

***

## 977.Squares of a Sorted Array(Easy)

数组中“0”附近的元素的平方值较小，故以正负数为分界点，采用双指针分别想数组头数组尾遍历。

```java
class Solution {
    public int[] sortedSquares(int[] A) {
        int i = 0;
        int len = A.length;
        while(i<len && A[i]<0){
            i++;
        }
        int j = i-1;
        int [] ans = new int[len];
        int k=0;
            
        //j for left,i for right
        while(j>=0 && i<len){
            if (A[j]*A[j]>=A[i]*A[i]){
                ans[k++] = A[i]*A[i];
                i++;
            }else{
                ans[k++] = A[j]*A[j];
                j--;
            }
        }
        
        while(j>=0){
            ans[k++] = A[j]*A[j];
            j--;
        }
        while(i<len){
            ans[k++] = A[i]*A[i];
            i++;    
        }
        return ans;
    }
}
```

### Tags: ==Two pointers==,Array

### reference：https://blog.csdn.net/yuanthecoder/article/details/95598863

***

## 713.Subarray Product Less Than K(Medium)

求所有连续子数组中，数组中所有元素乘积小于k的子数组个数

因为子数组中为连续数组，故应想到有双指针在子数组两端，若子数组乘积小于K，则右指针继续向右，若子数组乘积大于K，左指针向右，直至右指针遍历到数组末端。
注意：右指针向右移动一步，==增加的子数组数量n==\===子数组中元素个数a==\===1+2+...+n-(1+2+...+n-1)\n==\===right_ptr-left_ptr+1==。

```java
class Solution {
    public int numSubarrayProductLessThanK(int[] nums, int k) {
        int right = 0;
        int left =0;
        int mult = 1;
        int result = 0;
        while(right>=left && right <nums.length){
            if(right == left){
                if (nums[right]<k){
                    mult = mult*nums[right];
                    // System.out.println(mult);
                    result++;
                    right++;
                }else{
                    right++;
                    left++;
                    mult = 1;
                }
            }else if(mult*nums[right]<k){
                mult = mult * nums[right];
                // System.out.println(mult);
                result = result +right-left+1;
                right++;
            }else{
                mult = mult/nums[left];
                left++;
            }
        }
        // System.out.println(result);
        return result;
    }
}

        
//============================================================================
public int numSubarrayProductLessThanK(int[] nums, int k) {
    if (k <= 0) return 0;
    int result = 0, pre = 1, start = 0;
    for (int i = 0; i < nums.length; i++) {
        pre = pre * nums[i];
        if (pre < k) {
            result += i - start + 1;
        } else {
            for (int j = start; j <= i; j++) {
                pre = pre / nums[j];
                if (pre < k || (j == i && pre >= k)) {
                    start = j + 1;
                    break;
                }
            }
            result = result + i - start + 1;
        }
    }
    return result;
}    
```

###Tags: ==Two Pointer==,Array

### reference: https://blog.csdn.net/katrina95/article/details/79366608

***

