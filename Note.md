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
## 二分
### 整数二分
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
### 浮点数二分
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
## 高精度
### 高精度加法

```
#include<iostream>
#include<vector>
using namespace std;

// C = A + B
vector<int> add(vector<int> &A, vector<int> &B)
{
    vector<int> C;
    int t = 0;
    for(int i = 0; i < A.size() || i < B.size(); i ++)
    {
        if (i < A.size()) t += A[i];
        if (i < B.size()) t += B[i];
        C.push_back(t % 10);
        t /= 10;
    }
    if (t) C.push_back(1);
    return C;
}

int main(){
    string a, b;
    vector<int>A, B;
    cin >> a >> b;
    for(int i = a.size() - 1; i >= 0; i --) A.push_back(a[i] - '0');
    for(int i = b.size() - 1; i >= 0; i --) B.push_back(b[i] - '0');
    auto C = add(A, B);
    for(int i = C.size() - 1; i >= 0; i --) printf("%d", C[i]);
    return 0;
}
```

### 高精度减法

```
#include<iostream>
#include<vector>
using namespace std;

//判断是否有 A >= B
bool cmp(vector<int> &A, vector<int> &B)
{
    if(A.size() != B.size()) return A.size() > B.size();
    for(int i = A.size() - 1; i >= 0; i --)
        if (A[i] != B[i])
            return A[i] > B[i];
    return true;
}
// C = A - B
vector<int> sub(vector<int> A, vector<int> B)
{
    vector<int> C;
    for(int i = 0, t = 0; i < A.size(); i ++)
    {
        t = A[i] - t;
        if (i < B.size()) t -= B[i];
        C.push_back((t + 10) % 10);
        t = (t < 0) ? 1 : 0;
    }
    while(C.size() > 1 && C.back() == 0) C.pop_back(); //去掉前导0
    return C;
}

int main(){
    string a, b;
    vector<int>A, B;
    cin >> a >> b;
    for(int i = a.size() - 1; i >= 0; i --) A.push_back(a[i] - '0');
    for(int i = b.size() - 1; i >= 0; i --) B.push_back(b[i] - '0');

    if(cmp(A, B))
    {
        auto C = sub(A, B);
        for(int i = C.size() - 1; i >= 0; i --) printf("%d", C[i]);
    }
    else
    {
        auto C = sub(A, B);
        printf("-");
        for(int i = C.size() - 1; i >= 0; i --) printf("%d", C[i]);
    }

    return 0;
}
```

### 高精度乘法

```
#include<iostream>
#include<vector>
using namespace std;

//判断是否有 A >= B
vector<int> mul(vector<int> &A, int B)
{
    vector<int> C;
    for (int i = 0, t = 0; i < A.size() || t; i ++)
    {
        if (i < A.size()) t += A[i] * B;
        C.push_back(t % 10);
        t /= 10;
    }
    return C;
}

int main(){
    string a;
    int B;
    vector<int>A;
    cin >> a >> B;
    for(int i = a.size() - 1; i >= 0; i --) A.push_back(a[i] - '0');

    auto C = mul(A, B);

    for(int i = C.size() - 1; i >= 0; i --) printf("%d", C[i]);

    return 0;
}
```

### 高精度除法

```
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

//判断是否有 A >= B
vector<int> div(vector<int> &A, int b, int &r) // r 是引用 表示余数
{
    vector<int> C; //商
    r = 0;
    for (int i = A.size() - 1; i >= 0; i --)
    {
        r = r * 10 + A[i];
        C.push_back(r / b);
        r %= b;
    }
    reverse(C.begin(), C.end());
    while(C.size() > 1 && C.back() == 0) C.pop_back();
    return C;
}

int main(){
    string a;
    int b, r;
    vector<int>A;
    cin >> a >> b;
    for(int i = a.size() - 1; i >= 0; i --) A.push_back(a[i] - '0');

    auto C = div(A, b, r);

    for(int i = C.size() - 1; i >= 0; i --) printf("%d", C[i]);
    cout << endl << r << endl;

    return 0;
}
```
## 前缀和与差分
### 前缀和

```
#include<iostream>
#include<vector>
using namespace std;

// C = A + B
vector<int> add(vector<int> &A, vector<int> &B)
{
    vector<int> C;
    int t = 0;
    for(int i = 0; i < A.size() || i < B.size(); i ++)
    {
        if (i < A.size()) t += A[i];
        if (i < B.size()) t += B[i];
        C.push_back(t % 10);
        t /= 10;
    }
    if (t) C.push_back(1);
    return C;
}


int main(){
    string a, b;
    vector<int>A, B;
    cin >> a >> b;
    for(int i = a.size() - 1; i >= 0; i --) A.push_back(a[i] - '0');
    for(int i = b.size() - 1; i >= 0; i --) B.push_back(b[i] - '0');

    auto C = add(A, B);

    for(int i = C.size() - 1; i >= 0; i --) printf("%d", C[i]);

    return 0;
}
```
### 子矩阵的和

```
#include<iostream>

using namespace std;

const int N = 1010;
int n, m, q;
int a[N][N], s[N][N];

int main()
{
    scanf("%d%d%d", &n, &m, &q);
    for (int i = 1; i <= n; i ++)
        for(int j = 1; j <= m; j ++)
            scanf("%d", &a[i][j]);

    for (int i = 1; i <= n; i ++)
        for (int j = 1; j <= m; j++)
            s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + a[i][j];
    //求前缀和

    while (q--)
    {
        int x1, y1, x2, y2;
        scanf("%d%d%d", &x1, &y1, &x2, &y2);
        printf("%d\n", s[x2][y2] - s[x1 - 1][y2] - s[x2][y1 - 1] + s[x1 - 1][y1 - 1]);
        //算子矩阵的和
    }
    return 0;
}
```
### 差分

