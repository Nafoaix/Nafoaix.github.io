---
title: 数据结构——栈与队列
tags:
  - 数据结构
  - 栈
  - 队列
  - 学习笔记
categories:
  - 数据结构
  - 线性表
  - 栈
toc: true
cover: 'https://cdn.nafx.top/post_cover/20230419222601.png'
top_img: 'https://cdn.nafx.top/post_cover/20230419222601.png'
abbrlink: '73783806'
date: 2021-06-19 20:30:44
---

## 栈 stack

栈是**限定仅在表尾进行插入和删除操作的线性表**。把允许插入和删除的一端称为**栈顶(top)**,另一端称为**栈底(bottom)**,不含任何数据元素的栈称为空栈。栈又称为**后进先出(Last In First Out)**的线性表，简称**LIFO**结构。

### 栈的顺序存储结构实现

因为栈本身属于线性表的特例，那么栈的顺序存储其实也是线性表顺序存储的简化，称为顺序栈。

sqstack.h
``` c
#ifndef SQSTACK_H
#define SQSTACK_H

/*
************************************************************************
*                                宏定义
************************************************************************
*/

#define OK 1
#define ERR 0
#define TRUE 1
#define FALSE 0
#define MAXSIZE 5 /* 存储空间初始分配量 */

typedef int Elemtype;
typedef int Status;

/*
************************************************************************
*                                结构体定义
************************************************************************
*/

typedef struct
{
    Elemtype data[MAXSIZE];
    int top;
}Sqstack;

/*
************************************************************************
*                                函数声明
************************************************************************
*/

Status InitStack(Sqstack *S);
Status ClearStack(Sqstack *S);
Status StackEmpty(Sqstack S);
int StackLength(Sqstack S);
Status GetTop(Sqstack S,Elemtype *e);
Status Push(Sqstack *S,Elemtype e);
Status Pop(Sqstack *S,Elemtype *e);
Status visit(Elemtype c);
Status StackTraverse(Sqstack S);

#endif /* SQSTACK_H */


```

sqstack.c
```c
#include "stdio.h"
#include "sqstack.h"

/*  构造一个空栈S */
Status InitStack(Sqstack *S)
{
	/* S.data=(SElemType *)malloc(MAXSIZE*sizeof(SElemType)); */
	S->top = -1;
	return OK;
}

/* 把S置为空栈 */
Status ClearStack(Sqstack *S)
{
	// while (S->top >= 0)
	// {
	// 	S->data[S->top] = 0;
	// 	S->top--;
	// }
	// //释放元素内存
	S->top = -1;
	return OK;
}

/* 若栈S为空栈，则返回TRUE，否则返回FALSE */
Status StackEmpty(Sqstack S)
{ 
    if (S.top==-1)
        return TRUE;
    else
        return FALSE;
}

/* 返回S的元素个数，即栈的长度 */
int StackLength(Sqstack S)
{
	return S.top+1;
}

/* 若栈不空，则用e返回S的栈顶元素，并返回OK；否则返回ERROR */
Status GetTop(Sqstack S,Elemtype *e)
{
        if (S.top==-1)
                return ERR;
        else
                *e=S.data[S.top];
        return OK;
}

/* 插入元素e为新的栈顶元素 */
Status Push(Sqstack *S,Elemtype e)
{	
	/* 栈满 */
	if ( S->top == MAXSIZE-1 )	
	{
		printf("栈满!");
		return ERR;
	}
	S->top++;
	S->data[S->top]=e;

	return OK;
}

/* 若栈不为空，删除栈S顶元素，用e返回其值*/
Status Pop(Sqstack *S,Elemtype *e)
{
	if (S->top == -1)
	{
		printf("栈空!");
		return ERR;
	}
	*e=S->data[S->top];
	S->top--;

	return OK;
}

Status visit(Elemtype c)
{
        printf("%d ",c);
        return OK;
}

/* 从栈底到栈顶依次对栈中每个元素显示 */
Status StackTraverse(Sqstack S)
{
        int i;
        i=0;
        while(i<=S.top)
        {
                visit(S.data[i++]);
        }
        printf("\n");
        return OK;
}


int main(int argc, char *argv[])
{
	int opt;
	int exit = OK;
	Sqstack test;
	InitStack(&test);

	while(exit)
	{
		printf("\n======================================================================================================================== \n");
		printf(">>>请输入操作标号: 1.初始化栈; 2.将元素e入栈; 3.将栈顶元素弹出; 4.将栈清空; 5.打印栈内元素; 0.退出;\n");
		scanf("%d",&opt);
		switch (opt)
		{
			case 1:
			{
				InitStack(&test);
				printf("初始化完成，目前栈为空!\n");
			}	
				break;	 
			case 2:
				{
					Elemtype val;
					printf("请输入入栈元素的值并按回车:\n");
					scanf("%d",&val);
					if(Push(&test,val))
					{
						printf("入栈成功!目前栈内有%d个元素。\n",StackLength(test));
					}
				}
				break;
			case 3:
				{
					Elemtype val;
					if (Pop(&test,&val))
					{
					printf("栈顶元素%d弹出,目前栈内有%d个元素。\n",val,StackLength(test));
					}
				}
				break;
			case 4:
				{
					if(ClearStack(&test))
					printf("清空完成，目前栈内有%d个元素。\n",StackLength(test));
				}
				break;
			case 5:
				{
					printf("目前栈内有%d个元素，分别是:\n",StackLength(test));
					StackTraverse(test);
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

### 栈的链式存储结构实现

LinkStack.h

``` c
#ifndef LINKSTACK_H
#define LINKSTACK_H

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

