# Linked list 

Linked list struct in C++

```C++ 
struct ListNode {
     int val;
     ListNode *next;
     ListNode(int x) : val(x), next(NULL) {}  
 };
 ```
 
## Reorder list
Given a singly linked list L: L0→L1→…→Ln-1→Ln,
reorder it to: L0→Ln→L1→Ln-1→L2→Ln-2→…

My algorithm uses reverseList and middleNode functions
```C++
ListNode* middleNode(ListNode* head) {
   ListNode* bufMiddle = head;
   ListNode* buf = head;
   if (head) {
     while (head->next && head->next->next) {
       head = head->next->next;
       bufMiddle = bufMiddle->next;
     }
     return bufMiddle;
   }
   return NULL;
 }
 ListNode* reverseList(ListNode* head) {
   if (head) {
     ListNode* after = head->next;
     ListNode* buf = head->next;
     head->next = NULL;

     while (after) {
       buf = after->next;
       after->next = head;
       head = after;
       after = buf;
     }
     return head;
   }
   return head;

 }
 ListNode* splitTwoLists(ListNode* l1, ListNode* l2) {
   ListNode* buffer1 = l1;
   ListNode* buffer2 = l2;
   ListNode* buffer3;
   ListNode* buffer4;
   if (buffer1 && buffer2) {
     while (buffer1->next) {
       buffer3 = buffer1->next;
       buffer4 = buffer2->next;
       buffer1->next = buffer2;
       buffer2->next = buffer3;
       buffer1 = buffer3;
       buffer2 = buffer4;
     }
     if (buffer2 && buffer2->next == NULL) {
       buffer1->next = buffer2;
     }
     return l1;
   }
   else {
     if (buffer1)
       return buffer1;
     else
       return buffer2;
   }

 }
 void reorderList(ListNode* head) {
   if (head && head->next) {
       ListNode* middle = middleNode(head);
       ListNode* buffer = middle;
       middle = middle->next;
       buffer->next = NULL;
       middle = reverseList(middle);
       splitTwoLists(head, middle);
   }
 }
```

## Linked List Cycle II

Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

```C++
ListNode *detectCycle(ListNode *head) {
    int length;
    int i;
    if(hasCycle(head, &length)) {
        ListNode* buffer = head;
        ListNode* buffer2 = head->next;
        do {
            i = 0;
            do {
                if (buffer == buffer2)
                    return buffer;
                buffer2 = buffer2->next;
                i++;
            } while (i <= length);
            buffer = buffer->next;
        }   while (buffer);
    }
    return NULL;
        
}

bool hasCycle(ListNode *head, int* length) {
    if (head){
        ListNode* buffer = head->next;
        int i = 0;
        if (head->next) {
            head = head->next->next; //for the first move

            while (head && head->next) {
                if (buffer == head) {
                    do {
                        buffer = buffer->next;
                        i++;
                    } while (buffer != head);
                    
                    *length = i;
                    return true;
                }
                buffer = buffer->next;
                head = head->next->next;
            }
        }
    }
    return false;
}
```
  
## Merge Two Sorted Lists

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

```C++

ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* buffer1 = l1;
        ListNode* buffer2 = l2;
        ListNode* buffer = l1;
        ListNode* returningNode;
        if (buffer1 && buffer2) {
            if (buffer1->val <= buffer2->val) {
                 returningNode = buffer1;
            }
            else {
                returningNode = buffer2; //for second while cycle
                buffer2 = buffer1;
                buffer1 = returningNode;
            }
            do {
                while (buffer1->next != NULL && buffer1->next->val <= buffer2->val) {
                        buffer1 = buffer1->next;
                }
                buffer = buffer1->next;
                buffer1->next = buffer2;
                buffer1 = buffer2;
                buffer2 = buffer;
            } while (buffer);

            return returningNode;
        }
        else {
            if (buffer1)
                return buffer1;
            else
                return buffer2;
        }

  }
```

## Remove Nth Node From End of List

Given a linked list, remove the n-th node from the end of list and return its head.

```C++
ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* buffer = head;
        ListNode* removing = head;
        int i = 0;
        if(head->next) {
            while(i < n) {
                buffer = buffer->next;
                i++;
            } 
            if (buffer == NULL) { //for the n == listCount
                buffer = head->next;
                delete(head);
                return buffer;
            }
            while (buffer->next && removing->next->next) {
                removing = removing->next;
                buffer = buffer->next;
            } 

            buffer = removing->next->next;
            delete(removing->next);
            removing->next = buffer;
            return head;
        }
        else {
            return NULL;
        }
        
    }
```
    
## Middle of the Linked List

Given a non-empty, singly linked list with head node head, return a middle node of linked list.
If there are two middle nodes, return the second middle node.

```C++
ListNode* middleNode(ListNode* head) {
        ListNode* bufMiddle = head;
        ListNode* buf = head;
        if(head){
            while (head && head->next) {
                head = head->next->next;
                bufMiddle = bufMiddle->next;
            }
            return bufMiddle;
        }
        return NULL;
    }
```

## Delete Node in a Linked List

Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.