```
#include<iostream>

using namespace std;

const int N = 1010;
int n, m;
int a[N], b[N];

void insert(int l, int r, int c)
{
    b[l] += c;
    b[r + 1] -= c;
}

int main()
{
    scanf("%d%d", &n, &m);
    for(int i = 1; i <= n; i++) scanf("%d", &a[i]);

    for(int i = 1; i <= n; i ++) insert(i, i, a[i]);

    while(m --)
    {
        int l, r, c;
        scanf("%d%d%d", l, r, c);
        insert(l, r, c);
    }

    for(int i = 1; i <= n; i ++) b[i] += b[i - 1];

    for(int i = 1; i <= n; i ++) printf("%d ", b[i]);

    return 0;
}
```
### 差分矩阵

```
#include<iostream>

using namespace std;

const int N = 1010;
int n, m, q;
int a[N][N], b[N][N];

void insert(int x1, int y1, int x2, int y2, int c)
{
    b[x1][y1] += c;
    b[x2 + 1][y1] -= c;
    b[x1][y2 + 1] -= c;
    b[x1 + 1][y1 + 1] += c;
}

int main()
{
    scanf("%d%d%d", &n, &m, &q);
    for(int i = 1; i <= n; i++)
        for (int j = 1; j <= m; j++)
            scanf("%d", &a[i][j]);

    for(int i = 1; i <= n; i ++)
        for(int j = 1; j <= m; j ++)
            insert(i, j, i, j, a[i][j]);

    while(q --)
    {
        int x1, y1, x2, y2, c;
        scanf("%d%d%d%d%d", x1, y1, x2, y2, c);
        insert(x1, y1, x2, y2, c);
    }

    for(int i = 1; i <= n; i ++)
        for(int j = 1; j <= m; j++)
            b[i][j] += b[i - 1][j] + b[i][j - 1] - b[i - 1][j - 1];

    for(int i = 1; i <= n; i ++)
    {
        for(int j = 1; j <= m; j ++) printf("%d ", b[i][j]);
        puts("");
    }
    return 0;
}
```
## 双指针算法
### 最长连续不重复子序列

```
#include<iostream>
using namespace std;

const int N = 1e5 +10;

int a[N];
int s[N];
int n;

int main(){
    cin >> n;
    for(int i = 0; i < n; i ++) cin >> a[i];

    int res = 0;
    for(int i = 0, j = 0; i < n; i++) {
        s[a[i]]++;
        while (s[a[i]] > 1) {
            s[a[j]]--;
            j++;
        }
        res = max(res, i - j + 1);
    }
    cout << res << endl;
    return 0;
}
```

## 位运算
### 二进制中1的个数
```
#include<iostream>
using namespace std;

int lowbit(int x){
    return x & -x;
}

int main(){
    int n;
    cin >> n;
    while(n --){
        int x;
        cin >> x;
        int res = 0;
        while(x) x -= lowbit(x), res ++;
        cout << res << ' ';
    }
    return 0;
}
```
## 离散化
### 区间和
```
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

typedef pair<int, int> PII;

const int N = 300010;
int n, m;
int a[N], s[N];
vector<int> alls;
vector<PII> add, query;

int find(int x){
    //求离散化之后的结果
    int l = 0, r = alls.size() - 1;
    while(l < r){
        int mid = l + r >> 1;
        if(alls[mid] >= x) r = mid;
        else l = mid + 1;
    }
    return r + 1;
}

int main(){
    cin >> n >> m;
    for (int i = 0; i < n; i ++){
        int x, c;
        cin >> x >> c;
        add.push_back({x, c});
        alls.push_back(x);
    }
    for(int i = 0; i < m; i ++){
        int l, r;
        cin >> l >> r;
        query.push_back({l, r});
        alls.push_back(l);
        alls.push_back(r);
    }

    //去重
    sort(alls.begin(), alls.end());
    alls.erase(unique(alls.begin(), alls.end()), alls.end());

    //处理插入
    for (auto item : add){
        int x = find(item.first);
        a[x] += item.second;
    }

    //预处理前缀和
    for(int i = 1; i <= alls.size(); i ++) s[i] = s[i - 1] + a[i];

    //处理询问
    for(auto item : query){
        int l = find(item.first), r = find(item.second);
        cout << s[r] - s[l - 1] << endl;
    }

    return 0;
}
```

## 区间和并
### 区间和并

```
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

typedef pair<int, int> PII;

const int N = 100010;
int n;
vector<PII> segs;

void merge(vector<PII> &segs){
    vector<PII> res;
    sort(segs.begin(), segs.end());
    for(auto seg : segs) {
        cout << seg.first << " " << seg.second;
        cout << endl;
    }
    int st = -2e9, ed = -2e9;
    for(auto seg : segs){
        if(ed < seg.first){
            if(st != -2e9) res.push_back({st, ed});
            st = seg.first, ed = seg.second;
        }
        else ed = max(ed, seg.second);
    }
    if (st != -2e9) res.push_back({st, ed});
    segs = res;
}

int main(){
    cin >> n;
    for (int i = 0; i < n; i ++){
        int l, r;
        cin >> l >> r;
        segs.push_back({l, r});
    }
    merge(segs);

    cout << segs.size() << endl;

    return 0;
}
```