typedef struct StackNode
{
    Elemtype data;
    struct StackNode *next;
    
}StackNode,*LinkStackPtr;

typedef struct 
{
    LinkStackPtr top;
    int count;
}LinkStack;

/*
************************************************************************
*                                函数声明
************************************************************************
*/
Status visit(Elemtype c);
Status InitStack(LinkStack *S);
Status StackEmpty(LinkStack S);
Status ClearStack(LinkStack *S);
int StackLength(LinkStack S);
Status GetTop(LinkStack S,Elemtype *e);
Status Push( LinkStack *S,Elemtype e );
Status Pop( LinkStack *S,Elemtype *e );
Status StackTraverse(LinkStack S);

#endif /* LINKSTACK_H */
```

LinkStack.c

``` c
#include "stdio.h"
#include "stdlib.h"
#include "LinkStack.h"

/*  构造一个空栈S */
Status InitStack(LinkStack *S)
{ 
        // S->top = (LinkStackPtr)malloc(sizeof(StackNode)); /* 为头指针分配空间 */
        // if(!S->top)				/* 内存分配失败 */
        //         return ERR;
        // S->top->next=NULL;		/* 空栈S头指针指向空 */
		S->top = NULL;
        S->count=0;

        return OK;
}

/* 若栈S为空栈，则返回TRUE，否则返回FALSE */
Status StackEmpty(LinkStack S)
{ 
        if (S.count==0)
                return TRUE;
        else
                return FALSE;
}

/* 把S置为空栈 */
Status ClearStack(LinkStack *S)
{ 
        LinkStackPtr pxTop,temp;
        pxTop = S->top;	
        while(pxTop)		/* 直到头指针为空 */
        {  
                temp=pxTop;
                pxTop=pxTop->next;
                free(temp);
        } 
        S->count=0;
        return OK;
}

/* 返回S的元素个数，即栈的长度 */
int StackLength(LinkStack S)
{ 
        return S.count;
}

/* 若栈不空，则用e返回S的栈顶元素，并返回OK；否则返回ERROR */
Status GetTop(LinkStack S,Elemtype *e)
{
        if (S.top==NULL)
                return ERR;
        else
                *e=S.top->data;
        return OK;
}

/* 插入元素e为新的栈顶元素 */
Status Push( LinkStack *S,Elemtype e )
{
	LinkStackPtr newnode = (LinkStackPtr)malloc(sizeof(StackNode));
	newnode->data = e;
	newnode->next = S->top;
	S->top = newnode;
	S->count++;

	return OK;
}

/* 若栈非空，删除栈顶元素用e返回其值 */
Status Pop( LinkStack *S,Elemtype *e )
{
	LinkStackPtr pxTop;
	if (StackEmpty(*S))	/* 若栈空返回err */
		return ERR;
	pxTop = S->top;
	*e = pxTop->data;
	S->top = S->top->next; /* 使得栈顶指针下移一位，指向后一结点 */
	free(pxTop);
	S->count--;

	return OK;
}

Status visit(Elemtype c)
{
        printf("%d ",c);
        return OK;
}
/* 从栈底到栈顶依次对栈中每个元素显示 */
Status StackTraverse(LinkStack S)
{
		if (S.count == 0)
		{
			printf("栈空！\n");
			return ERR;
		}
		
        LinkStackPtr p=S.top;
        while(p)
        {
			visit(p->data);
			p=p->next;
        }
        printf("\n");
        return OK;
}