```C++
void deleteNode(ListNode* node) {
        int buf;
        ListNode* buffer = node->next;
        node->val = node->next->val;
        node->next = node->next->next;
        delete(buffer);
        
    }
```

## Palindrome Linked List

Given a singly linked list, determine if it is a palindrome.

```C++
bool isPalindrome(ListNode* head) {
    if (head && head->next) {
      ListNode* buffer = middleNode(head);
      ListNode* middle = buffer->next;
      buffer->next = NULL;
      middle = reverseList(middle);
      while (middle && head) {
        if (middle->val != head->val)
          return false;
        middle = middle->next;
        head = head->next;
      }
    }
    return true;
  }
  ListNode* reverseList(ListNode* head) {
    if (head) {
      ListNode* after = head->next;
      ListNode* buf = head->next;
      head->next = NULL;

      while (after) {
        buf = after->next;
        after->next = head;
        head = after;
        after = buf;
      }
      return head;
    }
    return head;

  }
  ListNode* middleNode(ListNode* head) {
    ListNode* bufMiddle = head;
    ListNode* buf = head;
    if (head) {
      while (head->next && head->next->next) {
        head = head->next->next;
        bufMiddle = bufMiddle->next;
      }
      return bufMiddle;
    }
    return NULL;
  }
```

## Reverse Linked List

Reverse a singly linked list.

itteratively:
```C++
ListNode* reverseList(ListNode* head) {
        if (head){
            ListNode* after = head->next;
            ListNode* buf = head->next;
            head->next = NULL;

            while(after) {
                buf = after->next;
                after->next = head;
                head = after;
                after = buf;
            }
            return head;
        }
        return head;
    
    }
```
recursively: 
```C++
    ListNode* reverseList(ListNode* head) {
        if (head && head->next) {
          ListNode* buffer = head->next;
          head->next = NULL;
          return reverseNode(buffer, head);
        }
        return head;
    }
    ListNode* reverseNode(ListNode* reversiveNode, ListNode* prev) {
        if (reversiveNode->next) {
          ListNode* buffer = reversiveNode->next;
          reversiveNode->next = prev;
          return reverseNode(buffer, reversiveNode);
        }
      reversiveNode->next = prev;
      return reversiveNode;
    }
```
## Remove Linked List Elements

Remove all elements from a linked list of integers that have value val.

```C++
ListNode* removeElements(ListNode* head, int val) {
        ListNode* buffer = head;
        ListNode* prevBuf = head;
        if (head && head->next){
            while (prevBuf && prevBuf->val == val) { //miss all first equals
                prevBuf = prevBuf->next;

            }
            head = prevBuf;
            if (!prevBuf)
                return NULL;
            buffer = prevBuf->next;
            while (buffer && buffer->next) {
                if (buffer->val == val) {
                    prevBuf->next = buffer->next;
                    buffer = buffer->next;
                }
                else{
                    prevBuf = buffer;
                    buffer = buffer->next;
                }
            }
            if (buffer && buffer->val == val) {
                prevBuf->next = NULL;
            }
            return head;
        }
        if (head && head->val == val)
            return NULL;
        else 
            return head;
    }
```

## Intersection of Two Linked Lists

Given a linked list, remove the n-th node from the end of list and return its head.

```C++
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        int i = getLength(headA) - getLength(headB);
        while (i > 0) {
            headA = headA->next;
            i--;
        }
        while (i < 0) {
            headB = headB->next;
            i++;
        }
        while (headA) {
            if (headA == headB)
                return headA;
            headA = headA->next;
            headB = headB->next;
        }
        return NULL;
    }
    
    int getLength(ListNode *list) {
        int i = 0;
        while (list){
            list = list->next;
            i++;
        }
        return i;
    }
```

##  Sort List

Sort a linked list in O(n log n) time using constant space complexity.
Algorithm uses Merge sort

```C++
ListNode* middleNode(ListNode* head) {
        if(head){
            ListNode* middle = head;
            while (head->next && head->next->next) {
                head = head->next->next;
                middle = middle->next;
            }
            head = middle;
            middle = middle->next;
            head->next = NULL;
            return middle;
        }
        return NULL;
    }
    
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        
        ListNode* buffer1 = l1;
        ListNode* buffer2 = l2;
        ListNode* buffer = l1;
        ListNode* returningNode;
        if (buffer1 && buffer2) {
            if (buffer1->val <= buffer2->val) {
                 returningNode = buffer1;
            }
            else {
                returningNode = buffer2; //for second while cycle
                buffer2 = buffer1;
                buffer1 = returningNode;
            }
            do {
                while (buffer1->next != NULL && buffer1->next->val <= buffer2->val) {
                        buffer1 = buffer1->next;
                }
                buffer = buffer1->next;
                buffer1->next = buffer2;
                buffer1 = buffer2;
                buffer2 = buffer;
            } while (buffer);

            return returningNode;
        }
        else {
            if (buffer1)
                return buffer1;
            else
                return buffer2;
        }

  }
    ListNode* sortList(ListNode* head) {
        if (head && head->next)
            return mergeTwoLists(sortList(head), sortList(middleNode(head)));
        return head;
        
    }
```

