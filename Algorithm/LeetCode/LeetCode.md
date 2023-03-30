- [LeetCode](#leetcode)
  - [网站](#网站)
  - [常见算法](#常见算法)
    - [冒泡排序](#冒泡排序)
    - [二分查找](#二分查找)
    - [无重复字符的最长子串](#无重复字符的最长子串)
    - [反转链表](#反转链表)
    - [两数之和](#两数之和)
    - [两数相加](#两数相加)
    - [使用两个队列实现栈](#使用两个队列实现栈)
    - [使用两个栈实现队列](#使用两个栈实现队列)

# LeetCode

## 网站
  - [CodeTop](https://codetop.cc/home)
  - [LeetCode](https://leetcode.cn/problemset/all/)

---
## 常见算法

### 冒泡排序
```java
   class Solution {
      public static void main(String[] args)  throws Exception {
        int[] arr =  { 5, 2, 7, 1, 9, 3 };
        for (int i = 0; i < arr.length; i++) {
            for (int j = 0; j < arr.length - i - 1; j++) {
                if (arr[j] > arr[j + 1]) {
                    int tmp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = tmp;
                }
            }
        }
        System.out.println(Arrays.toString(arr));
    }
}
```

### 二分查找
```java
   class Solution {
      public static void main(String[] args)  throws Exception {
         int[] arr = { 1, 2, 3, 4, 5, 6 };
        int target = 4;
        int result = binarySearch(arr, target);
        if (result == -1) {
            System.out.println("目标元素不存在！");
        } else {
            System.out.println("目标元素在数组中的索引为：" + result);
        }
    }

    public static int binarySearch(int[] arr, int target) {
        int left = 0;
        int right = arr.length - 1;
        while (left <= right) {
            int mid = left + (right - left)/2;
            if (arr[mid] == target) {
                return mid;
            } else if (arr[mid] > target) {
                right = mid - 1;
            } else if (arr[mid] < target) {
                left = mid + 1;
            }
        }
        return -1;
    } 
}
```

###  无重复字符的最长子串
> 给定一个字符串 s ，请你找出其中不含有重复字符的 最长子串 的长度。
```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
       int maxSubLength = 0;
       List<Character> subStr = new ArrayList<>();
       for (int i = 0; i< s.length(); i++) {
           Character curChar = s.charAt(i);
           int existIndex = subStr.indexOf(curChar);
           subStr.add(curChar);
           if (existIndex >= 0) {
               for (int j = existIndex; j >= 0; j-- ){
                   subStr.remove(j);
               }
           } else {
              if (maxSubLength < subStr.size()) {
                maxSubLength = subStr.size();
              }
           }        
       }
       return maxSubLength;  
    }
}
```

### 反转链表
- 给你单链表的头节点 head ，请你反转链表，并返回反转后的链表。
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode cur = head;
        ListNode pre = null;
        ListNode tmp = null;
        while (cur != null) {
            tmp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = tmp;
        }
        return pre;
     
    }
}
```

---
### 两数之和
- 时间复杂度O(N)，空间复杂度O(N)
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
      Map<Integer,Integer> buffer = new HashMap<>();
      for (int index = 0; index < nums.length; index++) {
          if (buffer.containsKey(target - nums[index])) {
              return new int[] {buffer.get(target - nums[index]), index};
          } else {
              buffer.put(nums[index], index);
          }
      }
      return new int[2];
    }
}
```
---
### 两数相加
- 时间复杂度：O(max(m,n),其中m和n分别为两个链表的长度。我们要遍历两个链表的全部位置，而处理每个位置只需要o(1)的时间
- 空间复杂度：O(1)。注意返回值不计入空间复杂度
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
       ListNode pre = new ListNode(0);
       ListNode cur = pre;
       int carry = 0;
       while (l1 != null || l2 != null) {
          int x = l1 != null ? l1.val : 0;
          int y = l2 != null ? l2.val : 0;
          int sum = x + y + carry;
          carry = sum / 10;
          cur.next = new ListNode(sum % 10);
          if (l1!=  null) {
              l1 = l1.next;
          }
          if (l2!= null) {
              l2 = l2.next;
          }
          cur = cur.next;
       } 
       if (carry > 0) {
          cur.next = new ListNode(carry); 
       }
       return pre.next;
    }
}
```

---
### 使用两个队列实现栈
```java
import java.util.*;

public class QueueStack {
    Queue<Integer> q1 = new LinkedList<Integer>();
    Queue<Integer> q2 = new LinkedList<Integer>();
    
    // Push element x onto stack.
    public void push(int x) {
        q1.offer(x);
        while (q1.size() > 1) {
            q2.offer(q1.poll());
        }
    }

    // Removes the element on top of the stack.
    public int pop() {
        int pop = q1.poll();
        if (q1.isEmpty()) {
            q1 = q2;
            q2 = new LinkedList<Integer>();
        }
        return pop;
    }

    // Get the top element.
    public int top() {
        return q1.peek();
    }

    // Return whether the stack is empty.
    public boolean empty() {
        return q1.isEmpty() && q2.isEmpty();
    }
}
```
---
### 使用两个栈实现队列
```java
import java.util.*;

public class StackQueue {
    Stack<Integer> s1 = new Stack<Integer>();
    Stack<Integer> s2 = new Stack<Integer>();
    
    // Push element x to the back of queue.
    public void push(int x) {
        s1.push(x);
    }

    // Removes the element from in front of queue.
    public int pop() {
        if (s2.isEmpty()) {
            while (!s1.isEmpty()) {
                s2.push(s1.pop());
            }
        }
        return s2.pop();
    }

    // Get the front element.
    public int peek() {
        if (s2.isEmpty()) {
            while (!s1.isEmpty()) {
                s2.push(s1.pop());
            }
        }
        return s2.peek();
    }

    // Return whether the queue is empty.
    public boolean empty() {
        return s1.isEmpty() && s2.isEmpty();
    }
}
```

---
- [回到首页](../../README.md)