int main(int argc, char *argv[])
{
int opt;
	int exit = OK;
	LinkStack test;
	InitStack(&test);

	while(exit)
	{
		printf("\n======================================================================================================================== \n");
		printf(">>>请输入操作标号: 1.初始化栈; 2.将元素e入栈; 3.将栈顶元素弹出; 4.将栈清空; 5.打印栈内元素; 0.退出;\n");
		scanf("%d",&opt);
		switch (opt)
		{
			case 1:
			{
				InitStack(&test);
				printf("初始化完成，目前栈为空!\n");
			}	
				break;	 
			case 2:
				{
					Elemtype val;
					printf("请输入入栈元素的值并按回车:\n");
					scanf("%d",&val);
					if(Push(&test,val))
					{
						printf("入栈成功!目前栈内有%d个元素。\n",StackLength(test));
					}
				}
				break;
			case 3:
				{
					Elemtype val;
					if (Pop(&test,&val))
					{
					printf("栈顶元素%d弹出,目前栈内有%d个元素。\n",val,StackLength(test));
					}
				}
				break;
			case 4:
				{
					if(ClearStack(&test))
					printf("清空完成，目前栈内有%d个元素。\n",StackLength(test));
				}
				break;
			case 5:
				{
					printf("目前栈内有%d个元素，分别是:\n",StackLength(test));
					StackTraverse(test);
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

### 栈总结

由于链栈每个元素都有指针域增加了一些内存开销，所以在栈的大小相对确定或变化在可控范围内时最好使用顺序栈。反之如果栈中的元素变化不可预料，最好使用对于栈长度无限制的链栈。

## 队列 queue

队列(queue)是只允许在一端进行插入操作，而在另一端进行删除操作的线性表。把允许插入的一端称为队尾，允许删除的一端称为队头。队列又称为**先进先出(First In First Out)**的线性表，简称**FIFO**结构。

队列也是作为一种特殊的线性表，同样存在顺序存储、链式存储两种存储方式。

### 队列的顺序存储

sqqueue.h
``` c
#ifndef SQQUEUE_H
#define SQQUEUE_H

/*
************************************************************************
*                                宏定义
************************************************************************
*/
#define OK 1
#define ERR 0
#define TRUE 1
#define FALSE 0
#define MAXSIZE 20 /* 存储空间初始分配量 */

typedef int Elemtype;
typedef int Status;

/*
************************************************************************
*                                结构体定义
************************************************************************
*/
/* 循环队列的顺序存储结构 */
typedef struct
{
	Elemtype data[MAXSIZE];
	int front;    	/* 头指针 */
	int rear;		/* 尾指针，若队列不空，指向队列尾元素的下一个位置 */
}SqQueue;

/*
************************************************************************
*                                函数声明
************************************************************************
*/
Status visit(Elemtype c);
Status InitQueue(SqQueue *Q);
Status ClearQueue(SqQueue *Q);
Status QueueEmpty(SqQueue Q);
int QueueLength(SqQueue Q);
Status GetHead(SqQueue Q,Elemtype *e);
Status EnQueue(SqQueue *Q,Elemtype e);
Status DeQueue(SqQueue *Q,Elemtype *e);
Status QueueTraverse(SqQueue Q);

#endif /* SQQUEUE_H */

```

sqqueue.c

``` c
#include "stdio.h"
#include "stdlib.h"
#include "sqqueue.h"

Status visit(Elemtype c)
{
	printf("%d ",c);
	return OK;
}

/* 初始化一个空队列Q */
Status InitQueue(SqQueue *Q)
{
	Q->front=0;
	Q->rear=0;
	return  OK;
}

/* 将Q清为空队列 */
Status ClearQueue(SqQueue *Q)
{
	Q->front=Q->rear=0;
	return OK;
}

/* 若队列Q为空队列,则返回TRUE,否则返回FALSE */
Status QueueEmpty(SqQueue Q)
{ 
	if(Q.front==Q.rear) /* 队列空的标志 */
		return TRUE;
	else
		return FALSE;
}

/* 返回Q的元素个数，也就是队列的当前长度 */
int QueueLength(SqQueue Q)
{
	return  (Q.rear-Q.front+MAXSIZE)%MAXSIZE;
}

/* 若队列不空,则用e返回Q的队头元素,并返回OK,否则返回ERROR */
Status GetHead(SqQueue Q,Elemtype *e)
{
	if(Q.front==Q.rear) /* 队列空 */
		return ERR;
	*e=Q.data[Q.front];
	return OK;
}

/* 若队列未满，则插入元素e为Q新的队尾元素 */
Status EnQueue(SqQueue *Q,Elemtype e)
{
	if ((Q->rear+1)%MAXSIZE == Q->front)	/* 队列满的判断 */
		return ERR;
	Q->data[Q->rear]=e;			/* 将元素e赋值给队尾 */
	Q->rear=(Q->rear+1)%MAXSIZE;/* rear指针向后移一位置， */
								/* 若到最后则转到数组头部 */
	return  OK;
}

/* 若队列不空，则删除Q中队头元素，用e返回其值 */
Status DeQueue(SqQueue *Q,Elemtype *e)
{
	if (Q->front == Q->rear)			/* 队列空的判断 */
		return ERR;
	*e=Q->data[Q->front];				/* 将队头元素赋值给e */
	Q->front=(Q->front+1)%MAXSIZE;	/* front指针向后移一位置， */
									/* 若到最后则转到数组头部 */
	return  OK;
}

/* 从队头到队尾依次对队列Q中每个元素输出 */
Status QueueTraverse(SqQueue Q)
{ 
	int i;
	i=Q.front;
	while(i!=Q.rear)
	{
		visit(Q.data[i]);
		i=(i+1)%MAXSIZE;
	}
	printf("\n");
	return OK;
}

int main(int argc, char *argv[])
{
int opt;
	int exit = OK;
	SqQueue test;
	InitQueue(&test);

	while(exit)
	{
		printf("\n======================================================================================================================== \n");
		printf(">>>请输入操作标号: 1.初始化队列; 2.队尾入队 3.队头出队 4.队列清空; 5.打印队列内元素; 0.退出;\n");
		scanf("%d",&opt);
		switch (opt)
		{
			case 1:
			{
				InitQueue(&test);
				printf("初始化完成，目前队列为空!\n");
			}	
				break;	 
			case 2:
				{
					Elemtype val;
					printf("请输入入队元素的值并按回车:\n");
					scanf("%d",&val);
					if(EnQueue(&test,val))
					{
						printf("入队成功!目前栈内有%d个元素。\n",QueueLength(test));
					}
				}
				break;
			case 3:
				{
					Elemtype val;
					if (DeQueue(&test,&val))
					{
					printf("队头元素%d弹出,目前队内有%d个元素。\n",val,QueueLength(test));
					}else{
					printf("操作失败，当前队列空！");
					}
				}
				break;
			case 4:
				{
					if(ClearQueue(&test))
					printf("清空完成，目前队内有%d个元素。\n",QueueLength(test));
				}
				break;
			case 5:
				{
					printf("目前站内有%d个元素，分别是:\n",QueueLength(test));
					QueueTraverse(test);
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

### 队列的链式存储

linkqueue.h

``` c
#ifndef LINKQUEUE_H
#define LINKQUEUE_H

/*
************************************************************************
*                                宏定义
************************************************************************
*/
#define OK 1
#define ERR 0
#define TRUE 1
#define FALSE 0
#define MAXSIZE 20 /* 存储空间初始分配量 */
#define OVERFLOW 3 // overflow range error in math.h

typedef int Elemtype;
typedef int Status;

/*
************************************************************************
*                                结构体定义
************************************************************************
*/
typedef struct QNode	/* 结点结构 */
{
   Elemtype data;
   struct QNode *next;
}QNode,*QueuePtr;

typedef struct			/* 队列的链表结构 */
{
   QueuePtr front,rear; /* 队头、队尾指针 */
}LinkQueue;

/*
************************************************************************
*                                函数声明
************************************************************************
*/
Status visit(Elemtype c);
Status InitQueue(LinkQueue *Q);
Status DestroyQueue(LinkQueue *Q);
Status ClearQueue(LinkQueue *Q);
Status QueueEmpty(LinkQueue Q);
int QueueLength(LinkQueue Q);
Status GetHead(LinkQueue Q,Elemtype *e);
Status EnQueue(LinkQueue *Q,Elemtype e);
Status DeQueue(LinkQueue *Q,Elemtype *e);
Status QueueTraverse(LinkQueue Q);

#endif /* LINKQUEUE_H 

```

linkqueue.c

``` c
#include "stdio.h"
#include "stdlib.h"
#include "linkqueue.h"

Status visit(Elemtype c)
{
	printf("%d ",c);
	return OK;
}

/* 构造一个空队列Q */
Status InitQueue(LinkQueue *Q)
{ 
	Q->front=Q->rear=(QueuePtr)malloc(sizeof(QNode));
	if(!Q->front)
	{
		exit(OVERFLOW);
		printf("OVERFLOW!");
	}		
	Q->front->next=NULL;
	return OK;
}

/* 销毁队列Q */
Status DestroyQueue(LinkQueue *Q)
{
	while(Q->front)
	{
		 Q->rear=Q->front->next;
		 free(Q->front);
		 Q->front=Q->rear;
	}
	return OK;
}

/* 将Q清为空队列 */
Status ClearQueue(LinkQueue *Q)
{
	QueuePtr p,q;
	Q->rear=Q->front;
	p=Q->front->next;
	Q->front->next=NULL;
	while(p)
	{
		 q=p;
		 p=p->next;
		 free(q);
	}
	return OK;
}

/* 若Q为空队列,则返回TRUE,否则返回FALSE */
Status QueueEmpty(LinkQueue Q)
{ 
	if(Q.front==Q.rear)
		return TRUE;
	else
		return FALSE;
}

/* 求队列的长度 */
int QueueLength(LinkQueue Q)
{ 
	int i=0;
	QueuePtr p;
	p=Q.front;
	while(Q.rear!=p)
	{
		 i++;
		 p=p->next;
	}
	return i;
}

/* 若队列不空,则用e返回Q的队头元素,并返回OK,否则返回ERROR */
Status GetHead(LinkQueue Q,Elemtype *e)
{ 
	QueuePtr p;
	if(Q.front==Q.rear)
		return ERR;
	p=Q.front->next;
	*e=p->data;
	return OK;
}


/* 插入元素e为Q的新的队尾元素 */
Status EnQueue(LinkQueue *Q,Elemtype e)
{ 
	QueuePtr s=(QueuePtr)malloc(sizeof(QNode));
	if(!s) /* 存储分配失败 */
		exit(OVERFLOW);
	s->data=e;
	s->next=NULL;
	Q->rear->next=s;	/* 把拥有元素e的新结点s赋值给原队尾结点的后继 */
	Q->rear=s;		/* 把当前的s设置为队尾结点，rear指向s */
	return OK;
}

/* 若队列不空,删除Q的队头元素,用e返回其值,并返回OK,否则返回ERROR */
Status DeQueue(LinkQueue *Q,Elemtype *e)
{
	QueuePtr p;
	if(Q->front==Q->rear)
		return ERR;
	p=Q->front->next;		/* 将欲删除的队头结点暂存给p*/
	*e=p->data;				/* 将欲删除的队头结点的值赋值给e */
	Q->front->next=p->next;/* 将原队头结点的后继p->next赋值给头结点后继 */
	if(Q->rear==p)		/* 若队头就是队尾，则删除后将rear指向头结点  */
		Q->rear=Q->front;
	free(p);
	return OK;
}

/* 从队头到队尾依次对队列Q中每个元素输出 */
Status QueueTraverse(LinkQueue Q)
{
	QueuePtr p;
	p=Q.front->next;
	while(p)
	{
		 visit(p->data);
		 p=p->next;
	}
	printf("\n");
	return OK;
}

int main(int argc, char *argv[])
{
int opt;
	int exit = OK;
	LinkQueue test;
	InitQueue(&test);

	while(exit)
	{
		printf("\n======================================================================================================================== \n");
		printf(">>>请输入操作标号: 1.初始化队列; 2.队尾入队 3.队头出队 4.队列清空; 5.打印队列内元素; 0.退出;\n");
		scanf("%d",&opt);
		switch (opt)
		{
			case 1:
			{
				InitQueue(&test);
				printf("初始化完成，目前队列为空!\n");
			}	
				break;	 
			case 2:
				{
					Elemtype val;
					printf("请输入入队元素的值并按回车:\n");
					scanf("%d",&val);
					if(EnQueue(&test,val))
					{
						printf("入队成功!目前栈内有%d个元素。\n",QueueLength(test));
					}
				}
				break;
			case 3:
				{
					Elemtype val;
					if (DeQueue(&test,&val))
					{
					printf("队头元素%d弹出,目前队内有%d个元素。\n",val,QueueLength(test));
					}else{
					printf("操作失败，当前队列空！");
					}
				}
				break;
			case 4:
				{
					if(ClearQueue(&test))
					printf("清空完成，目前队内有%d个元素。\n",QueueLength(test));
				}
				break;
			case 5:
				{
					printf("目前站内有%d个元素，分别是:\n",QueueLength(test));
					QueueTraverse(test);
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

### 队列总结

链队列适用于需要支持任意长度的队列，且对入队、出队和删除操作都需要支持的场景，而如果对访问效率要求较高的场景，则需要考虑使用循环队列。
