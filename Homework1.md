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

## Linked List Cycle I

Given a linked list, return true if there is a cycle. If there is no cycle, return false.

```C++
    bool hasCycle(ListNode *head) {
        if (head && head->next){
            ListNode* buffer = head->next;
            head = head->next->next; //for the first move
            while (head && head->next) {
                if (buffer == head)
                    return true;
                buffer = buffer->next;
                head = head->next->next;
            }
        }
        return false;
    }
```
  

## Linked List Cycle II

Given a linked list, return the node where the cycle begins. If there is no cycle, return null.
Algorithm explanation: we are starting two nodes, where one of them running throw 1 elements, and second running two times faster(throw 2 elements). When they have meeted each other(cycle detected), we return meeting node. And from original head and meeting node have to be on the same distance from cycling node.
h - distance from original head to cycling node   
c - distance from cycling node to meeting node    
k = distance from meeting node to cycling node    
l - cycle length    

L - distance runned slow node      
L = h + c + l*n; // n and m some natural numbers  
2L = h + c + l*m;   
2*(h+c) + l*n = h + c + l*m;  
h + c = l*(m - n);       
if h + c should be div by l then if k = l - c     
h = k;    

```C++
    ListNode *detectCycle(ListNode *head) {
        ListNode* meetingNode;
        meetingNode = hasCycle(head);
        if (meetingNode) {
            while (meetingNode != head) {
                head = head->next;
                meetingNode = meetingNode->next;
            }
            return head;
        }
        return NULL;
    }
    
    ListNode* hasCycle(ListNode *head) {
        if (head && head->next){
            ListNode* buffer = head->next;
            head = head->next->next; //for the first move
            while (head && head->next) {
                if (buffer == head)
                    return buffer;
                buffer = buffer->next;
                head = head->next->next;
            }
        }
        return NULL;
    }
```
  
## Merge Two Sorted Lists

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

```C++

    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if (l1 && l2) {
            ListNode* l1Runner = l1; 
            ListNode* l2Runner = l2;
            ListNode* buffer = l1;
            ListNode* returningNode;
            if (l1Runner->val <= l2Runner->val) {
                 returningNode = l1Runner;
            }
            else {
                returningNode = l2Runner; //for second while cycle
                l2Runner = l1Runner;      //in algorithm l1Runner->val should be greater
                l1Runner = returningNode;
            }
            do {
                while (l1Runner->next != NULL && l1Runner->next->val <= l2Runner->val) {
                        l1Runner = l1Runner->next;
                }
                buffer = l1Runner->next;
                l1Runner->next = l2Runner;
                l1Runner = l2Runner;
                l2Runner = buffer;
            } while (buffer);
            
            return returningNode;
        }
        else {
            if (l1)
                return l1;
            else
                return l2;
        }

    }
```

## Remove Nth Node From End of List

Given a linked list, remove the n-th node from the end of list and return its head.

```C++
     ListNode* removeNthFromEnd(ListNode* head, int n) {
        if(head->next) {
            ListNode* buffer = head;
            ListNode* removing = head;
            int i = 0;
            while(i < n) {
                buffer = buffer->next;
                i++;
            } 
            if (!buffer) { //for the n == listCount
                buffer = head->next;
                delete(head);
                return buffer;
            }
            while (buffer->next) {
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
        if(head){
            ListNode* middle = head;
            while (head && head->next) {
                head = head->next->next;
                middle = middle->next;
            }
            return middle;
        }
        return NULL;
    }
```

## Delete Node in a Linked List

Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.

```C++
    void deleteNode(ListNode* node) {
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
      while (middle) { // because middle list shorter then cutted head list
        if (middle->val != head->val)
          return false;
        middle = middle->next;
        head = head->next;
      }
    }
    return true;
  }
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
     }
     return head;
 }
  ListNode* middleNode(ListNode* head) {
    if (head) {
      ListNode* middle = head;
      while (head->next && head->next->next) {//another middleNode function
        head = head->next->next;             //there head->next->next in 'if' for doing bufMiddle->next 
        middle = middle->next;               //in isPalindrom for odd and even list count
      }
      return middle;
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
        if (head && head->next){
            while (head && head->val == val) { //miss all first equals
                head = head->next;
            }
            if (!head)
                return NULL;
            ListNode* deleteBuf;
            ListNode* buffer = head;
            while (buffer->next) {  //buffer->val cannot be equal to val because we already have missed
                if (buffer->next->val == val) {
                    deleteBuf = buffer->next;
                    buffer->next = buffer->next->next;
                    delete(deleteBuf);
                    if (!buffer) //additional conditional for possible null pointer
                        break;
                }
                else{
                    buffer = buffer->next;
                }
            }
            return head;
        }
        if (head && head->val == val)
            return NULL;
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
        if (l1 && l2) {
            ListNode* l1Runner = l1; 
            ListNode* l2Runner = l2;
            ListNode* buffer = l1;
            ListNode* returningNode;
            if (l1Runner->val <= l2Runner->val) {
                 returningNode = l1Runner;
            }
            else {
                returningNode = l2Runner; //for second while cycle
                l2Runner = l1Runner;      //in algorithm l1Runner->val should be greater
                l1Runner = returningNode;
            }
            do {
                while (l1Runner->next != NULL && l1Runner->next->val <= l2Runner->val) {
                        l1Runner = l1Runner->next;
                }
                buffer = l1Runner->next;
                l1Runner->next = l2Runner;
                l1Runner = l2Runner;
                l2Runner = buffer;
            } while (buffer);
            
            return returningNode;
        }
        else {
            if (l1)
                return l1;
            else
                return l2;
        }

    }
    ListNode* sortList(ListNode* head) {
        if (head && head->next)
            return mergeTwoLists(sortList(head), sortList(middleNode(head)));
        return head;
        
    }
```

