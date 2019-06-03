最直观的解法是

## 新建一个List

通过在循环中用0来替代空节点的值来简化了代码的逻辑，但是每次申请了一个节点，比较耗时空

```c++
//20ms,10.5MB
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* p = l1;
        ListNode* q = l2;
        ListNode* head= new ListNode(0);
        ListNode* curr=head;
        int carry=0;
        while(p||q){
            int x= (p==NULL)? 0 : p->val;
            int y= (q==NULL)? 0 : q->val;
            ListNode* temp =  new ListNode((x+y+carry)%10);
            carry = (x+y+carry)/10;
            curr->next=temp;
            curr=curr->next;
            if(p!=NULL) p=p->next;
            if(q!=NULL) q=q->next;
        }
        if(carry>0){
            curr->next=new ListNode(1);
        }
        return head->next;
        
    }
};
```

接下来尝试

## 在一个list上直接合并另一个list

```c++
//20ms,9.8MB
class Solution {
 public:
	 ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
		 ListNode* l1_work = l1;
		 ListNode* l2_work = l2;
		 ListNode* dummyhead = new ListNode(0);
		 ListNode* curr = dummyhead;
		 int carry = 0, sum = 0;
		 while (l1_work || l2_work) {
			 int x = (l1_work == NULL) ? 0 : l1_work->val;
			 int y = (l2_work == NULL) ? 0 : l2_work->val;
			 sum = x + y + carry;
			 carry = (x + y + carry) / 10;
			
			 if (l1_work != NULL) {
				 l1_work->val = sum;
				 curr->next = l1_work;
				 curr = curr->next;
				 l1_work = l1_work->next;
				 if (l2_work != NULL) l2_work = l2_work->next;
			 }
			 else {
				 l2_work->val = sum;
				 curr->next = l2_work;
				 curr = curr->next;
				 l2_work = l2_work->next;
			 }
			 
		 }
		 if (carry > 0) {
			 curr->next = new ListNode(1);
		 }
		 return dummyhead->next;

	 }
 };
```

空间确实有一点点改进，不过时间上好像没有优化到.....
最后尝试找到一个比我快一些的做法，发现leetcode每次测得差距很大...，不过我倒是找到了一种稍微有些不同的做法，把所有的操作放到一个循环的做法

```c++
//20ms,
class Solution {
 public:
	 ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
		 ListNode* ans = nullptr;
		 ListNode* work;
		 int sum = 0, carry = 0;
		 while (l1 || l2 || carry!=0) {
			 sum = carry;
			 if (l1) {
				 sum += l1->val;
				 l1 = l1->next;
			 }
			 if (l2) {
				 sum += l2->val;
				 l2 = l2->next;
			 }
			 if (ans == nullptr) {
				 work = new ListNode(sum % 10);
				 ans = work;
			 }
			 else if(work!=nullptr){
				 work->next = new ListNode(sum % 10);
				 work = work->next;
			 }
			 carry = sum / 10;
		 }
		 return ans;
	 }
 };
```

