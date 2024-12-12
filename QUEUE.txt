
#include<stdio.h> 
#define n 5 
int main() 
{ 
int queue[n],ch=1,front=0,rear=0,i,j=1,x=n; 
printf("Queue using Array"); 
printf("\n1.Insertion \n2.Deletion \n3.Display \n4.Exit"); 
while(ch!=4) 
{ 
printf("\n Enter the choice:"); 
scanf("%d",&ch); 
switch(ch) 
{ 
case 1: 
if(rear==x) 
printf("\nQueue is full"); 
else 
{ 
printf("\nEnter no %d:",j++); 
scanf("%d",&queue[rear++]); 
} 
break; 
case 2: 
if(front==rear) 
{ 
printf("\nQueue is empty"); 
} 
else 
{ 
printf("\n Deleted element is %d",queue[front++]); 
x++; 
} 
break; 
case 3: 
printf("\n Queue elements are:\n"); 
if(front==rear) 
printf("\n Queue is empty"); 
else 
{ 
for(i=front;i<rear;i++) 
{ 
printf("%d",queue[i]); 
printf("\n"); 
} 
} 
break; 
case 4: 
{ 
break; 
} 
} 
} 
return 0; 
}