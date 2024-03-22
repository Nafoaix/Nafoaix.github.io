---
title: 数据结构——线性表
tags:
  - 数据结构
  - 线性表
  - 学习笔记
categories:
  - 数据结构
  - 线性表
toc: true
cover: 'https://cdn.nafx.top/post_cover/20230401171811.png'
top_img: 'https://cdn.nafx.top/post_cover/20230401171811.png'
abbrlink: 335a1544
date: 2021-05-01 16:24:41
---

## 线性表

线性表(List):零个或多个数据元素的有限序列。

### 顺序存储线性表

- 优点: 可以快速存取表中任一位置的元素，无需为变种元素之间的逻辑关系增加存储空间。
- 缺点: 插入删除需要移动大量元素，长度变化较大时难以确定存储空间的容量(分配大了浪费，小了溢出)，造成存储空间碎片。
查找性能O(1),插入删除O(n)。
若线性表需要频繁查找，很少进行插入和删除操作时可以采用顺序存储结构，一个简单的顺序存储线性表实现例子:

``` c
#include "stdio.h"

/*
************************************************************************
*                                宏定义
************************************************************************
*/

#define OK 1
#define ERR 0
#define TRUE 1
#define FALSE 0
#define MAXSIZE 11

typedef int Status;
typedef int ElemType;

/*
************************************************************************
*                                结构体定义
************************************************************************
*/

typedef struct 
{
	ElemType data[MAXSIZE];
	int len;
}sqList;

/*
************************************************************************
*                                函数
************************************************************************
*/

/* 初始化顺序线性表 */
Status ListInit(sqList *L)
{
	// ElemType defaultList[11] = {1,8,0,2,5,3,3,1,4,0,8};
	// for (int i = 0; i <L->len ; i++)
	// L->data[i] = defaultList[i];
	// L->len = 11;

	L->len = 0;

	return OK;
}

/* 初始条件：顺序线性表L已存在。操作结果：若L为空表，则返回TRUE，否则返回FALSE */
Status ListEmpty(sqList L)
{ 
	if(L.len == 0)
		return TRUE;
	else
		return FALSE;
}

/* 初始条件：顺序线性表L已存在。操作结果：返回L中数据元素个数 */
int ListLength(sqList L)
{
	return L.len;
}

/* 初始条件：顺序线性表L已存在，1≤i≤ListLength(L) */
/* 操作结果：用e返回L中第i个数据元素的值,注意i是指位置，第1个位置的数组是从0开始 */	
Status GetElem(sqList L,int i ,ElemType *e)
{
	if(L.len == 0 || i<1 || i>L.len )
		return ERR;
	*e = L.data[i-1];

	return OK;
}

/* 初始条件：顺序线性表L已存在,1≤i≤ListLength(L)， */
/* 操作结果：在L中第i个位置之前插入新的数据元素e，L的长度加1 */
Status ListIns(sqList *L ,int i,ElemType e)
{
	int k;
	if (L->len == MAXSIZE )	/* 顺序线性表已经满 */
		return ERR;
	if (i<1 || i>L->len+1)	/* 当i比第一位置小或者比最后一位置后一位置还要大时 */
		return ERR;
	if ( i<= L->len)		/* 若插入数据位置不在表尾 */
	{
		for (k = L->len-1;k>=i-1;k--)	/* 将要插入位置之后的数据元素向后移动一位 */
			L->data[k+1] = L->data[k];
	}
	L->data[i-1] = e;					 /* 将新元素插入 */
	L->len++;
	return OK;

}

/* 初始条件：顺序线性表L已存在，1≤i≤ListLength(L) */
/* 操作结果：删除L的第i个数据元素，并用e返回其值，L的长度减1 */
Status ListDel(sqList *L,int i,ElemType *e)
{
	int k;
	if (L->len == 0)		/* 线性表为空 */
		return ERR;
	if (i<1 || i>L->len)	/* 删除位置不正确 */
		return ERR;
	*e = L->data[i-1];
	if(i<L->len)			/* 如果删除不是最后位置 */
	{
		for (k=i;k<L->len;k++)
			L->data[k-1] = L->data[k];	/* 将删除位置后继元素前移 */
	}
	L->len--;
	return OK;
}

/* 初始条件：顺序线性表L已存在 */
/* 操作结果：依次对L的每个数据元素输出 */
Status PrintList(sqList L)
{
	if (L.len == 0)
	{
		printf("当前表为空!");
		return ERR;
	}
	for (int i = 0 ; i<L.len-1 ; i++)
	{
		printf("%d,",L.data[i]);
	}
	printf("%d \n",L.data[L.len-1]);
	return OK;
}

/* 初始条件：顺序线性表L已存在 */
/* 操作结果：返回L中第1个与e满足关系的数据元素的位序。 */
/* 若这样的数据元素不存在，则返回值为0 */
int LocateElem(sqList L,ElemType e)
{
    int i;
    if (L.len==0)
            return 0;
    for(i=0;i<L.len;i++)
    {
            if (L.data[i]==e)
                    break;
    }
    if(i>=L.len)
            return 0;

    return i+1;
}

/*将所有的在线性表Lb中但不在La中的数据元素插入到La中*/
void unionL(sqList *La,sqList Lb)
{
	int La_len,Lb_len,i;
	ElemType e;                        /*声明与La和Lb相同的数据元素e*/
	La_len=ListLength(*La);            /*求线性表的长度 */
	Lb_len=ListLength(Lb);
	for (i=1;i<=Lb_len;i++)
	{
		GetElem(Lb,i,&e);              /*取Lb中第i个数据元素赋给e*/
		if (!LocateElem(*La,e))        /*La中不存在和e相同数据元素*/
			ListInsert(La,++La_len,e); /*插入*/
	}
}

int main(int argc, char *argv[])
{
	int opt;
	int exit = OK;
	sqList thislist;
	ListInit(&thislist);

	while(exit)
	{
		printf("\n===================================================================================================== \n");
		printf(">>>请输入操作标号: 1.初始化顺序线性表; 2.在第i个位置插入新元素e; 3.删除第i位元素; 4.查看第i位元素; 5.查看当前表; 0.退出;\n");
		scanf("%d",&opt);
		switch (opt)
		{
			case 1:
			{
				ListInit(&thislist);
				printf("初始化完成，目前线性表元素为:");
				PrintList(thislist);
			}	
				break;	 
			case 2:
				{
					int i,e;
					printf("请依次输入i和e的值:\n");
					scanf("%d %d",&i,&e);
					if (ListIns(&thislist,i,e))
					{
						printf("插入成功，目前线性表元素为:");
						PrintList(thislist);
					}else
					{
						printf("插入失败!\n");
					}
				}
				break;
			case 3:
				{
					int i,e;
					printf("请输入i的值:\n");
					scanf("%d",&i);
					if (ListDel(&thislist,i,&e))
					{
						printf("删除成功，目前线性表元素为:");
						PrintList(thislist);
					}else
					{
						printf("删除失败!\n");
					}
		
				}
				break;
			case 4:
				{
					int i,e;
					printf("请输入i的值:\n");
					scanf("%d",&i);
					if (GetElem(thislist,i,&e))
					{
						printf("取值成功，目前线性表第%d位元素为:%d\n",i,e);
					}else
					{
						printf("取值失败!\n");
					}
				}
				break;
			case 5:
			{
				printf("目前线性表元素为:");
				PrintList(thislist);
			}	
				break;	
			case 0:
				exit = ERR;
				printf("Goodbye~\n");
				break;
			default:
				printf("输入错误请按标号重新输入！\n");
		}
	}
	return 0;
}

```

