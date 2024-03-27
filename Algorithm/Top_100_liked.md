# LeetCode 热题 100



## 链表

###### [160. 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/)

- 解题思路

  ```text
  如果两个链表相交，那么必然有一个公共节点。
  我们可以用两个指针分别同时遍历两个链表，如果遍历到链表尾部还没有相交，那么将该链表的当前位置移动到另一条链表的头部，继续遍历，必然相交。
  ```

  

- 核心代码

    ```python
    # Definition for singly-linked list.
    # class ListNode:
    #     def __init__(self, x):
    #         self.val = x
    #         self.next = None
    
    class Solution:
        def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
            pA = headA  # 将 pA 初始化为链表 A 的头节点
            pB = headB  # 将 pB 初始化为链表 B 的头节点
            
            while pA != pB:  # 当 pA 和 pB 不相等时循环
                # 如果 pA 不为 None，pA 向后移动一位；如果 pA 为 None，将 pA 移动到链表 B 的头节点
                if pA is not None:
                    pA = pA.next
                else:
                    pA = headB
                
                # 如果 pB 不为 None，pB 向后移动一位；如果 pB 为 None，将 pB 移动到链表 A 的头节点
                if pB is not None:
                    pB = pB.next
                else:
                    pB = headA
            
            # 当 pA 和 pB 相等时（即都为 None，表示链表不相交；或者都指向相交节点），结束循环，返回 pA 或 pB
            return pA
    ```
    
    

###### [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/)

- 解题思路

  ```text
  第一种解法：
  	迭代双指针
  	假设链表为 1 -> 2 -> 3 -> null  我们想要把它改成 null <- 1 <- 2 <- 3 。
  在遍历链表时，将当前节点的 next 指针改为指向前一个节点。由于节点没有引用其前一个节点，因此必须事先存储其前一个节点。在更改引用之前，还需要存储后一个节点。最后返回新的头引用
  
  
  复杂度分析
  
  时间复杂度：O(n)
  空间复杂度：O(1)
  
  ```

  

- 核心代码

  ```python
   class Solution:
      def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
          # method 1 迭代双指针
          # cur, pre = head, None
          
          # while cur:
          #     tmp = cur.next # 暂存后继节点 cur.next
          #     cur.next = pre # 修改 next 引用指向
          #     pre = cur      # pre 暂存 cur
          #     cur = tmp      # cur 访问下一节点
          # return pre
  
          def recur(cur, pre):
              if not cur: return pre     # 终止条件
              res = recur(cur.next, cur) # 递归后继节点
              cur.next = pre             # 修改节点引用指向
              return res                 # 返回反转链表的头节点
          
          return recur(head, None)       # 调用递归并返回
   
  ```



###### [234. 回文链表](https://leetcode.cn/problems/palindrome-linked-list/)

- 解题思路

  ```text
  解法1：
  	遍历链表将值插入列表，然后比较列表与反转列表是否相等
  	
  解法2：
  	快慢指针
  	首先使用快慢指针同时遍历链表，当快指针走到尽头的时候，此时慢指针指向链表中间的节点，然后反转链表后半部分，最后比较前半部分和反转后的半部分的各个节点是否相同。
  	
  
  ```

  

- 核心代码

  ```python
  class Solution:
      def isPalindrome(self, head: Optional[ListNode]) -> bool:
          # mehtod 1 遍历插入列表，比较反转列表是否相等
  
          # res = []
          # cur = head
          # while cur:
          #     res.append(cur.val)
          #     cur = cur.next
          # return res == res[::-1]
  
  
          # method 2  快慢指针
          # 找到链表的中点
          fast, slow = head, head
          while fast.next and fast.next.next:
              slow = slow.next
              fast = fast.next.next
          
          # 反转链表的后半部分
          pre = None
          cur = slow.next
          while cur:
              tmp = cur.next
              cur.next = pre
              pre = cur
              cur = tmp
  
          # 比较前半部分和反转后的后半部分
          while pre:
              if head.val != pre.val:
                  return False
              head = head.next
              pre = pre.next
          return True
  ```

  

###### [141. 环形链表](https://leetcode.cn/problems/linked-list-cycle/)

- 解题思路

  ```text
  使用快慢指针，如果链表里面存在环，那么走的快的指针一定能追上走的慢的指针
  快指针每次移动两步，慢指针每次移动一步，如果快指针能追上慢指针，代表有环，否则会到链表末尾即结束
  
  复杂度分析
  时间复杂度：O(n)
  空间复杂度：O(1)
  ```

  

- 核心代码

  ```python
   class Solution:
      def hasCycle(self, head: Optional[ListNode]) -> bool:
          if not head or not head.next:
              return False
          # 快慢指针，如果指针相遇，一定有环
          slow, fast = head, head.next
          while fast != slow:
              if not fast or not fast.next:
                  return False
              fast = fast.next.next
              slow = slow.next
          return True
  ```



###### [142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)


- 解题思路

  ```text
  快慢指针
  	为了找到链表开始入环的第一个节点，我们可以使用快慢指针的方法。首先使用快慢指针找到环内的某个节点，然后使用两个指针分别从头节点和相遇点出发，每次都前进一步，当这两个指针相遇时，它们就在环的起始位置相遇。
  
  
  复杂度分析
  时间复杂度：O(n)
  空间复杂度：O(1)
  ```

  

- 核心代码

    ```python
 class Solution:
        def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
            slow, fast = head, head
            # 第一步：使用快慢指针找到环内的一个相遇点
            while True:
                if not fast or not fast.next:
                    return None  # 没有环
                slow = slow.next
                fast = fast.next.next
                if slow == fast:
                    break  # 找到环内的相遇点
            
            # 第二步：找到环的起始节点
            fast = head  # 将快指针重新指向头节点
            while fast != slow:  # 当快慢指针再次相遇时，相遇点即为环的起始节点
                slow = slow.next
                fast = fast.next
            
            return fast  # 返回环的起始节点
    ```



###### [21. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)

- 解题思路

  ```text
  要合并两个升序链表为一个新的升序链表，并返回新链表的头节点，可以使用一个简单的迭代方法。创建一个哨兵节点（也称为伪头节点）来简化边界情况的处理，然后使用两个指针分别遍历两个链表，每次比较两个指针指向的节点的值，将较小值的节点接到新链表的末尾，并移动该链表的指针。重复这个过程，直到一个链表被完全遍历。最后，将未完成遍历的链表接到新链表的末尾。
  
  复杂度分析
  时间复杂度：O(n)
  空间复杂度：O(1)
  ```

  

- 核心代码

  ```python
   class Solution:
      def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
  
          # 创建一个哨兵节点（伪头节点）
          head = ListNode()
          cur = head
          # 遍历两个链表，直到至少一个链表被完全遍历
          while list1 and list2:
              if list1.val <= list2.val:
                  cur.next = list1
                  list1 = list1.next
              else:
                  cur.next = list2
                  list2 = list2.next
              cur = cur.next
  
          # 将未遍历完的链表接到新链表的末尾
          if list1:
              cur.next = list1
          else:
              cur.next = list2
           # 返回合并后链表的头节点
          return head.next
  
  
  ```





-----



###### example

- 解题思路

  ```text
  
  
  复杂度分析
  时间复杂度：O(n)
  空间复杂度：O(1)
  ```

  

- 核心代码

  ```python
   
  ```



----