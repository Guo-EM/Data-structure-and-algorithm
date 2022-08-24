# 基础算法
## 排序 
### 快速排序模板
```
#include<iostream>
using namespace std;

const int N = 1e6 + 10;

int n;
int q[N];

void quick_sort(int q[], int l, int r) //数组，左右边界
{
    if (l >= r ) return;
    
    int x = q[l], i = l - 1, j = r + 1; //取左边界作为x值
    while(i < j)
    {
        do i++; while (q[i] < x); //小于边界值 i 指针右移
        do j--; while (q[j] > x); //大于边界值 j 指针左移
        if(i < j) swap(q[i], q[j]);     
    }
    //递归处理
    quick_sort(q, l, j);
    quick_sort(q, j + 1, r);
}

int main()
{
    scanf("%d", &n);
    for(int i = 0; i < n; i++) scanf("%d", &q[i]);
    
    quick_sort(q, 0, n-1);
    
    for(int i = 0; i < n; i++) printf("%d", q[i]);
    
    return 0;
}
```
### 归并排序模板
```
#include<iostream>
using namespace std;

const int N = 1000010;

int n;
int q[N], tmp[N];
void merge_sort(int q[], int l, int r)
{
    if (l >= r) return;
    int mid = l + r >> 1;
    merge_sort(q, l, mid), merge_sort(q, mid + 1, r);
    
    int k = 0, i = l, j = mid + 1;
    while (i <= mid && j <= r)
    {
        if(q[i] <= q[j]) tmp[k ++] = q[i ++];
        else tmp[k++] = q[j ++];
    }
    while(i <= mid) tmp[k ++] = q[i ++];
    while(j <= r) tmp[k ++] = q[j ++];
    for (i = l, j = 0; i <= r; i ++, j++) q[i] = tmp[i];
}

int main()
{
    scanf("%d", &n);
    for (int i = 0; i < n; i ++) scanf("%d", &q[i]);
    
    merge_sort(q, 0, n-1);
    
    for(int i = 0; i < n; i ++) printf("%d", q[i]);
    
    return 0;
}
```
### 二分
#### 整数二分
```
//区间[l, r]被划分成[l, mid]和[mid + 1, r]时使用：
int bsearch_1(int l, int r)
{
    while(l < r)
    {
        int mid = l + r >> 1;
        if (check(mid)) r = mid; //check()判断mid是否满足性质
        else l = mid + 1;
    }
    return l;
}

//区间[l, r]被划分成[l, mid - 1]和[mid, r]时使用：
int bsearch_2(int l, int r)
{
    while(l < r)
    {
        int mid = l + r + 1 >> 1;
        if(check(mid)) l = mid;
        else r = mid - 1;
    }
    return l;
}
```
#### 浮点数二分
```
#include<iostream>
using namespace std;

int main()
{
    double x;
    cin >> x;
    double l = 0, r = x;
    while(r - l > 1e-6) //区间足够小时结束迭代，精度比要求精度高两位
    {
        double mid = (l + r) / 2;
        if (check(*****)) r = mid;
        else  l = mid;
    }
    printf("%lf\n", l);
    
    return 0;
}
```
