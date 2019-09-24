# Lists (homework 1)

Definition for singly-linked list:

```C++ 
struct ListNode {
     int val;
     ListNode *next;
     ListNode(int x) : val(x), next(NULL) {}  
 };
 ```
## Reorder list
uses reverseList
```
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
