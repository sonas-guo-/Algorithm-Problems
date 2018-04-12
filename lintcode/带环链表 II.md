# 题目链接
http://www.lintcode.com/zh-cn/problem/linked-list-cycle-ii/#

# 思路
先找出来是否有环。有环的话找出来相遇节点。然后根据相遇节点可以求出环的大小k。从开始出发走k步，然后另一个指针开始出发，那么两个指针最终相遇在入口点。
证明[单链表是否有环并如何找到环入口](https://www.cnblogs.com/cyttina/archive/2012/10/28/2743760.html)

# 代码
	class Solution {
	public:
	    /*
	     * @param head: The first node of linked list.
	     * @return: The node where the cycle begins. if there is no cycle, return null
	     */
	    ListNode * detectCycle(ListNode * head) {
	        // write your code here
	        ListNode *mnode=meetNode(head);
	        if (!mnode) return NULL;
	        ListNode *p1=mnode->next;
	        int cntCircle=1;
	        while (p1!=mnode)
	        {
	            p1=p1->next;
	            ++cntCircle;
	        }
	        p1=head;
	        for(int i=0;i<cntCircle;i++)
	            p1=p1->next;
	        ListNode *p2=head;
	        while(p1!=p2)
	        {
	            p1=p1->next;
	            p2=p2->next;
	        }
	        return p1;
	    }
	    ListNode * meetNode(ListNode *head)
	    {
	        if (!head) return NULL;
	        ListNode *slow=head->next;
	        if (!slow) return NULL;
	        ListNode *fast=slow->next;
	        while(slow&&fast)
	        {
	            if (slow==fast)
	                return slow;
	            slow=slow->next;
	            fast=fast->next;
	            if (fast)
	                fast=fast->next;
	        }
	        return NULL;
	    }
	};