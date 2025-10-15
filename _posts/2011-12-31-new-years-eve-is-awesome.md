# 排错问题

错排数 *D*(*n*)表示 *n*个元素的排列中，**没有任何一个元素出现在其原始位置**的排列方式的总数。例如：

- *n*=2：`[2, 1]`是唯一的错排，故 *D*(2)=1。
- *n*=3：`[2, 3, 1]`和 `[3, 1, 2]`是错排，故 *D*(3)=2。

**错排数的递推公式**

错排数可以通过以下递推关系计算：

```
D(n)=(n−1)×(D(n−1)+D(n−2))
```

**初始条件**：

```
D(0)=1（空排列算作一种错排）	关键，确保递推正确性
D(1)=0（单个元素无法错排）
```

## 将 n封信装入 n个信封，求全部装错的概率

**动态规划实现**

```c++
#include <iostream>
#include <vector>

int derangement(int n) {
    if (n == 0) return 1; // 空排列
    if (n == 1) return 0;
    
    std::vector<int> dp(n + 1);
    dp[0] = 1;
    dp[1] = 0;
    
    for (int i = 2; i <= n; ++i) {
        dp[i] = (i - 1) * (dp[i - 1] + dp[i - 2]);
    }
    
    return dp[n];
}

int main() {
    std::cout << "D(3) = " << derangement(3) << std::endl; // 输出 2
    std::cout << "D(4) = " << derangement(4) << std::endl; // 输出 9
    return 0;
}
```

**数学推导（容斥原理）**

错排数也可以通过容斥原理直接计算：
$$
D(n) = n! \left( 1 - \frac{1}{1!} + \frac{1}{2!} - \cdots + (-1)^n \frac{1}{n!} \right)
$$

```c++
#include <iostream>
#include <cmath>

int factorial(int k) {
    return (k == 0) ? 1 : k * factorial(k - 1);
}

int derangement_math(int n) {
    int sum = 0;
    for (int k = 0; k <= n; ++k) {
        sum += pow(-1, k) / factorial(k);
    }
    return factorial(n) * sum;
}
```
