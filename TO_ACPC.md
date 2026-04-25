# ACM ICPC Team Reference

## Table of Contents
- [Template](#template)
- [Include & define](#include--define)
- [Merge_Segmants](#mergesegmants)
- [MEX](#mex)
- [Co-ordinate Compression](#co-ordinate-compression)
- [2D Partial](#2d-partial)
- [2D Prefix](#2d-prefix)
- [Rotate Matrix 90 Degree](#rotate-matrix-90-degree)
- [Counting](#counting)
- [isPrime](#isprime)
- [Sieve](#sieve)
- [Prime factors](#prime-factors)
- [DivisibleBy N](#divisibleby-n)
- [Divisors](#divisors)
- [divUpTo1e18](#divupto1e18)
- [Matrix](#matrix)
- [ax+by+c = 0](#axbyc--0)
- [countPairsWithGCDX](#countpairswithgcdx)
- [GCD & LCM](#gcd--lcm)
- [DSU](#dsu)
- [Bridges](#bridges)
- [LCA (Euler)](#lca-euler)
- [LCA](#lca)
- [Seg Tree](#seg-tree)
- [Seg Tree Lazy](#seg-tree-lazy)
- [2D Seg Tree](#2d-seg-tree)
- [BIT](#bit)
- [SpareTable](#sparetable)
- [SQRT](#sqrt)
- [MOOOOoo](#moooooo)
- [Merge Sort Tree](#merge-sort-tree)
- [Ordered Set](#ordered-set)
- [XOR Basis](#xor-basis)
- [Hashing](#hashing)
- [KMP](#kmp)
- [HLD](#hld)
- [DSU on Tree](#dsu-on-tree)
- [Hashing 1](#hashing-1)
- [Hashing 2](#hashing-2)
- [Trie](#trie)


---

## Template

```cpp
#include <bits/stdc++.h>
#include<chrono>
#define int long long
#define ld long double
#define endl '\n'
#define ep cout << fixed << setprecision(10)
#define all(x) x.begin(), x.end()
#define allr(x) x.rbegin(), x.rend()
#define FIO ios_base::sync_with_stdio(false), cin.tie(nullptr), cout.tie(nullptr);
// count the leading zeros __builtin_clz()
// count the trailing zeros __builtin_ctz()
// count the number of set bits
__builtin_popcount()
// count the number of set bits in long long
__builtin_popcountll()
#pragma GCC optimize("O3,unroll-loops")
#pragma GCC
target("avx2,bmi,bmi2,lzcnt,popcnt")
int dx_all[8] = { 1, 0, -1, 0, 1, 1, -1, -1 }, dy_all[8] = { 0, 1, 0, -1, -1, 1, -1, 1 };
int dx[4] = { 0, 1, -1, 0 }, dy[4] = { 1, 0, 0, -1 };
int knight_dx[] = { +2, +1, -1, -2, -2, -1, +1, +2 }, knight_dy[] = { +1, +2, +2, +1, -1, -2, -2, -1 };
using namespace std;
```

## Merge_Segmants

```cpp
void Merge_Segmants() {
    vector<int> left, right;
    vector <pair<int, int>> ranges;
    sort(ranges.begin(), ranges.end());
    int st = ranges[0].first, en = ranges[0].second;
    for (int i = 1; i < ranges.size(); i++) {
        if (ranges[i].first < en)
        en = max(en, ranges[i].second);
        else {
            left.push_back(st);
            right.push_back(en);
            st = ranges[i].first;
            en = ranges[i].second;
        }
    }
    left.push_back(st); right.push_back(en);
    int m = left.size();
}
```

## MEX

```cpp
int MEX(vector<int>& v){
    int n = v.size();
    bool exists[n];
    memset(exists, 0, sizeof exists);
    for (auto element : v)
    if (element < n) exists[element] = true;
    for (int i = 0; i < n; ++i)
    if (!exists[i]) return i;
    return n;}
```

## Co-ordinate Compression

```cpp
vector<int> compress(vector<int>&v) {
    vector<int> temp = v;
    sort(temp.begin(), temp.end());
    temp.erase(unique(temp.begin(), temp.end()), temp.end());
    vector<int> ret;
    for (int x : v) {
        ret.push_back(lower_bound(temp.begin(), temp.end(), x) - temp.begin());
    } return ret;
}
```

## 2D Partial

```cpp
void PartialSum_in2D(int x1, int y1, int x2, int
y2, vector<vector<int>>& Prefix) {
    if (x1 > x2) swap(x1, x2);
    if (y1 > y2) swap(y1, y2);
    Prefix[x1][y1]++; Prefix[x2 + 1][y1]--;
    Prefix[x1][y2 + 1]--;Prefix[x2 + 1][y2 + 1]++;
}
```

## 2D Prefix

```cpp
vector<vector<int>> buildPrefixSum2D(const
vector<vector<int>>& grid) {
    int n = grid.size(), m = grid[0].size();
    vector<vector<int>> pref(n + 1, vector<int>(m + 1, 0));
    for (int i = 1; i <= n; ++i)
    for (int j = 1; j <= m; ++j)
    pref[i][j] = grid[i - 1][j - 1] + pref[i - 1][j] + pref[i][j - 1] - pref[i - 1][j - 1];
    return pref;}
int queryPrefixSum2D(const
vector<vector<int>>& pref, int x1, int y1, int
x2, int y2) {
    // x1, y1, x2, y2 are 1-based and inclusive
    return pref[x2][y2] - pref[x1 - 1][y2] - pref[x2][y1 - 1] + pref[x1 - 1][y1 - 1];
}
// minimum adjacent swaps to convert a to b
int cost(vector<int>& a, vector<int>& b) {
    int n = a.size();
    map<int, deque<int>> pos;
    ordered_set<int> st;
    int res = 0;
    for (int i = 0; i < n; ++i) {
        pos[a[i]].push_back(i);
        st.insert(i);
    }
    for (int i = 0; i < n; ++i) {
        int idx = pos[b[i]].front();
        pos[b[i]].pop_front();
        res += st.order_of_key(idx);
        st.erase(idx);
    } return res;
}
```

## Rotate Matrix 90 Degree

```cpp
vector<vector<int>> rotate90(vector<vector<int>>& g) {
    int n = g.size(), m = g[0].size();
    vector res(m, vector<int>(n));
    for (int i = 0; i < m; ++i)
    for (int j = 0; j < n; ++j)
    res[i][j] = g[n - j - 1][i];
    return res;
}
```

## Counting

```cpp
mod = 1e9 + 7;
vector<int> fact, invr;
int mod_add(int a, int b) { return (a + b) % mod; }
int mod_sub(int a, int b) { return (a - b + mod)
    % mod; }
int mod_mul(int a, int b) { return (1LL * a * b)
    % mod; }
int power(int a, int b) {
    int res = 1;
    while (b) {
        if (b & 1) res = mod_mul(res, a);
        a = mod_mul(a, a);
        b >>= 1;
    }
    return res;
}
int mod_inv(int a) {
    return power(a, mod - 2);
}
void init_comb(int n) {
    fact.assign(n + 1, 1);
    invr.assign(n + 1, 1);
    for (int i = 1; i <= n; ++i)
    fact[i] = mod_mul(fact[i - 1], i);
    invr[n] = mod_inv(fact[n]);
    for (int i = n - 1; i >= 0; --i)
    invr[i] = mod_mul(invr[i + 1], i + 1);
}
int nPr(int n, int r) {
    if (n < r) return 0;
    return mod_mul(fact[n], invr[n - r]);
}
int nCr(int n, int r) {
    if (n < r) return 0;
    return mod_mul(fact[n], mod_mul(invr[r], invr[n - r]));
}
int SandB(int n, int k) {
    if (std::min(n, k) < 0) return 0;
    return nCr(n + k - 1, k - 1);
}
int catalan(int n) {
    int c = nCr(n * 2, n);
    return mod_mul(c, mod_inv(n + 1));
}
```

## isPrime

```cpp
bool isPrime(int n) { // O(sqrt(n))
    for (int i = 2; i * i <= n; i++) {
        if (n % i == 0) {
            return false;
        }
    }
    return true;
}
Sieve1
const int N = 1e7 + 6;
bool is_prime[N];
void sieve() { // O(N) ~= O(N*log(log(N)))
    fill(is_prime, is_prime + N, true);
    is_prime[0] = is_prime[1] = false;
    for (int p = 2; p * p < N; p++) {
        if (is_prime[p]) {
            for (int m = p * p; m < N; m += p) {
                is_prime[m] = false;
            }
        }
    }
}
Sieve2
// pr is all the primes, low[x] is the lowest
prime of x
vector<int> pr, low;
void Sieve(int n) {
    low.assign(n + 1, 0);
    for (int i = 2; i <= n; ++i) {
        if (!low[i]) {
            low[i] = i;
            pr.push_back(i);
        }
        for (int& j : pr) {
            if (j > low[i] || i * j > n) break;
            low[i * j] = j;
        }
    }
}
Prime factors log
// prime factors in log x
vector<int> pf(int x) {
    vector<int> factors;
    while (x > 1) {
        factors.push_back(low[x]);
        x /= low[x];
    }
    return factors;
}
Prime factor sqrt
vector<pair<int, int> > getPrimeFactors(ll n) {
    // O(sqrt(n))
    vector<pair<int, int> > ret;
    for (int p = 2; p * p <= n; p++) {
        int e = 0;
        while (n % p == 0) {
            n /= p;
            e++;
        }
        if (e > 0) {
            ret.push_back({ p, e });
        }
    }
    if (n > 1) {
        ret.push_back({ n, 1 });
    }
    return ret;
}
```

## DivisibleBy N

```cpp
bool isDivisibleByN(string num , int n) {
    int remainder = 0;
    for (char digit : num) {
        remainder = (remainder * 10 + (digit - '0')) % n;
    } return remainder == 0;
}
```

## Divisors

```cpp
vector<int> divisors(int n) {
    vector<int> ret;
    for (int i = 1; i * i <= n; ++i) {
        if (n % i == 0) {
            ret.push_back(i);
            if (i * i != n) ret.push_back(n / i);
        }
    }return ret;
}
int divUpTo1e18(int N) {
    int ans = 1; // num of div up to 1e18
    for (int p : pr) {
        if (p * p * p > N) { break; }
        int count = 1;
        while (N % p == 0) { N /= p; count++; }
        ans *= count;
    }
    if (N > 1) {
        if (N <= 1e6) {
            if (low[N] == N) { ans *= 2; }
            else {
                int root = sqrt(N);
                if (root * root == N && low[root] == root) {
                    ans *= 3;
                }
                else {
                    ans *= 4;
                }
            }
        }
        else {
            bool is_prime = true;
            for (long long p : pr) {
                if (p * p > N) break;
                if (N % p == 0) {
                    is_prime = false;
                    break;
                }
            }
            if (is_prime) {
                ans *= 2;
            }
            else {
                int root = sqrt(N);
                if (root * root == N) {
                    bool root_is_prime = true;
                    for (long long p : pr) {
                        if (p * p > root) break;
                        if (root % p == 0) {
                            root_is_prime = false;
                            break;
                        }
                    }
                    if (root_is_prime) {
                        ans *= 3;
                    }
                    else {
                        ans *= 4;
                    }
                }
                else {
                    ans *= 4;
                }
            }
        }
    }
    return ans;
}
int xorall(int n) {
    if (n % 4 == 0) return n;
    if (n % 4 == 1) return 1;
    if (n % 4 == 2) return n + 1;
    return 0;}
```

## Matrix

```cpp
const int mod = 1e9 + 7;
using Row = vector<int>;
using Matrix = vector<Row>;
Matrix mul(Matrix& a, Matrix& b) {
    int n = a.size(), m = a[0].size(), k = b[0].size();
    Matrix res(n, Row(k));
    for (int i = 0; i < n; ++i)
    for (int j = 0; j < k; ++j)
    for (int o = 0; o < m; ++o) {
        res[i][j] += 1ll * a[i][o] * b[o][j] % mod;
        if (res[i][j] >= mod) res[i][j] -= mod;
        if (res[i][j] < 0) res[i][j] += mod;
    }
    return res;}
Matrix power(Matrix a, int b) {
    int n = a.size();
    Matrix res(n, Row(n));
    for (int i = 0; i < n; ++i) res[i][i] = 1;
    while (b) {
        if (b & 1) res = mul(res, a);
        a = mul(a, a), b >>= 1;
    }
    return res;
}
Solve ax+by+c = 0
#define double long double
const double EPS = 1e-9;
pair<complex<double>, complex<double>> solveQuadratic(double a, double
b, double c) {
    complex<double> discriminant = b * b - 4.0
    * a * c;
    complex<double> sqrt_discriminant = sqrt(discriminant);
    complex<double> root1 = (-b + sqrt_discriminant) / (2.0 * a);
    complex<double> root2 = (-b - sqrt_discriminant) / (2.0 * a);
    return { root1, root2 };
}
```

## countPairsWithGCDX

```cpp
const int N = 1e6 + 6;
int cnt[N], dp[N];
int countPairsWithGCDX(int x, vector<int>& a) {
    int mx = 0;
    for (int& i : a) {
        mx = max(i, mx);
        cnt[i]++;
    }
    for (int i = mx; i; --i) {
        int mul = 0;
        for (int j = i; j <= mx; j += i) {
            mul += cnt[j];
        }
        dp[i] = mul * (mul - 1) / 2;
        for (int j = i + i; j <= mx; j += i)
        dp[i] -= dp[j];
    }
    return dp[1];
}
```

## GCD & LCM

```cpp
int gcd(int a, int b) {
    if (b == 0) return a;
    return gcd(b, a % b);
}
int lcm(int a, int b) { return a * (b / gcd(a, b)); }
```

## DSU

```cpp
struct DSU {
    int n, cnt;
    vector<int> p, sz;
    DSU(int n) : n(n), cnt(n), p(n + 1), sz(n + 1, 1)
    {
        iota(p.begin(), p.end(), 0);
    }
    int find(int u) {
        if (u == p[u]) return u;
        return p[u] = find(p[u]);
    }
    bool same(int u, int v) {
        return find(u) == find(v);
    }
    bool merge(int u, int v) {
        int uP = find(u), vP = find(v);
        if (vP == uP) return false;
        if (sz[vP] > sz[uP]) swap(uP, vP);
        sz[uP] += sz[vP], p[vP] = uP, cnt--;
        return true;
    }
};
```

## Bridges

```cpp
struct Kabary {
    vector <vector<pair<int, int>>> adj;
    vector <set<int>> compressed_adj;
    vector <int> vis, low, depth, is_bridge, node_id;
    int cur_id = 1, n, m;
    Kabary(int _n, int _m) {
        n = _n, m = _m;
        adj.resize(n + 1);
        compressed_adj.resize(n + 1);
        vis.resize(n + 1);
        low.resize(n + 1, INT_MAX);
        depth.resize(n + 1, 0);
        is_bridge.resize(m + 1);
        node_id.resize(n + 1);
    }
    void add_edge(int u, int v, int idx) {
        adj[u].emplace_back(v, idx);
        adj[v].emplace_back(u, idx);
    }
    void find_bridges(int u, int p) {
        vis[u] = 1;
        for (auto& [v, idx] : adj[u]) {
            if (v == p) continue;
            if (vis[v]) {
                low[u] = min(low[u], depth[v]);
                continue;
            } depth[v] = 1 + depth[u];
            find_bridges(v, u);
            low[u] = min(low[u], low[v]);
            if (low[v] > depth[u]) is_bridge[idx] = 1;
        }
    }
    void dfs(int u, int p) {
        vis[u] = 1;
        node_id[u] = cur_id;
        for (auto& [v, idx] : adj[u]) {
            if (vis[v] or is_bridge[idx] or v == p)
            continue;
            dfs(v, u);
        }
    }
    void rebuild_graph() {
        vis.assign(n + 1, 0);
        for (int i = 1; i <= n; i++) {
            if (!vis[i]) {
                dfs(i, i);
                cur_id++;
            }
        }
        for (int i = 1; i <= n; i++) {
            for (auto& [v, idx] : adj[i]) {
                compressed_adj[node_id[v]].insert(node_id[i]
                );
            }
        }
    }
    void go() { find_bridges(1, 1);
        rebuild_graph(); }
};
```

## LCA (Euler)

```cpp
struct LCA {
    int n;
    vector<pair<int, int>> tour;
    vector<int> depth, tin, par;
    SparseTable<pair<int, int>> sp;
    LCA(int sz) {
        n = sz + 1;
        tin.resize(n);
        depth.resize(n);
        par.resize(n);
        tour.reserve(n * 2);
        sp = SparseTable<pair<int, int>>(2 * n);
    }
    void dfs(int u, int p, int d, auto& adj) {
        tin[u] = (int)tour.size();
        depth[u] = d;
        par[u] = p;
        tour.emplace_back(d, u);
        for (int v : adj[u]) {
            if (v == p) continue;
            dfs(v, u, d + 1, adj);
            tour.emplace_back(d, u);
        }
    }
    void build(auto& adj) {
        dfs(1, 0, 0, adj);
        sp.build(tour);
    }
    int get(int u, int v) {
        return sp.query(min(tin[u], tin[v]), max(tin[u], tin[v])).second;
    }
    int dist(int u, int v) {
        return depth[u] + depth[v] - 2 * depth[get(u, v)];
    }
};
```

## LCA

```cpp
vector<vector<int>> ancestors;
vector<int> in, out, depth;
int timer = 0;
void dfs(int u, int p) {
    in[u] = timer++, ancestors[u][0] = p, depth[u] = depth[p] + 1;
    for (int i = 1; i < LOG; i++) {
        int v = ancestors[u][i - 1];
        ancestors[u][i] = ancestors[v][i - 1];
    }
    for (int v : adj[u]) {
        if (v != p) dfs(v, u);
    } out[u] = timer - 1;
}
int findKth(int u, int k) {
    for (int i = LOG - 1; i >= 0; i--) {
        if (k & (1 << i)) {
            u = ancestors[u][i];
        }
    } return u;
}
int getLCA(int u, int v) {
    if (depth[u] < depth[v])
    swap(u, v);
    u = findKth(u, depth[u] - depth[v]);
    if (u == v) return u;
    for (int i = LOG - 1; i >= 0; i--) {
        if (ancestors[u][i] == ancestors[v][i])
        continue;
        u = ancestors[u][i];
        v = ancestors[v][i];
    } return ancestors[u][0];
}
int getDistance(int u, int v) {
    int lca = getLCA(u, v);
    return (depth[u] + depth[v]) - (2 * depth[lca]);
}
```

## Seg Tree

```cpp
struct Node
{
    int val;
    Node() {}
    Node(int x) {}
    void change(int x) {}
};
struct SegTree
{
    int tree_size;
    vector<Node> SegData;
    SegTree(int n)
    {
        tree_size = 1;
        while (tree_size < n) tree_size *= 2;
        SegData.assign(2 * tree_size, Node());
    }
    Node merge(Node& lf, Node& ri)
    {
        Node ret = Node();
        return ret;
    }
    void init(vector<int>& arr, int ni, int lx, int rx)
    {
        if (rx - lx == 1)
        {
            if (lx < arr.size())SegData[ni] = Node(arr[lx]);
            return;
        }
        int mid = (rx + lx) / 2;
        init(arr, 2 * ni + 1, lx, mid);
        init(arr, 2 * ni + 2, mid, rx);
        SegData[ni] = merge(SegData[2 * ni + 1], SegData[2 * ni + 2]);
    }
    void init(vector<int>& arr) { init(arr, 0, 0, tree_size); }
    void set(int idx, int val, int ni, int lx, int rx)
    {
        if (rx - lx == 1)
        {
            SegData[ni].change(val); return;
        }
        int mid = (rx + lx) / 2;
        if (idx < mid)
        {
            set(idx, val, 2 * ni + 1, lx, mid);
        }
        else
        {
            set(idx, val, 2 * ni + 2, mid, rx);
        }
        SegData[ni] = merge(SegData[2 * ni + 1], SegData[2 * ni + 2]);
    }
    void set(int idx, int val)
    {
        set(idx, val, 0, 0, tree_size);
    }
    Node get(int l, int r, int ni, int lx, int rx)
    {
        if (lx >= l && rx <= r) return SegData[ni];
        if (lx >= r || rx <= l) return Node();
        int mid = (rx + lx) / 2;
        Node lf = get(l, r, 2 * ni + 1, lx, mid);
        Node ri = get(l, r, 2 * ni + 2, mid, rx);
        return merge(lf, ri);
    }
    Node get(int l, int r) { return get(l, r, 0, 0, tree_size); }
};
```

## Seg Tree Lazy

```cpp
struct Node
{
    int val; int lazy = 0; bool isLazy = 0;
    Node() {}
    Node(int x) {}
    void update(int x, int lf, int ri) {}
};
struct SegTree
{
    int tree_size;
    vector<Node> SegData;
    SegTree(int n)
    {
        tree_size = 1;
        while (tree_size < n) tree_size *= 2;
        SegData.assign(2 * tree_size, Node());
    }
    Node merge(Node& lf, Node& ri)
    {
        Node ret = Node();
        return ret;
    }
    void init(vector<int>& arr, int ni, int lx, int rx)
    {
        if (rx - lx == 1)
        {
            if (lx < arr.size())
            SegData[ni] = Node(arr[lx]);
            return;
        }
        int mid = (rx + lx) / 2;
        init(arr, 2 * ni + 1, lx, mid);
        init(arr, 2 * ni + 2, mid, rx);
        SegData[ni] = merge(SegData[2 * ni + 1], SegData[2 * ni + 2]);
    }
    void init(vector<int>& arr)
    {
        init(arr, 0, 0, tree_size);
    }
    void prop(int ni, int lx, int rx) {
        if (rx - lx == 1 || (!SegData[ni].isLazy))
        return;
        int mid = (rx + lx) / 2;
        int ln = ni * 2 + 1, rn = ni * 2 + 2;
        SegData[ln].update(SegData[ni].lazy, lx, mid);
        SegData[rn].update(SegData[ni].lazy, mid, rx);
        // end lazy
    }
    void update(int l, int r, int val, int ni, int lx, int
    rx) {
        prop(ni, lx, rx);
        if (lx >= r || rx <= l)return;
        if (lx >= l && rx <= r) {
            SegData[ni].update(val, lx, rx);
            return;
        }
        int mid = (rx + lx) / 2;
        int ln = ni * 2 + 1, rn = ni * 2 + 2;
        update(l, r, val, ln, lx, mid);
        update(l, r, val, rn, mid, rx);
        SegData[ni] = merge(SegData[ln], SegData[rn]);
    }
    void update(int l, int r, int val) {
        update(l, r, val, 0, 0, tree_size);
    }
    Node get(int l, int r, int ni, int lx, int rx)
    {
        prop(ni, lx, rx);
        if (lx >= l && rx <= r)
        return SegData[ni];
        if (lx >= r || rx <= l)
        return Node();
        int mid = (rx + lx) / 2;
        Node lf = get(l, r, 2 * ni + 1, lx, mid);
        Node ri = get(l, r, 2 * ni + 2, mid, rx);
        return merge(lf, ri);
    }
    Node get(int l, int r)
    {
        return get(l, r, 0, 0, tree_size);
    }
};
```

## 2D Seg Tree

```cpp
struct Node {
    int ign = 0, val;
    Node() : val(ign) {}
    Node(int x) : val(x) {}
    Node operator+(Node& r) {
        Node res = Node();
        res.val = val + r.val;
        return res;
    }
};
#define lNode (x * 2 + 1)
#define rNode (x * 2 + 2)
#define md (lx + (rx - lx) / 2)
struct Sagara {
    int n;
    vector<Node> node;
    Sagara(int sz) {
        n = 1;
        while (n < sz) n *= 2;
        node.assign(n * 2, Node());
    }
    void set(int& ind, int& val, int x, int lx, int rx) {
        if (rx - lx == 1) {
            node[x] = Node(val);
            return;
        }
        if (ind < md) set(ind, val, lNode, lx, md);
        else set(ind, val, rNode, md, rx);
        node[x] = node[lNode] + node[rNode];
    }
    void set(int ind, int val) { set(ind, val, 0, 0, n);
    }
    Node get(int& i) { return node[n + i - 1]; }
    Node get(int& i) {
        return node[n + i - 1];
    }
    Node query(int& l, int& r, int x, int lx, int rx) {
        if (lx >= r || rx <= l) return Node();
        if (rx <= r && lx >= l) return node[x];
        Node L = query(l, r, lNode, lx, md);
        Node R = query(l, r, rNode, md, rx);
        return L + R;
    }
    Node query(int l, int r) { return query(l, r, 0, 0, n); }
};
struct Sagara2D {
    vector<Sagara> node;
    int n;
    Sagara2D(int _n, int _m) {
        n = 1;
        while (n < _n) n <<= 1;
        node.assign(n * 2, Sagara(_m));
    }
    void set(int i, int j, int val, int x, int lx, int rx) {
        if (rx - lx == 1)
        return node[x].set(j, val);
        if (i < md) set(i, j, val, lNode, lx, md);
        else set(i, j, val, rNode, md, rx);
        Node L = node[lNode].get(j);
        Node R = node[rNode].get(j);
        node[x].set(j, (L + R).val);
    }
    void set(int i, int j, int val) { set(i, j, val, 0, 0, n); }
    int query(int t, int b, int l, int r, int x, int lx, int
    rx) {
        if (rx <= t || lx >= b) return 0;
        if (lx >= t && rx <= b) return
        node[x].query(l, r).val;
        int L = query(t, b, l, r, lNode, lx, md);
        int R = query(t, b, l, r, rNode, md, rx);
        return L + R;
    }
    // tree.query(--x1 , x2 , --y1 , y2)
    int query(int t, int b, int r, int l) {
        return query(t, b, r, l, 0, 0, n);
    }
};
```

## BIT

```cpp
struct BIT {
    vector<int> Tree;
    int n;
    BIT(int _n) {
        n = _n + 1;
        Tree.assign(n, 0);
    }
    int merge(int& a, int& b) { return a + b; }
    void update(int idx, int val) {
        for (; idx < n; idx = idx | (idx + 1))
        Tree[idx] = merge(Tree[idx], val);
    }
    void update(int l, int r, int val) {
        update(l, val);
        update(r + 1, -val);
    }
    int query(int r) {
        int ret = 0;
        for (; r >= 0; r = (r & (r + 1)) - 1)
        ret = merge(ret, Tree[r]);
        return ret;
    }
    int query(int l, int r) {
        return query(r) - query(l - 1);
    }
};
```

## SpareTable

```cpp
struct SpareTable {
    int IGN = LLONG_MAX;
    vector <vector<int>> SparseTable;
    int merge(int a, int b);
    void Build(const vector <int>& arr) {
        int n = arr.size();
        SparseTable = vector
        <vector<int>>(__lg(n) + 1, vector<int> (n));
        SparseTable[0] = arr;
        for (int msk = 1; (1 << msk) <= n; msk++) {
            for (int i = 0; i + (1 << msk) <= n; i++) {
                SparseTable[msk][i] = merge( SparseTable[msk - 1][i], SparseTable[msk - 1][i + (1 << msk) / 2]
                );
            }
        }
    }
    int query(int l, int r) {
        int len = r - l + 1;
        int res = IGN;
        for (int msk = 0; l <= r; msk++) {
            if ((len >> msk) & 1) {
                res = merge(res, SparseTable[msk][l]);
                l += (1 << msk);
            }
        }
        return res;
    }
    int queryO1(int l, int r) {
        int msk = __lg(r - l + 1);
        return merge(SparseTable[msk][l], SparseTable[msk][r - (1 << msk) + 1]);
    }
};
```

## SQRT

```cpp
struct SqrtDecomposition {
    int n, blockSize;
    vector<long long> arr, blocks;
    SqrtDecomposition(int size) {
        n = size;
        blockSize = sqrt(n) + 1; // size of each
        block
        arr.assign(n, 0);
        blocks.assign(blockSize, 0);
    }
    // Build from initial array
    void build(vector<long long>& input) {
        arr = input;
        for (int i = 0; i < n; i++)
        blocks[i / blockSize] += arr[i];
    }
    // Point update: arr[idx] = val
    void update(int idx, long long val) {
        blocks[idx / blockSize] += (val - arr[idx]);
        arr[idx] = val;
    }
    // Range query: sum from l to r
    long long query(int l, int r) {
        long long sum = 0;
        int startBlock = l / blockSize;
        int endBlock = r / blockSize;
        if (startBlock == endBlock) {
            for (int i = l; i <= r; i++) sum += arr[i];
        }
        else {
            int endOfStartBlock = (startBlock + 1)
            * blockSize - 1;
            for (int i = l; i <= endOfStartBlock; i++)
            sum += arr[i];
            for (int b = startBlock + 1; b < endBlock; b++) sum += blocks[b];
            for (int i = endBlock * blockSize; i <= r;
            i++) sum += arr[i];
        }
        return sum;
    }
};
MOOOO
int SQ, res = 0; // Set it to ceil(sqrt(n));
const int N = 2e5 + 10;
int arr[N], ans[N], n, q;
struct Query {
    int l, r, idx;
    bool operator<(Query& other) {
        if (l / SQ != other.l / SQ) return l / SQ < other.l / SQ;
        return (l / SQ & 1 ? r < other.r : r > other.r);
    }
};
vector <Query> queries; // Resizeeeeeeee
void add(int idx);
void del(int idx);
int query();
void calc() {
    sort(queries.begin(), queries.end());
    int l = 0, r = -1;
    for (auto& [lq, rq, idx] : queries) {
        while (l > lq) add(--l);
        while (r < rq) add(++r);
        while (l < lq) del(l++);
        while (r > rq) del(r--);
        ans[idx] = query();
    }
}
```

## Merge Sort Tree

```cpp
struct Node {
    vector<int> v;
    Node() {};
    Node(int x) : v({ x }) {};
};
#define lNode (x * 2 + 1)
#define rNode (x * 2 + 2)
#define md (lx + (rx - lx) / 2)
struct MergeSortSagara {
    vector<Node> node;
    int n;
    MergeSortSagara(int _n) {
        n = 1;
        while (n < _n) n <<= 1;
        node.assign(n * 2, Node());
    }
    Node merge(Node& l, Node& r) {
        Node res;
        int i = 0, j = 0;
        while (i < l.v.size() && j < r.v.size()) {
            if (l.v[i] < r.v[j]) res.v.push_back(l.v[i++]);
            else res.v.push_back(r.v[j++]);
        }
        while (i < l.v.size())
        res.v.push_back(l.v[i++]);
        while (j < r.v.size())
        res.v.push_back(r.v[j++]);
        return res;
    }
    void build(vector<int>& v, int x, int lx, int rx) {
        if (rx - lx == 1) {
            if (lx < v.size()) node[x] = Node(v[lx]);
            return;
        }
        build(v, lNode, lx, md);
        build(v, rNode, md, rx);
        node[x] = merge(node[lNode], node[rNode]);
    }
    void build(vector<int>& v) { build(v, 0, 0, n); }
    int query(int l, int r, int x, int lx, int rx, int val)
    {
        if (rx <= l || lx >= r) return 0;
        if (lx >= l && rx <= r) return calc(node[x], val);
        return query(l, r, lNode, lx, md, val) + query(l, r, rNode, md, rx, val);
    }
    int query(int l, int r, int val) {
        return query(l, r, 0, 0, n, val);
    }
    // change this function to match your need
    int calc(Node& no, int val) { return
        greater_than(no, val); }
    int less_than(Node& no, int val) {
        return lower_bound(no.v.begin(), no.v.end(), val) - no.v.begin();
    }
    int greater_than(Node& no, int val) {
        return no.v.size() - less_than(no, val) - equal(no, val);
    }
    int equal(Node& no, int val) {
        return upper_bound(no.v.begin(), no.v.end(), val) - lower_bound(no.v.begin(), no.v.end(), val);
    }
    int kth(int l, int r, int k) {
        int low, high = 1e9, ans = -1;
        while (low <= high) {
            int mid = (low + high) / 2;
            int cnt = query(l, r, mid);
            if (cnt >= k) {
                ans = mid;
                high = mid - 1;
            }
            else {
                low = mid + 1;
            } }
        return ans;
    }
};
```

## Ordered Set

```cpp
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp> using namespace __gnu_pbds;
typedef tree<int, null_type, greater_equal<int>, rb_tree_tag, tree_order_statistics_node_update> ordered_multiset;
void Erase(ordered_multiset& s, int val) {
    int place = s.order_of_key(val);
    auto it = s.find_by_order(place);
    s.erase(it);}
```

## XOR Basis

```cpp
struct Basis {
    int size = 0, n = 0;
    int basis[lg];
    Basis() {
        size = n = 0;
        for (int i = lg - 1; i >= 0; --i) basis[i] = 0;
    }
    bool insert(int x) {
        n++;
        for (int i = lg - 1; i >= 0; --i) {
            if (((x >> i) & 1) == 0) continue;
            if (not basis[i]) {
                basis[i] = x, ++size;
                return true;
            }
            x = (x ^ basis[i]);
        }
        return false;
    }
    bool merge(Basis& w) {
        bool repeat = false;
        for (int i = 0; i < lg; ++i)
        if (w.basis[i] > 0 and not
        insert(w.basis[i]))
        repeat = true;
        return repeat;
    }
    bool can(int x) { // if n > size then you can get
        x = 0
        for (int i = lg - 1; i >= 0; --i)
        if (basis[i] and (x & (1LL << i))) x = (x ^ basis[i]);
        return x == 0;
    }
    int count_xors(int x) { // NOTE: Add
        exponentiation
        return (can(x) ? (exp(2, n - size) + mod - 1)
        % mod :0);
    }
    int kth(int k) {
        int x = 0;
        for (int i = lg - 1, c = size; i >= 0; --i) {
            if (not basis[i]) continue; --c;
            if (x & (1LL << i)) {
                if ((1LL << c) >= k) x = (x ^ basis[i]);
                else k = k - (1LL << c);
            }
            else if (k > (1LL << c)) {
                x = (x ^ basis[i]), k = k - (1LL << c);
            }
        }
        return x;
    }
    int get_max() {
        int ans = 0;
        for (int i = lg - 1; i >= 0; --i) {
            if (basis[i] && not(ans & (1LL << i))) ans = (ans
            ^ basis[i]);
        }
        return ans;
    }
    void AND(int x) {
        vector< int > upd;
        for (int i = lg - 1; i >= 0; --i) {
            basis[i] = (basis[i] & x);
            if (basis[i]) upd.push_back(basis[i]);
            basis[i] = 0;
        }
        for (int& val : upd) insert(val);
    }
    void OR(int x) {
        vector< int > upd;
        for (int i = lg - 1; i >= 0; --i) {
            basis[i] = (basis[i] | x);
            if (basis[i]) upd.push_back(basis[i]);
            basis[i] = 0;
        }
        for (int& val : upd) insert(val);
    }
};
```

## Hashing 1

```cpp
#define MAXLEN 1000010
constexpr uint64_t mod = (1ULL << 61) - 1;
const uint64_t seed = chrono::system_clock::now().time_since_epo
ch().count();
const uint64_t base = mt19937_64(seed)() % (mod / 3) + (mod / 3);
uint64_t base_pow[MAXLEN];
int64_t modmul(uint64_t a, uint64_t b) {
    uint64_t l1 = (uint32_t)a, h1 = a >> 32, l2 = (uint32_t)b, h2 = b >> 32;
    uint64_t l = l1 * l2, m = l1 * h2 + l2 * h1, h = h1 * h2;
    uint64_t ret = (l & mod) + (l >> 61) + (h << 3)
    + (m >> 29) + (m << 35 >> 3) + 1;
    ret = (ret & mod) + (ret >> 61);
    ret = (ret & mod) + (ret >> 61);
    return ret - 1;
}
void init() {
    base_pow[0] = 1;
    for (int i = 1; i < MAXLEN; i++) {
        base_pow[i] = modmul(base_pow[i - 1], base);
    }
}
struct MagekHash {
    /// Remove suff vector and usage if reverse
    hash is not required for more speed
    vector<int64_t> pref, suff;
    MagekHash() {}
    template <typename T> MagekHash(const vector<T>& ar) {
        if (!base_pow[0]) init();
        int n = ar.size();
        pref.resize(n + 3, 0);
        //suff.resize(n + 3, 0);
        for (int i = 1; i <= n; i++) {
            pref[i] = modmul(pref[i - 1], base) + ar[i - 1] + 997;
            if (pref[i] >= mod) pref[i] -= mod;
        }
        for (int i = n; i >= 1; i--) {
            suff[i] = modmul(suff[i + 1], base) + ar[i - 1] + 997;
            if (suff[i] >= mod) suff[i] -= mod;
        }
    }
    MagekHash(const char* str)
    : MagekHash(vector<char>(str, str + strlen(str))) {
    }
    uint64_t get_hash(int l, int r) {
        int64_t h = pref[r + 1] - modmul(base_pow[r - l + 1], pref[l]);
        return h < 0 ? h + mod : h;
    }
    uint64_t rev_hash(int l, int r) {
        int64_t h = suff[l + 1] - modmul(base_pow[r - l + 1], suff[r + 2]);
        return h < 0 ? h + mod : h;
    }};
```

## Hashing 2

```cpp
const int M = 2, N = 1e6 + 5;
int B[M], mods[M], pw[M][N];
struct Hash {
    vector<array<int, M> > prefix, suffix;
    Hash() {
        if (B[0])return;
        mt19937
        rng(chrono::system_clock::now().time_since_
        epoch().count());
        auto rnd = [&](int a, int b) {
            return a + rng() % (b - a + 1);
        };
        auto check = [&](int x) {
            for (int i = 2; i * i <= x; ++i)if (x % i == 0)return true;
            return false;
        };
        for (int i = 0; i < M; ++i) {
            B[i] = rnd(130, 500);
            mods[i] = rnd(1e9, 2e9);
            while (check(mods[i]))mods[i]--;
            pw[i][0] = 1;
            for (int j = 1; j < N; ++j)
            pw[i][j] = pw[i][j - 1] * B[i] % mods[i];
        }
    }
    void build(string& s) { Init(s, 1); }
    void Init(string& s, bool withSuffix = false) {
        InitPrefix(s);
        if (withSuffix)
        InitSuffix(s);
    }
    void InitPrefix(string& s) {
        int n = (int)s.size();
        prefix.assign(n, {});
        array<int, M> hash{};
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < M; ++j) {
                hash[j] = (hash[j] * B[j]) % mods[j];
                hash[j] = (hash[j] + s[i]) % mods[j];
            }
            prefix[i] = hash;
        }
    }
    void InitSuffix(string& s) {
        int n = (int)s.size();
        suffix.assign(n, {});
        array<int, M> hash{};
        for (int i = n - 1; i >= 0; --i) {
            for (int j = 0; j < M; ++j) {
                hash[j] = (hash[j] * B[j]) % mods[j];
                hash[j] = (hash[j] + s[i]) % mods[j];
            }
            suffix[i] = hash;
        }
    }
    int get_hash(int l, int r) {
        if (l == 0)return prefix[r][0] * prefix[r][1];
        array<int, M> res = prefix[r];
        for (int i = 0; i < M; ++i) {
            res[i] -= prefix[l - 1][i] * pw[i][r - l + 1] % mods[i];
            if (res[i] < 0) res[i] += mods[i];
        }
        return res[0] * res[1];
    }
};
```

## KMP

```cpp
int fail[N];
void KMP(string& s)
{
    int n = s.size();
    for (int i = 1; i < n; i++)
    {
        int j = fail[i - 1];
        while (j > 0 && s[i] != s[j]) j = fail[j - 1];
        if (s[i] == s[j]) j++;
        fail[i] = j;
    }
}
```

## HLD

```cpp
vector<vector<int>> adj;
vector<int> par, depth, in, sz, big, head, vals;
int timer = 1;
void dfs(int u, int p, int d = 0) {
    par[u] = p, depth[u] = d, sz[u] = 1;
    int mx = 0;
    for (int &v : adj[u]) {
        if (v == p) continue;
        dfs(v, u, d + 1);
        if (sz[v] > mx) big[u] = v, mx = sz[v];
        sz[u] += sz[v];
    }
}
void flatten(int u, int hd) {
    head[u] = hd , in[u] = timer++;
    if (big[u])
    flatten(big[u], hd);
    for (int &v : adj[u])
    if (v != par[u] && v != big[u])
    flatten(v, v);
}
void init(int n) {
    timer = 1;
    adj.assign(n + 1, {});
    par.assign(n + 1, {});
    depth.assign(n + 1, {});
    in.assign(n + 1, {});
    sz.assign(n + 1, {});
    big.assign(n + 1, {});
    head.assign(n + 1, {});
    vals.assign(n + 1, {});
}
void HLD() {
    dfs(1 , 1 , 0);
    flatten(1 , 1);
}
int query(int l, int r) {
    return 0;
}
int getPath(int u, int v) {
    int a = head[u], b = head[v];
    int res = 0;
    while (a != b) {
        if (depth[a] < depth[b]) swap(a, b), swap(u, v);
        res = res + query(in[a], in[u]);
        u = par[a], a = head[u];
    }
    if (in[u] > in[v]) swap(u, v);
    res = res + query(in[u], in[v]);
    return res;
}
vector <pair<int,int>> getRanges(int u, int v) {
    vector <pair<int,int>> ret;
    int a = head[u], b = head[v];
    int res = 0;
    while (a != b) {
        if (depth[a] < depth[b]) swap(a, b), swap(u, v);
        ret.emplace_back(in[a], in[u]);
        u = par[a], a = head[u];
    }
    if (in[u] > in[v]) swap(u, v);
    ret.emplace_back(in[u], in[v]);
    return ret;
}
int getSubtree(int u) {
    return query(in[u], in[u] + sz[u] - 1);
}
void updateNode(int u, int value) {
    // update pos[u] in the structure
}
```

## DSU on Tree

```cpp
vector <vector<int>> adj;
vector <int> ans , big , sz;
void init(int n) {
    adj.resize(n + 1 , {});
    big.assign(n + 1 , 0);
    sz.assign(n + 1 , 0);
}
void pre(int u , int p) {
    sz[u] = 1;
    for (int v : adj[u]) {
        if (v == p) continue;
        pre(v , u);
        sz[u] += sz[v];
        if (!big[u] or sz[v] > sz[big[u]]) {
            big[u] = v;
        }
    }
}
void update(int u , int add) {}
void collect(int u , int p , int add) {
    update(u , add);
    for (int v : adj[u]) {
        if (v == p) continue;
        collect(v , u , add);
    }
}
void dfs(int u , int p , bool keep) {
    for (int v : adj[u]) {
        if (v == p or v == big[u]) continue;
        dfs(v , u , 0);
    }
    if (big[u] != 0)
    dfs(big[u] , u , 1);
    update(u , 1);
    for (int v : adj[u]) {
        if (v == p or v == big[u]) continue;
        collect(v , u , 1);
    }
    // solve
    if (!keep) {
        collect(u , p , 0);}}
```
## Trie

```cpp
struct Trie {

    struct Node {
        int nxt[26];
        int pass = 0;
        int end = 0;

        Node() {
            memset(nxt, 0, sizeof nxt);
        }
    };

    vector<Node> tr;
    int distinct_words = 0;

    Trie() {
        tr.reserve(200000);
        tr.push_back(Node());
    } // init

    int add_node() {
        tr.push_back(Node());
        return tr.size() - 1;
    } // create node

    void insert(const string& s) {
        int u = 0;
        for(char c : s) {
            int v = c - 'a';
            if(!tr[u].nxt[v])
                tr[u].nxt[v] = add_node();
            u = tr[u].nxt[v];
            tr[u].pass++;
        }
        if(tr[u].end == 0)
            distinct_words++;
        tr[u].end++;
    } // insert word

    bool search(const string& s) {
        int u = 0;
        for(char c : s) {
            int v = c - 'a';
            if(!tr[u].nxt[v]) return false;
            u = tr[u].nxt[v];
        }
        return tr[u].end > 0;
    } // exact match

    int count_word(const string& s) {
        int u = 0;
        for(char c : s) {
            int v = c - 'a';
            if(!tr[u].nxt[v]) return 0;
            u = tr[u].nxt[v];
        }
        return tr[u].end;
    } // occurrences of word

    int count_pref(const string& s) {
        int u = 0;
        for(char c : s) {
            int v = c - 'a';
            if(!tr[u].nxt[v]) return 0;
            u = tr[u].nxt[v];
        }
        return tr[u].pass;
    } // words with prefix

    bool starts_with(const string& s) {
        int u = 0;
        for(char c : s) {
            int v = c - 'a';
            if(!tr[u].nxt[v]) return false;
            u = tr[u].nxt[v];
        }
        return true;
    } // prefix exists

    bool erase(const string& s) {
        if(!search(s)) return false;

        int u = 0;
        vector<int> path = {0};

        for(char c : s) {
            int v = c - 'a';
            u = tr[u].nxt[v];
            path.push_back(u);
        }

        tr[u].end--;
        if(tr[u].end == 0)
            distinct_words--;

        for(int i = 1; i < path.size(); i++)
            tr[path[i]].pass--;

        return true;
    } // remove one occurrence

    string kth_word(int k) {
        string res = "";
        int u = 0;

        while(true) {
            if(tr[u].end >= k)
                return res;

            k -= tr[u].end;

            for(int i = 0; i < 26; i++) {
                int v = tr[u].nxt[i];
                if(v) {
                    if(tr[v].pass >= k) {
                        res += char('a' + i);
                        u = v;
                        break;
                    } else {
                        k -= tr[v].pass;
                    }
                }
            }
        }
    } // k-th lexicographical word (1-based)

    int size() {
        return tr.size();
    } // number of nodes

    void clear() {
        tr.clear();
        tr.push_back(Node());
        distinct_words = 0;
    } // reset for new test case
};
```

## BTrie

```cpp
struct BinaryTrie {
    static const int MAX_BIT = 62; // adjust for the max number of bits needed

    struct Node {
        int nxt[2]; // next nodes for bit 0 and 1
        int cnt;    // how many numbers pass through this node
        int end;    // how many numbers end at this node
        Node() { nxt[0] = nxt[1] = 0; cnt = end = 0; }
    };

    vector<Node> tr;

    BinaryTrie(int reserve_nodes = 0) { if(reserve_nodes) tr.reserve(reserve_nodes); tr.push_back(Node()); } // constructor

    int add_node() { tr.push_back(Node()); return (int)tr.size() - 1; } // create new node

    int size() const { return tr[0].cnt; } // total numbers
    int nodes() const { return (int)tr.size(); } // total nodes
    bool empty() const { return tr[0].cnt == 0; } // is trie empty
    void clear() { tr.clear(); tr.push_back(Node()); } // reset trie

    void insert(unsigned long long x) { // insert number x
        int u = 0; tr[u].cnt++;
        for(int i = MAX_BIT; i >= 0; --i) {
            int b = (x >> i) & 1ULL;
            if(!tr[u].nxt[b]) tr[u].nxt[b] = add_node();
            u = tr[u].nxt[b]; tr[u].cnt++;
        }
        tr[u].end++;
    }

    int count_exact(unsigned long long x) const { // occurrences of x
        int u = 0;
        for(int i = MAX_BIT; i >= 0; --i) {
            int b = (x >> i) & 1ULL;
            if(!tr[u].nxt[b]) return 0;
            u = tr[u].nxt[b];
        }
        return tr[u].end;
    }

    bool contains(unsigned long long x) const { return count_exact(x) > 0; } // check if x exists

    bool erase(unsigned long long x) { // remove one occurrence of x
        if(count_exact(x) == 0) return false;
        int u = 0; tr[u].cnt--;
        for(int i = MAX_BIT; i >= 0; --i) {
            int b = (x >> i) & 1ULL;
            u = tr[u].nxt[b]; tr[u].cnt--;
        }
        tr[u].end--; return true;
    }

    unsigned long long max_xor_value(unsigned long long x) const { // max XOR with x
        if(empty()) return 0;
        int u = 0; unsigned long long res = 0;
        for(int i = MAX_BIT; i >= 0; --i) {
            int b = (x >> i) & 1ULL, want = b ^ 1;
            if(tr[u].nxt[want] && tr[tr[u].nxt[want]].cnt > 0) { res |= (1ULL << i); u = tr[u].nxt[want]; }
            else u = tr[u].nxt[b];
        }
        return res;
    }

    unsigned long long min_xor_value(unsigned long long x) const { // min XOR with x
        if(empty()) return 0;
        int u = 0; unsigned long long res = 0;
        for(int i = MAX_BIT; i >= 0; --i) {
            int b = (x >> i) & 1ULL;
            if(tr[u].nxt[b] && tr[tr[u].nxt[b]].cnt > 0) u = tr[u].nxt[b];
            else { res |= (1ULL << i); u = tr[u].nxt[b ^ 1]; }
        }
        return res;
    }

    unsigned long long max_xor_number(unsigned long long x) const { // number giving max XOR with x
        if(empty()) return ULLONG_MAX;
        int u = 0; unsigned long long y = 0;
        for(int i = MAX_BIT; i >= 0; --i) {
            int b = (x >> i) & 1ULL, want = b ^ 1;
            if(tr[u].nxt[want] && tr[tr[u].nxt[want]].cnt > 0) { y |= (1ULL << i); u = tr[u].nxt[want]; }
            else { y |= (unsigned long long)b << i; u = tr[u].nxt[b]; }
        }
        return y;
    }

    unsigned long long min_xor_number(unsigned long long x) const { // number giving min XOR with x
        if(empty()) return ULLONG_MAX;
        int u = 0; unsigned long long y = 0;
        for(int i = MAX_BIT; i >= 0; --i) {
            int b = (x >> i) & 1ULL;
            if(tr[u].nxt[b] && tr[tr[u].nxt[b]].cnt > 0) { y |= (unsigned long long)b << i; u = tr[u].nxt[b]; }
            else { y |= (unsigned long long)(b ^ 1) << i; u = tr[u].nxt[b ^ 1]; }
        }
        return y;
    }

    unsigned long long kth_smallest(long long k) const { // kth smallest number
        if(k <= 0 || k > tr[0].cnt) return ULLONG_MAX;
        int u = 0; unsigned long long res = 0;
        for(int i = MAX_BIT; i >= 0; --i) {
            int left = tr[u].nxt[0], left_cnt = left ? tr[left].cnt : 0;
            if(k <= left_cnt) u = left;
            else { k -= left_cnt; res |= (1ULL << i); u = tr[u].nxt[1]; }
        }
        return res;
    }

    long long count_less_than(unsigned long long x) const { // count numbers < x
        long long ans = 0; int u = 0;
        for(int i = MAX_BIT; i >= 0; --i) {
            int b = (x >> i) & 1ULL;
            if(b == 1) { int left = tr[u].nxt[0]; if(left) ans += tr[left].cnt; u = tr[u].nxt[1]; }
            else u = tr[u].nxt[0];
            if(!u) break;
        }
        return ans;
    }

    long long count_xor_less_than(unsigned long long x, unsigned long long k) const { // count numbers y: (x^y) < k
        long long ans = 0; int u = 0;
        for(int i = MAX_BIT; i >= 0; --i) {
            int xb = (x >> i) & 1ULL, kb = (k >> i) & 1ULL;
            if(!u) break;
            if(kb == 1) { int same = tr[u].nxt[xb]; if(same) ans += tr[same].cnt; u = tr[u].nxt[xb ^ 1]; }
            else u = tr[u].nxt[xb];
        }
        return ans;
    }

    unsigned long long lower_bound_value(unsigned long long x) const { // first number >= x
        long long rk = count_less_than(x) + 1;
        if(rk > tr[0].cnt) return ULLONG_MAX;
        return kth_smallest(rk);
    }

    unsigned long long upper_bound_value(unsigned long long x) const { // first number > x
        if(x == ULLONG_MAX) return ULLONG_MAX;
        long long rk = count_less_than(x + 1) + 1;
        if(rk > tr[0].cnt) return ULLONG_MAX;
        return kth_smallest(rk);
    }
};
```