### 链式存储线性表

#### 单链表

单链表不需要提前分配存储空间，元素个数也不受限制。
查找性能O(n)，插入性能O(1).
如需要频繁插入删除时或线性表中元素个数变化较大或根本不知道有多大时可以采用链式存储线性表结构。一个简单的单链表实现例子:

``` c

/*
************************************************************************
*                                头文件
************************************************************************
*/
#include "stdio.h"
#include "stdlib.h"

/*
************************************************************************
*                                宏定义
************************************************************************
*/
#define OK 1
#define ERR 0
#define TRUE 1
#define FALSE 0

typedef int Elemtype;
typedef int Status;

/*
************************************************************************
*                                结构体定义
************************************************************************
*/
typedef struct Node
{
	Elemtype data;
	struct Node *next;	
}Node, *Linklist;

/*
************************************************************************
*                                函数
************************************************************************
*/
/* 初始化链式线性表 */
Status InitList (Linklist *Head)
{
	*Head = (Linklist)malloc(sizeof(Node));	/* 产生头结点,并使Head指向此头结点 */

	if(!(*Head)) 			/* 存储分配失败 */
        return ERR;
	
	(*Head)->next = NULL;	/* 指针域为空 */
	(*Head)->data = 0;

	return OK;
}

/* 初始条件：链式线性表L已存在。操作结果：若L为空表，则返回TRUE，否则返回FALSE */
Status ListEmpty(Linklist L)
{ 
    if(L->next)
            return FALSE;
    else
            return TRUE;
}

/* 初始条件：链式线性表L已存在。操作结果：将L重置为空表 */
Status CleanList (Linklist *L)
{
	Linklist p = (*L)->next;	/*  p指向第一个结点 */
	while (p)
	{
		Linklist tmp = p->next;
		free(p);
		p = tmp; 
	}
	(*L)->next = NULL;			/* 头结点指针域为空 */
	(*L)->data = 0;
	return OK;
}

/* 初始条件：链式线性表L已存在。操作结果：返回L中数据元素个数 */
int ListLength(Linklist L)
{
    // int i=0;
    // Linklist p=L->next; /* p指向第一个结点 */
    // while(p)                        
    // {
    //     i++;
    //     p=p->next;
    // }
    // return i;
	return L->data;
}

/* 初始条件：链式线性表L已存在，1≤i≤ListLength(L) */
/* 操作结果：用e返回L中第i个数据元素的值 */
Status GetElem (Linklist L,int i,Elemtype *e)
{
	int j = 1;
	Linklist p = L->next;	/* 让p指向链表L的第一个结点 */
	while(p && j < i) 		/* p不为空或者计数器j还没有等于i时，循环继续 */
    {
        p = p->next;
        j++;
    }
	if (!p || j>i )  		/*  第i个元素不存在 */
		return ERR;
	*e = p->data;			/*  取第i个元素的数据 */

	return OK;
}

/* 初始条件：链式线性表L已存在 */
/* 操作结果：返回L中第1个与e满足关系的数据元素的位序。 */
/* 若这样的数据元素不存在，则返回值为0 */
int LocateElem(Linklist L,Elemtype e)
{
    int i=0;
    Linklist p=L->next;
    while(p)
    {
        i++;
        if(p->data==e) /* 找到这样的数据元素 */
                return i;
        p=p->next;
    }

    return 0;
}

/* 初始条件：链式线性表L已存在,1≤i≤ListLength(L)， */
/* 操作结果：在L中第i个位置之前插入新的数据元素e，L的长度加1 */
Status InsNode (Linklist *L,int i ,Elemtype e)
{
	int j = 1;
	Linklist p = *L; 	
	while (p && j < i) 							/* 寻找第i个结点 */			
	{
		p = p->next;
		++j;
	}
	if (!p || j>i )  							/* 第i个元素不存在 */
		return ERR;
	Linklist new_node = (Linklist)malloc(sizeof(Node)); 	/*  生成新结点(C语言标准函数) */
	new_node->data = e;
	new_node->next = p->next;
	p->next = new_node;
	++(*L)->data;

	return OK;
}

/* 初始条件：链式线性表L已存在，1≤i≤ListLength(L) */
/* 操作结果：删除L的第i个数据元素，并用e返回其值，L的长度减1 */
Status DelNode (Linklist *L,int i ,Elemtype *e) 
{
	int j = 1;
	Linklist prev = *L;
	while (prev->next && j < i)	 // 找到第i个节点的前一个节点prev
	{
		prev = prev->next;
		++j;
	}
	if ( !(prev->next) || j > i ) //第i个结点不存在
		return ERR;
	Linklist p = prev->next; 	// 找到第i个节点p
	prev->next = p->next;       // 从链表中删除p节点
	*e = p->data;
	free(p);
	--(*L)->data;

	return OK;
}

/* 初始条件：链式线性表L已存在 */
/* 操作结果：依次对L的每个数据元素输出 */
void PrintNode (Linklist L)
{
	Linklist p = L->next;

	while (p)
	{
		printf("%d ",p->data);
		p = p->next;
	}
	printf("\n");
	
}

/*  头插法插入元素e */
Linklist ListInsHead (Linklist *L , Elemtype e)
{
	Linklist new_node = (Linklist)malloc(sizeof(Node));
	new_node->data = e;
	if ((*L)->next == NULL)
	{
		(*L)->next = new_node;
		new_node->next  = NULL;
	}else{
	new_node->next = (*L)->next;
	(*L)->next = new_node;
	}
	++(*L)->data;

	return *L;
}

/*  尾插法插入元素e */
Linklist ListInsTail (Linklist *L , Elemtype e)
{
	Linklist new_node = (Linklist)malloc(sizeof(Node));
	new_node->data = e;
	new_node->next = NULL;
	Linklist p = *L;
	while (p->next != NULL)
	{
		p = p->next;   // 找到链表的尾节点
	}
	p->next = new_node;
	++(*L)->data;

	return *L;
}





int main(int argc, char *argv[])
{
	int opt;
	int exit = OK;
	Linklist thislist;
	InitList(&thislist);

	while(exit)
	{
		printf("\n======================================================================================================================== \n");
		printf(">>>请输入操作标号: 1.头插法增加结点; 2.尾插法增加结点; 3.在第i个位置之前插入新结点; 4.删除第i个位置的结点; 5.查看第i个结点的值; 6.清空当前链表; 7.查看当前链表; 0.退出;\n");
		scanf("%d",&opt);
		switch (opt)
		{
			case 1:
			{
				Elemtype val;
				printf("请输入新增结点的值并按回车:\n");
				scanf("%d",&val);
				ListInsHead(&thislist,val);
				printf("插入完成，目前链表共%d有个结点。每个结点值分别为:",thislist->data);
				PrintNode(thislist);
			}	
				break;	 
			case 2:
				{
					Elemtype val;
					printf("请输入新增结点的值并按回车:\n");
					scanf("%d",&val);
					ListInsTail(&thislist,val);
					printf("插入完成，目前链表共%d有个结点。每个结点值分别为:",thislist->data);
					PrintNode(thislist);
				}
				break;
			case 3:
				{
					int i,e;
					printf("请输入i的值:\n");
					scanf("%d",&i);
					printf("请输入新增结点的值并按回车:\n");
					scanf("%d",&e);
					if (InsNode(&thislist,i,e))
					{
						printf("插入完成，目前链表共%d有个结点。每个结点值分别为:",thislist->data);
						PrintNode(thislist);
					}else
					{
						printf("插入失败!\n");
					}
				}
				break;
			case 4:
				{
					int i,e;
					printf("请输入i的值:\n");
					scanf("%d",&i);
					if (DelNode(&thislist,i,&e))
					{
						printf("删除完成，删除结点的值为: %d \n目前链表共有%d个结点。每个结点值分别为:",e,thislist->data);
						PrintNode(thislist);
					}else
					{
						printf("删除失败!\n");
					}
				}
				break;
			case 5:
				{
					int i,e;
					printf("请输入i的值:\n");
					scanf("%d",&i);
					if (GetElem(thislist,i,&e))
					{
						printf("该结点的值为: %d \n",e);
					}else
					{
						printf("查看失败!\n");
					}
				}
				break;
			case 6:
			{
				CleanList(&thislist);
				printf("清空完成! \n");
				
			}	
			case 7:
			{
				if (thislist->next == NULL)
				{
					printf("当前链表为空! 请先增加些结点。\n\n");
				}else{
					printf("当前链表共%d有个结点。每个结点值分别为:",thislist->data);
					PrintNode(thislist);
				}
			}	
				break;	
			case 0:
				exit = ERR;
				printf("Goodbye~\n");
				break;
			default:
				printf("输入错误请按标号重新输入！\n");
		}
	}

	return 0;
}

```

