# -_-
5种排序算法（选择，冒泡，归并，快速，插入）5种规模（10,100,1000,10000,100000）的时间
#include<iostream>
#include<time.h>
using namespace std;
//****************************************************************
void SelectSort(int *p,int len)//选择排序,找到最小的放在前面
{
	int i,j,min,temp;
	
	for(i=0;i<len-1;i++)
	{
		min=i;
		for(j=i+1;j<len;j++)
		{
			if(*(p+min)>*(p+j))
			{
				min=j;//min保存最小的下标
			}
		}
		if(min!=i)//最小的不是初始的
		{
			temp=*(p+min);
			*(p+min)=*(p+i);
			*(p+i)=temp;
		}
	}
}
//****************************************************************
void BubbleSort(int *p,int len)//冒泡排序
{
	int i,j,temp;	
	
	for(i=0;i<len;i++)
		for(j=0;j<len-1-i;j++)
		{
			if(*(p+j)>*(p+j+1))
			{
				temp=*(p+j);
				*(p+j)=*(p+j+1);
				*(p+j+1)=temp;
			}
		}
}
//****************************************************************
void Merge(int R[],int low,int mid, int high)
//将两个相邻的有序表R[low..mid]和R[mid+1..high]合并为一个有序表R[low..high]
{
	int *R1=new int[high-low+1];
	int i=low;//第一个有序表下标
	int j=mid+1;//第二个有序表下标
	int k=0;//R1下标
	while(i<=mid&&j<=high)//1,2子表均未扫描完成
	{
		if(R[i]<=R[j])//记录1子表到R1
		{
			R1[k]=R[i];
			i++;
			k++;
		}
		else//记录2子表到R2
		{
			R1[k]=R[j];
			j++;
			k++;
		}
	}
	while(i<=mid)//2子表记录完成，将1子表剩下的记录到R1
	{
		R1[k]=R[i];
		i++;
		k++;
	}
	while(j<=high)//1子表记录完成，将2子表剩下的记录到R1
	{
		R1[k]=R[j];
		j++;
		k++;
	}
	for(k=0,i=low;i<=high;k++,i++)//R1复制回R中
	{
		R[i]=R1[k];
	}
	delete []R1;
}

void MergePass(int R[],int len,int n)
{
	int i;
	for(i=0;i+2*len-1<n;i=i+2*len)		//归并len长的两相邻子表
		Merge(R,i,i+len-1,i+2*len-1);
	if(i+len-1<n)						//余下两个子表，后者小于len
		Merge(R,i,i+len-1,n-1);			//
}

void MergeSort(int *p,int len)//归并排序
{
	int length;
	for(length=1;length<len;length=2*length)
	{
		MergePass(p,length,len);
	}
}
//****************************************************************
void QuickSort(int a[], int left, int right)  
{  
    if(left>right)  
    {  
        //左边下标比右边下标大，返回
        return ;  
    }  
	
    int i = left;  
    int j = right;  
    int x = a[i];  
	
    while(i<j)  
    {  
        while(i<j && a[j]>x) //向前搜索  
        {   j--;  }  
        if(i<j)  
        {  
            a[i] = a[j];  
            i++;  
        }  
		
        while(i<j && a[i]<x) //向后搜索  
        {   i++;    }  
        if(i<j)  
        {  
            a[j] = a[i];  
            j--;  
        }  
    }  
    a[i] = x;  
    QuickSort(a, left, i-1);  
    QuickSort(a, i+1, right);  
}  
//****************************************************************
void InsertSort(int R[],int len)//插入排序
{
	int i;
	int j;
	int temp;
	for(i=1;i<len;i++)//从第二个数据开始
	{
		if(R[i-1]>R[i])
		{
			temp=R[i];
			j=i-1;
			do
			{
				R[j+1]=R[j];//将i-1位置的值后移
				j--;
			}while(j>=0&&R[j]>temp);//直到遇到的值比i位置的小
			R[j+1]=temp;
		}
	}
}
//****************************************************************************
int* create_p(int len)//创建长度为len的随机数组
{
	int *p=new int[len];
	srand((unsigned)time(0));//以时间为种子设置随机数	
	for(int i=0;i<len;i++)
	{
		*(p+i)=rand();
	}
	return p;
}

//****************************************************************
int main()
{
	while(1)
	{
		int i,j,len,option;
		double start,finish;
		double T[20],sum;

		cout<<"请选择使用的排序算法:"<<endl;
		cout<<"1.选择排序	2.冒泡排序	3.合并排序	4.快速排序	5.插入排序"<<endl;
		cin>>option;
		
		if(option!=1&&option!=2&&option!=3&&option!=4&&option!=5)
		{
			break;
		}
		
		cout<<"算法规模:";
		cin>>len;
		if(len>100000)
			break;

		int *p=new int[len];
			
		for(j=0;j<20;j++)//20组时间
		{
			if(option==1)//选择排序
			{
				start=clock();
				for(i=0;i<(100000/len);i++)//循环取平均
				{	
					p=create_p(len);
					SelectSort(p,len);
					delete []p;
				}
				finish=clock();
			}
			
			else if(option==2)//冒泡排序
			{
				start=clock();
				for(i=0;i<(100000/len);i++)//循环取平均
				{	
					p=create_p(len);
					BubbleSort(p,len);
					delete []p;
				}
				finish=clock();
			}
			
			else if(option==3)//归并排序
			{
				start=clock();
				for(i=0;i<(100000/len);i++)//循环取平均
				{
					p=create_p(len);
					MergeSort(p,len);
					delete []p;
				}
				finish=clock();
			}
			
			else if(option==4)//快速排序
			{
				start=clock();
				for(i=0;i<(100000/len);i++)//循环取平均
				{
					p=create_p(len);					
					QuickSort(p,0,len-1);
					delete []p;
				}
				finish=clock();
			}
			
			else if(option==5)//插入排序
			{
				start=clock();
				for(i=0;i<(100000/len);i++)//循环取平均
				{
					p=create_p(len);					
					InsertSort(p,len);
					delete []p;
				}
				finish=clock();
			}
			
			T[j]=((finish-start)/(100000/len))/CLOCKS_PER_SEC;//平均时间	
			
		}			
		sum=0;
		cout<<"运行时间（单位:秒）"<<endl;
		for(j=0;j<20;j++)
		{
			cout<<T[j]<<" ";
			sum=sum+T[j];
		}
		cout<<endl;
		cout<<"平均时间:"<<sum/20<<endl;		
			
	}
	return 0;
}
