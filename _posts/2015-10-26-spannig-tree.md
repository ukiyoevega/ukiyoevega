---
layout: archive
title: 二叉树的实现
---

功能
1. 前、中、后序方法建立二叉树。
2. 实现二叉树前、中、后序遍历(包括递归与非递归算法)。
3. 实现二叉树的层次遍历。
4. 交换二叉树的左右子数。
5. 求二叉树的叶子总数，高度。

[cpp] view plain copy

	1	#include <iostream>  
	2	using namespace std;  
	3	#include <stack>  
	4	#include <queue>  
	5	struct BinTreeNode{  
	6	    int data;  
	7	    BinTreeNode *leftChild,*rightChild;  
	8	};  
	9	  
	10	void preOrder(BinTreeNode *t){  
	11	    if(t!=0){cout<<t->data<<" ";  
	12	        preOrder(t->leftChild);  
	13	        preOrder(t->rightChild);  
	14	    }  
	15	}  
	16	  
	17	void preOrderWithoutRecursion(BinTreeNode *t){  
	18	    /*：如果二叉树非空，则 
	19	     （1）将二叉树的根结点作为当前结点。 
	20	     （2）若当前结点非空，则先输出该结点，并将该结点进栈，再将其左孩子结点作为当前结点，重复步骤(2)，直到当前结点为空为止。 
	21	     （3）若栈非空，则将栈顶结点出栈，并将该结点的右孩子结点作为当前结点。 
	22	     （4）重复步骤(2), (3)，直到栈为空且当前结点为空为止。*/  
	23	    stack<BinTreeNode*> s;  
	24	    while(!s.empty()||t!=NULL){  
	25	        while(t!=NULL){cout<<t->data<<" ";s.push(t);t=t->leftChild;}  
	26	        if(!s.empty()){t=s.top();s.pop();t=t->rightChild;}  
	27	    }  
	28	}  
	29	  
	30	void inOrder(BinTreeNode *t){  
	31	    if(t!=NULL){  
	32	        inOrder(t->leftChild);  
	33	        cout<<t->data<<" ";  
	34	        inOrder(t->rightChild);  
	35	    }  
	36	}  
	37	  
	38	void inOrderWithoutRecursion(BinTreeNode *t){  
	39	    /*如果二叉树非空，则 
	40	     （1）将二叉树的根结点作为当前结点； 
	41	     （2）若当前结点非空，则该结点进栈并将其左孩子结点作为当前结点，重复步骤（2），直到当前结点为空为止； 
	42	     （3）若栈非空，则将栈顶结点出栈并作为当前结点，接着访问当前结点，再将当前结点的右孩子结点作为当前结点； 
	43	     （4）重复步骤（2）、（3），直到栈为空且当前结点为空为止。*/  
	44	    stack<BinTreeNode*> s;  
	45	    while(!s.empty()||t!=NULL){  
	46	        while(t!=NULL){s.push(t);t=t->leftChild;}  
	47	        if(!s.empty()){t=s.top();s.pop();cout<<t->data<<" ";t=t->rightChild;}  
	48	    }  
	49	}  
	50	void postOrder(BinTreeNode *t){  
	51	    if(t!=NULL){  
	52	        postOrder(t->leftChild);  
	53	        postOrder(t->rightChild);  
	54	        cout<<t->data<<" ";  
	55	    }  
	56	}  
	57	  
	58	  
	59	void postOrderWithoutRecursion(BinTreeNode *t){  
	60	    //用非递归算法进行后序遍历二叉树时，一个结点需要进栈，出栈各两次。为了区别同一个结点的两次进栈，需要设置一个标志flag, flag=0为第一次进栈，表示该结点出栈之后不能访问；flag=1为第二次进栈，表示该结点出栈之后可以访问  
	61	    stack<BinTreeNode*> s;  
	62	    stack<int> num;int flag;  
	63	    while(!s.empty()||t!=NULL){  
	64	        while(t!=NULL){s.push(t);num.push(0);t=t->leftChild;}  
	65	        if(!s.empty()){  
	66	            t=s.top();s.pop();flag=num.top();num.pop();  
	67	            if(flag==1){cout<<t->data<<" ";t=0;}  
	68	            else{s.push(t);num.push(1);t=t->rightChild;}  
	69	        }  
	70	    }  
	71	}  
	72	  
	73	void levelOrder(BinTreeNode *t){  
	74	    /*层次遍历的过程： 
	75	     (1)遍历进行之前先将二叉树的根结点存入队列中。 
	76	     (2)然后依次从队列中取出队头结点，每取出一个结点，都先访问该结点。 
	77	     (3)接着分别检查该结点是否存在左、右孩子，若存在则先后入列。 
	78	     (4)如此反复，直到队列为空为止。*/  
	79	    queue<BinTreeNode* > q; q.push(t);  
	80	    while (!q.empty()){  
	81	        t=q.front();  
	82	        q.pop();  
	83	        cout<<t->data<<" ";  
	84	        if(t->leftChild!=NULL) q.push(t->leftChild);  
	85	        if(t->rightChild!=NULL) q.push(t->rightChild);  
	86	    }  
	87	}  
	88	  
	89	  
	90	int size(BinTreeNode *t){  
	91	    if(t==0) return 0;  
	92	    else if(t->leftChild==0&&t->rightChild==0) return 1;  
	93	    else return size(t->rightChild)+size(t->leftChild);  
	94	}  
	95	  
	96	int height(BinTreeNode *t){  
	97	    if(t==0) return 0;  
	98	    else{  
	99	        int LeftHeight = height(t->leftChild);  
	100	        int RightHeight = height(t->rightChild);  
	101	        return LeftHeight>RightHeight?LeftHeight+1:RightHeight+1;  
	102	    }  
	103	}  
	104	  
	105	BinTreeNode * preOrderCreate(int value){  
	106	    BinTreeNode *current;  
	107	    int i;cin>>i;  
	108	    if(i==value) return NULL;  
	109	    else{  
	110	        current = new BinTreeNode;  
	111	        current->data=i;  
	112	        current->leftChild=preOrderCreate(value);  
	113	        current->rightChild=preOrderCreate(value);  
	114	        return current;  
	115	    }  
	116	}  
	117	  
	118	BinTreeNode * inOrderCreate(int value){  
	119	    BinTreeNode *current,*q;  
	120	    int i;cin>>i;  
	121	    if(i==value) return NULL;  
	122	    else{  
	123	        q=inOrderCreate(value);  
	124	        current = new BinTreeNode;  
	125	        current->data=i;  
	126	        current->leftChild=q;  
	127	        current->rightChild=inOrderCreate(value);  
	128	        return current;  
	129	    }  
	130	}  
	131	  
	132	BinTreeNode * postOrderCreate(int value){  
	133	    BinTreeNode *current,*l,*r;  
	134	    int i;cin>>i;  
	135	    if(i==value) return NULL;  
	136	    else{  
	137	        l=postOrderCreate(value);  
	138	        r=postOrderCreate(value);  
	139	        current = new BinTreeNode;  
	140	        current->data=i;  
	141	        current->leftChild=l;  
	142	        current->rightChild=r;  
	143	        return current;  
	144	    }  
	145	}  
	146	  
	147	void swop(BinTreeNode *t){  
	148	    BinTreeNode *temp;  
	149	    if(t->leftChild!=NULL&&t->rightChild!=NULL){  
	150	        temp=t->leftChild;t->leftChild=t->rightChild;t->rightChild=temp;  
	151	        swop(t->leftChild);swop(t->rightChild);  
	152	    }  
	153	}  
	154	  
	155	void nodePrint(BinTreeNode *t){  
	156	    if(t){  
	157	        cout<<t->data<<" ";  
	158	        nodePrint(t->leftChild);  
	159	        nodePrint(t->rightChild);  
	160	    }  
	161	}  
	162	  
	163	int main(){  
	164	    cout<<"**************输入数字实现对应功能**************"<<endl;  
	165	    cout<<"              1. 前序建立一棵二叉树"<<endl;  
	166	    cout<<"              2. 前序建立一棵二叉树"<<endl;  
	167	    cout<<"              3. 前序建立一棵二叉树"<<endl;  
	168	    cout<<"              4. 前序遍历递归算法"<<endl;  
	169	    cout<<"              5. 前序遍历非递归算法"<<endl;  
	170	    cout<<"              6. 中序遍历递归算法"<<endl;  
	171	    cout<<"              7. 中序遍历非递归算法"<<endl;  
	172	    cout<<"              8. 后序遍历递归算法"<<endl;  
	173	    cout<<"              9. 后序遍历非递归算法"<<endl;  
	174	    cout<<"              10. 层次遍历算法"<<endl;  
	175	    cout<<"              11. 求树高"<<endl;  
	176	    cout<<"              12. 求叶子总数"<<endl;  
	177	    cout<<"              13. 输出二叉树"<<endl;  
	178	    cout<<"              14. 交换左右子数"<<endl;  
	179	    cout<<"              15. 退出"<<endl;  
	180	    BinTreeNode *root;  
	181	    int i;  
	182	    while(cin>>i){  
	183	        switch (i) {  
	184	        case 1:root = preOrderCreate(-1);break;  
	185	        case 2:root = inOrderCreate(-1);break;  
	186	        case 3:root = postOrderCreate(-1);break;  
	187	        case 4: preOrder(root);break;  
	188	        case 5: preOrderWithoutRecursion(root);break;  
	189	        case 6: inOrder(root);break;  
	190	        case 7: inOrderWithoutRecursion(root);break;  
	191	        case 8: postOrder(root);break;  
	192	        case 9: postOrderWithoutRecursion(root);break;  
	193	        case 10:levelOrder(root);break;  
	194	        case 11:cout<<"Height: "<<height(root)<<endl;break;  
	195	        case 12:cout<<"Size: "<<size(root)<<endl;break;  
	196	        case 13:nodePrint(root);break;  
	197	        case 14:swop(root);break;  
	198	        case 15:exit(0);break;  
	199	  
	200	        }  
	201	    }  
	202	  
	203	}  