#### 静态链表

静态链表在插入和删除操作时只需要移动游标不需要移动元素从而改进了顺序存储结构插入和删除元素需要移动大量元素的缺点。但是没有解决连续存储分配带来的表长难以确定问题，页数去了顺序存储随机存取的特性。下面是一个简单的静态链表实现例子:

``` c

/*
************************************************************************
*                                头文件
************************************************************************
*/
#include "stdio.h"

/*
************************************************************************
*                                宏定义
************************************************************************
*/
#define OK 1
#define ERR 0
#define TRUE 1
#define FALSE 0
#define MAXSIZE 10

typedef int Elemtype;
typedef int Status;

/*
************************************************************************
*                                结构体定义
************************************************************************
*/

/* 这是一个静态链表的数据结构定义，其中Component表示链表的每个结点，包括数据元素data和游标cur。
staticLinkList是一个数组，用于存储静态链表的所有结点，其中MAXSIZE表示静态链表的最大长度。*/
typedef struct
{
	Elemtype data;
	int cur;		// 游标(cursor),为0时表示无指向
} Component,staticLinkList[MAXSIZE];

/*
************************************************************************
*                                函数
************************************************************************
*/
/* 数组第一个元素space[0]的cur用来存放备用链表(空闲空间)第一个结点的下标。
   数组的最后一个元素[MAXSIZE-1]用来存放第一个插入元素的下标，相当于头结点，整个链表为空时则为0*/
Status InitStaticList (staticLinkList space)
{
	for (int i = 0 ; i<MAXSIZE-1; i++)
		space[i].cur = i + 1;
	space[MAXSIZE-1].cur = 0;	/* 目前静态链表为空，最后一个元素的cur为0 */
	return OK;
}

/* 若备用空间链表非空（还有存储空间），则返回分配的节点下标，否则返回0*/
int Malloc_SLL ( staticLinkList space)
{
	int i = space[0].cur;		/* 当前数组第一个元素的cur存的值 */
	                        	/* 就是要返回的第一个备用空闲的下标 */
	if (space[0].cur)
	{
		space[0].cur = space[i].cur; //将下一个空闲结点的下标放在space[0]的cur中以便下次使用
	}
	return i;

}

/* 将下标为k的结点回收到备用链表 */
void Free_SSL (staticLinkList space , int k)
{
	space[k].cur = space[0].cur;	//将备用链表(空闲空间)第一个结点的下标给要回收结点的下标
	space[0].cur = k;				//space[0]的cur用来存放备用链表(空闲空间)第一个结点的下标

}

/* 初始条件：静态链表L已存在。操作结果：返回L中数据元素个数 */
int ListLength ( staticLinkList L )
{
	int num = 0; 				//存储静态链表L中元素的个数
	int i = L[MAXSIZE-1].cur;	//[MAXSIZE-1]用来存放第一个插入元素的下标
	while (i)
	{
		i = L[i].cur;           //遍历静态链表直到最后一个有值元素（其cur的值为0）
		++num;
	}

	return num;
}

/* 向静态链表L的位置i插入e元素 */
Status Ins_SSL (staticLinkList L,int i ,Elemtype e)
{
	if ( i<1 || i > ListLength(L)+1 ) //验证插入位置的合法性
		return ERR;
	int k = MAXSIZE - 1; 			 //[MAXSIZE-1]用来存放第一个插入元素的下标
	int idle_node = Malloc_SLL(L);  
	if (idle_node)					//如果没有空间Malloc_SLL(L)返回0复制给idle_node
	{
		L[idle_node].data = e; 
		for ( int pos = 1; pos <= i-1 ; pos++) //从[MAXSIZE-1].cur指向的第一个元素开始找到第i个元素之前的位置
		{
			k = L[k].cur; 					
		}
		L[idle_node].cur = L[k].cur; 		//将找到的第i个元素前的位置的cur（也就是下个节点的位置）给新节点的cur
		L[k].cur = idle_node; 				//将新插入元素的下标值给第i个元素前的位置的cur
		return OK;
	}else
	 
	return ERR;
}

/*  删除在L中第i个数据元素   */
Status Del_SSL(staticLinkList L,int i)
{
	if ( i<1 || i > ListLength(L)  ) //验证i位置的合法性
		return ERR;
	int k = MAXSIZE - 1;
	for ( int pos = 1; pos <= i-1 ; pos++) //从[MAXSIZE-1].cur指向的第一个元素开始找到第i个元素之前的位置
	{
		k = L[k].cur; 					
	}
	int temp = L[k].cur; 		//将要删除的结点的cur值，也就是后面结点的下标给temp
	L[k].cur = L[temp].cur; 	//将下面结点的cur值赋给第i个元素之前的位置
	Free_SSL(L,temp);			//将temp位置的结点释放

	return OK;
}

void Print_SSL (staticLinkList L)
{
	int i = L[MAXSIZE-1].cur;	//[MAXSIZE-1]用来存放第一个插入元素的下标
	while (i)
	{
		printf("%d ",L[i].data);
		i = L[i].cur;           //遍历静态链表直到最后一个有值元素（其cur的值为0）
	}
	printf("\n\n");
}

int main(int argc, char *argv[])
{
	int opt;
	int exit = OK;
	staticLinkList thisList;
	if (InitStaticList(thisList))
	printf("静态链表初始化成功！");
	else printf("静态链表初始化失败！");
	

	while(exit)
	{	
		printf("\n===================================================================================================== \n");
		printf(">>>请输入操作标号: 1.在第i个位置插入新元素e; 2.删除第i位元素; 3.查看当前表; 0.退出;\n");
		scanf("%d",&opt);
		switch (opt)
		{
			case 1:
			{
				int i,e;
				printf("请依次输入i和e的值:\n");
				scanf("%d %d",&i,&e);
				if(Ins_SSL(thisList,i,e))			
				{
					printf("插入成功，目前静态链表共有%d个元素，分别为:",ListLength(thisList));
					Print_SSL(thisList);
				}else{
					printf("插入失败!\n");
				}

			}	
				break;	 
			case 2:
				{
					int i;
					printf("请依次输入i的值:\n");
					scanf("%d",&i);
					if (thisList[MAXSIZE-1].cur)
					{	
						if (ListLength(thisList) == 1)
						{
							if (Del_SSL(thisList,i))
							{
								printf("删除成功，目前目前链表为空");
							}else{
								printf("删除失败!\n");
							}	
						}else{
							if (Del_SSL(thisList,i))
							{
								printf("删除成功，目前静态链表共有%d个元素，分别为:",ListLength(thisList));
								Print_SSL(thisList);
							}else{
								printf("删除失败!\n");
							}
						}
					}else{
						printf("删除失败，当前链表为空！");
					}
				}
				break;
			case 3:
				{
					if (thisList[MAXSIZE-1].cur)
					{
						printf("目前线性表元素为:");
						Print_SSL(thisList);

					}else{
						printf("链表为空! \n");
					}
				}
				break;
			case 0:
				exit = ERR;
				printf("Goodbye~\n");
				break;
			default:
				printf("输入错误请按标号重新输入！\n");
		}
	}
	return 0;
}

```
