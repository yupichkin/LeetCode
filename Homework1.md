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
uses reverseList and middleNode functions
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
