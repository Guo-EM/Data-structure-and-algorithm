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
    for (i = l, j = 0; i <= r; i ++, j++) q[i] = tmp[j];
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
##### 例题
```
//例：数的范围

#include<iostream>
using namespace std;

const int N = 1e6 + 10;

int q[N];

//如果出现多个目标寻找的是数的边界
int find_l(int q[], int target, int l, int r){
    if(l >= r) return 0;
    while(l < r){
        int mid = (l + r) >> 1;
        if(q[mid] >= target) r = mid;  //寻找的是大于target的第一个数 即左边界
        else l = mid + 1;
    }
    if(q[l] == target) return l;
    else return -1;
}

int find_r(int q[], int target, int l, int r){
    if (l >= r) return 0;
    while(l < r){
        int mid = (l + r + 1) >> 1;
        if(q[mid] <= target) l = mid; //寻找的是小于target的第一个数 即右边界
        else r = mid - 1;
    }
    if(q[l] == target) return l;
    else return -1;
}

int main(){
    int n, findn;
    scanf("%d%d", &n, &findn);
    int target[findn];
    for(int i = 0; i < n; i ++) scanf("%d", &q[i]);
    for(int j = 0; j < findn; j ++) scanf("%d", &target[j]);
    for(int k = 0; k < findn; k ++) {
        int left = find_l(q, target[k], 0, n - 1);
        int right = find_r(q, target[k], 0, n - 1);
        printf("%d %d\n", left, right);
    }
    return 0;
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
### 高精度
#### 高精度加法

```
#include<iostream>
#include<vector>
using namespace std;

const int N = 1e6 + 10;

// C = A + B
vector<int> add(vector<int> &A, vector<int> &B)
{
    vector<int> C;
    int t = 0;
    for(int i = 0; i < A.size() || i < B.size(); i ++)
    {
        if (i < A.size()) t += A[i];
        if (i < B.size()) t += B[i];
        t /= 10;
    }
    if (t) C.push_back(1);
    return C;
}


int main(){
    string a, b;
    vector<int>A, B;
    scanf("%s %s", &a, &b);
    for(int i = A.size() - 1; i >= 0; i --) A.push_back(a[i] - '0');
    for(int i = B.size() - 1; i >= 0; i --) B.push_back(b[i] - '0');

    auto C = add(A, B);

    for(int i = C.size() - 1; i >= 0; i --) printf("%d", C[i]);

    return 0;
}
```

#### 高精度减法

```

```

#### 高精度乘法

```

```

#### 高精度除法

```

```
