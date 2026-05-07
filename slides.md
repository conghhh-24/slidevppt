---
theme: default
class: 'text-lg'
transition: fade-out
---

# <span class="inline-block animate-bounce">🎯</span> 算法设计与分析
## <span class="text-blue-500">第4章 基本的算法策略</span>

<div class="absolute bottom-8 left-1/2 transform -translate-x-1/2">
  <span class="text-gray-400 text-sm animate-pulse">使用 ← → 键或点击导航</span>
</div>

---
layout: two-cols
transition: slide-up
---

## 📚 本章内容框架

| 章节 | 算法策略 |
|------|----------|
| 4.1 | **迭代算法** |
| 4.2 | 蛮力法 |
| 4.3 | 分而治之 |
| 4.4 | 贪婪算法 |
| 4.5 | 动态规划 |
| 4.6 | 算法比较 |

::right::

## 💡 核心思想速览

<div class="space-y-3 mt-8">
  <div class="p-3 bg-blue-900/30 rounded-lg border-l-4 border-blue-400 animate-fade-in">
    <strong class="text-blue-300">迭代</strong>: <span class="text-gray-200">用旧值算新值，逐步逼近</span>
  </div>
  <div class="p-3 bg-green-900/30 rounded-lg border-l-4 border-green-400 animate-fade-in delay-100">
    <strong class="text-green-300">蛮力</strong>: <span class="text-gray-200">穷举搜索所有可能</span>
  </div>
  <div class="p-3 bg-purple-900/30 rounded-lg border-l-4 border-purple-400 animate-fade-in delay-200">
    <strong class="text-purple-300">分治</strong>: <span class="text-gray-200">分解问题，递归求解</span>
  </div>
</div>

---
transition: slide-left
---

## 🔄 4.1 迭代算法（总论）

### 🎯 基本思想
用旧值逐步计算新值，通过重复操作逼近目标

### 📋 常见策略

<div class="grid grid-cols-2 gap-2 mt-3">
  <div class="px-3 py-2 text-xs bg-blue-900/50 text-blue-300 rounded border border-blue-700/50">
    <strong>累加/累乘</strong>
    <p class="text-gray-400">数值计算基础</p>
  </div>
  <div class="px-3 py-2 text-xs bg-green-900/50 text-green-300 rounded border border-green-700/50">
    <strong>递推</strong>
    <p class="text-gray-400">从初始条件正向推导</p>
  </div>
  <div class="px-3 py-2 text-xs bg-orange-900/50 text-orange-300 rounded border border-orange-700/50">
    <strong>倒推</strong>
    <p class="text-gray-400">从目标状态逆向求解</p>
  </div>
  <div class="px-3 py-2 text-xs bg-purple-900/50 text-purple-300 rounded border border-purple-700/50">
    <strong>逼近</strong>
    <p class="text-gray-400">解方程的迭代方法</p>
  </div>
</div>

### 📝 核心步骤

1. **确定迭代模型**（数学/逻辑关系）
2. **建立迭代关系式**
3. **控制迭代过程**（次数/精度条件）

---
transition: slide-right
---

## 🐰 4.1.1 递推法（Recurrence / Forward Recursion）

### 例1：兔子繁殖问题（斐波那契数列）

**问题背景**：
假设一对兔子每个月生一对小兔子，小兔子一个月后成熟，问n个月后有多少对兔子？

**数学模型**：
$
y_1 = y_2 = 1, \quad y_n = y_{n-1} + y_{n-2}
$

**数列前10项**：
<div class="flex flex-wrap gap-2 text-base">
  <span class="px-2 py-1 bg-blue-900/60 text-blue-300 rounded-full border border-blue-700/50">1</span>
  <span class="px-2 py-1 bg-blue-900/60 text-blue-300 rounded-full border border-blue-700/50">1</span>
  <span class="px-2 py-1 bg-blue-900/60 text-blue-300 rounded-full border border-blue-700/50">2</span>
  <span class="px-2 py-1 bg-blue-900/60 text-blue-300 rounded-full border border-blue-700/50">3</span>
  <span class="px-2 py-1 bg-blue-900/60 text-blue-300 rounded-full border border-blue-700/50">5</span>
  <span class="px-2 py-1 bg-blue-900/60 text-blue-300 rounded-full border border-blue-700/50">8</span>
  <span class="px-2 py-1 bg-blue-900/60 text-blue-300 rounded-full border border-blue-700/50">13</span>
  <span class="px-2 py-1 bg-blue-900/60 text-blue-300 rounded-full border border-blue-700/50">21</span>
  <span class="px-2 py-1 bg-blue-900/60 text-blue-300 rounded-full border border-blue-700/50">34</span>
  <span class="px-2 py-1 bg-blue-900/60 text-blue-300 rounded-full border border-blue-700/50">55</span>
</div>

---
transition: zoom-in
---

## 💻 算法1：标准两变量递推

### 核心思路
使用三个变量交替存储前两项和当前项，空间复杂度为 $O(1)$

```c
// Fibonacci 数列标准实现
int a=1, b=1, c, n=12;
for(int i=3; i<=n; i++) {
    c = a + b;  // 计算新值
    a = b;      // 变量移位
    b = c;
    printf("%d ", c);
}
```

### 变量变化
| 迭代 | a | b | c |
|------|---|---|---|
| 初始 | 1 | 1 | - |
| i=3 | 1 | 2 | 2 |

---
transition: slide-up
---

## ⚡ 算法2：一次循环递推3步的变种

### 优化思路
每次循环计算3项，减少循环次数约 1/3

```c
int a=1, b=1;
for(int i=1; i<=n/3; i++) {
    printf("%d %d ", a, b);
    ab = a + b;   // b = F(2i+2)
    b = a + b;   // b = F(2i+2)
    printf("%d ", a);
}
```

**复杂度**：$O(n)$，常数优化约33%
---
transition: slide-down
---

## 🎨 算法3：紧凑变量更新方式

### 设计技巧
利用赋值顺序，先更新a再更新b，使b的值依赖于新的a

```c
int a=1, b=1;
for(int i=3; i<=n; i++) {
    a = a + b;  // a = F(i)
    b = a + b;  // b = F(i+1)
    printf("%d ", a);
}
```

### 执行过程（n=6）
```
初始: a=1, b=1
i=3:   a=2, b=3   → 输出 2
i=4:   a=5, b=8   → 输出 5
i=5:   a=13, b=21 → 输出 13
```
---
layout: two-cols
transition: slide-left
---

## 📐 例2：求两个整数的最大公约数

### 欧几里得算法

**核心原理**：
$
gcd(a, b) = gcd(b, a \mod b)
$

**终止条件**：当 $b = 0$ 时，$a$ 即为最大公约数

::right::

```c
int gcd(int a, int b) {
    while(b != 0) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}
```

### 📝 执行示例（gcd(48, 18)）

| 步骤 | a | b | a % b |
|------|---|---|-------|
| 1 | 48 | 18 | 12 |
| 2 | 18 | 12 | 6 |
| 3 | 12 | 6 | 0 |
| 4 | 6 | 0 | - |

**结果**：$gcd(48, 18) = 6$

---
transition: fade-in
---

## 🔙 4.1.2 倒推法（Backward Iteration）

### 🎯 基本概念
<span class="text-xl">从目标状态出发，逆向推导初始条件</span>

### 📋 适用场景

<div class="flex flex-wrap gap-3 mt-6 justify-center">
  <div class="px-4 py-2 bg-red-900/50 text-red-300 rounded-full hover:scale-110 transition-transform cursor-pointer border border-red-700/50">
    ✅ 已知最终状态
  </div>
  <div class="px-4 py-2 bg-orange-900/50 text-orange-300 rounded-full hover:scale-110 transition-transform cursor-pointer border border-orange-700/50">
    ⚡ 正向计算复杂
  </div>
  <div class="px-4 py-2 bg-amber-900/50 text-amber-300 rounded-full hover:scale-110 transition-transform cursor-pointer border border-amber-700/50">
    🗺️ 需要追踪路径
  </div>
</div>

### 🔄 与递推法的对比

| 方法 | 方向 | 起点 | 终点 |
|------|------|------|------|
| 递推法 | 正向 | 初始条件 | 目标状态 |
| 倒推法 | 逆向 | 目标状态 | 初始条件 |

---
transition: slide-right

---

## 🍑 例1：猴子吃桃问题

**问题描述**：
猴子第一天摘若干桃，吃一半加1个。以后每天吃前一天剩下的一半零一个。第10天只剩1个桃，求第一天摘多少桃？

**倒推公式**：
$
x_n = 2(x_{n+1} + 1)
$

```c
int x = 1;
for(int i=9; i>=1; i--) {
    x = 2 * (x + 1);
}
```

### 计算过程

| 天数 | 桃子数 |
|:----:|:------:|
| 10 | 1 |
| 9 | 4 |
| 8 | 10 |
| 7 | 22 |

---
layout: image-right
image: ./assets/yanghui.png
transition: zoom-in
---

## 🔺 例2：杨辉三角形输出（1维数组）

**核心公式**：
$
A[1] = A[i] = 1, \quad A[j] = A[j-1] + A[j] \quad (j = i-1, i-2, \dots, 2)
$

```c
int A[10] = {0};
A[1] = 1;
for(int i=1; i<=n; i++) {
    A[i] = 1;
    for(int j=i-1; j>=2; j--) {
        A[j] = A[j-1] + A[j];
    }
    // 输出第i行
    for(int j=1; j<=i; j++) {
        printf("%d ", A[j]);
    }
    printf("\n");
}
```

### 🧠 设计要点

**为什么要从右向左计算？**

> 避免覆盖还未使用的数据！
> 如果从左向右计算，A[j-1]会被新值覆盖，导致后续计算错误。

### 📊 执行过程（第5行）

```
初始: [1, 1, 1, 1, 1, 0, ...]
j=4:   [1, 1, 1, 2, 1, 0, ...]
j=3:   [1, 1, 3, 2, 1, 0, ...]
j=2:   [1, 4, 3, 2, 1, 0, ...]
最终:  [1, 4, 6, 4, 1, 0, ...]
```

---
layout: image-center
image: ./assets/desert.png
transition: fade-in
---

## 🏜️ 例3：穿越沙漠问题

**问题描述**：
> 一辆吉普车要穿越1000公里的沙漠，吉普车总装载能力为500加仑，每公里耗油1加仑。起点有充足的油，但途中没有加油站。问最少需要多少油才能穿越沙漠？

### 🎯 核心策略：建立油库，分段运输

**倒推思路**：
1. 从终点倒推油库位置
2. 每个油库存储足够往返油量
3. 逐步向前扩展

### 📊 最优解分析

| 油库位置 | 存储油量 |
|----------|----------|
| 0 km | 0 |
| ~166.7 km | ~166.7 加仑 |
| ~266.7 km | 200 加仑 |
| ... | ... |
| 383.33 km | 首座油库 |

**最少总油量**：约 **3833.33 加仑**

---
transition: slide-up
---

## 🔢 4.1.3 迭代法解方程

### 🎯 基本思想
<span class="text-xl">通过迭代逐步逼近方程的根</span>

### 📋 基本步骤

<div class="flex flex-col gap-2">
  <div class="flex items-center gap-3 p-3 bg-blue-900/40 rounded-lg border border-blue-700/50">
    <span class="w-8 h-8 bg-blue-600 text-white rounded-full flex items-center justify-center">1</span>
    <span class="text-gray-200">确定迭代函数 $x = \phi(x)$</span>
  </div>
  <div class="flex items-center gap-3 p-3 bg-green-900/40 rounded-lg border border-green-700/50">
    <span class="w-8 h-8 bg-green-600 text-white rounded-full flex items-center justify-center">2</span>
    <span class="text-gray-200">选择初始近似值 $x_0$</span>
  </div>
  <div class="flex items-center gap-3 p-3 bg-amber-900/40 rounded-lg border border-amber-700/50">
    <span class="w-8 h-8 bg-amber-600 text-white rounded-full flex items-center justify-center">3</span>
    <span class="text-gray-200">迭代计算 $x_{n+1} = \phi(x_n)$</span>
  </div>
  <div class="flex items-center gap-3 p-3 bg-red-900/40 rounded-lg border border-red-700/50">
    <span class="w-8 h-8 bg-red-600 text-white rounded-full flex items-center justify-center">4</span>
    <span class="text-gray-200">判断收敛条件 $|x_{n+1} - x_n| < \epsilon$</span>
  </div>
</div>

### ⚠️ 收敛条件

$
|\phi'(x)| < 1 \quad \text{在根附近}
$

---
transition: zoom-in
---

## 🧩 例1：迭代法求方程根（通用框架）

### 通用迭代模板

```c
// 通用迭代框架
float x0 = 1.0, x1;
do {
    x1 = phi(x0);  // 迭代函数
    if(fabs(x1 - x0) < 1e-6) break;  // 收敛判断
    x0 = x1;  // 更新近似值
} while(1);
printf("根为：%f", x1);
```

### 📝 应用示例

求方程 $x^3 - x - 1 = 0$ 的根：

**迭代函数**：$\phi(x) = \sqrt[3]{x + 1}$

```c
float phi(float x) {
    return pow(x + 1, 1.0/3.0);
}
```

### 📊 迭代过程

| 迭代次数 | $x_n$ |
|----------|-------|
| 0 | 1.0 |
| 1 | 1.26 |
| 2 | 1.312 |
| 3 | 1.322 |
| 4 | 1.324 |

---
layout: image-right
image: ./assets/newton.png
transition: slide-left
---

## 🚀 例2：牛顿迭代法

### 核心思想
利用切线近似，二次收敛速度

**迭代公式**：
$
x_{n+1} = x_n - \frac{f(x_n)}{f'(x_n)}
$

```c
float newton_iteration(float x0) {
    float x1;
    do {
        x1 = x0 - f(x0) / f_prime(x0);
        if(fabs(x1 - x0) < 1e-6) break;
        x0 = x1;
    } while(1);
    return x1;
}
```

### ⚡ 收敛特性

| 特性 | 说明 |
|------|------|
| 收敛速度 | 二次收敛（快） |
| 初始值 | 需接近真实根 |
| 条件 | $f'(x) \neq 0$ |
| 复杂度 | $O(1)$ 每步 |

---
layout: image-right
image: ./assets/bisection.png
transition: slide-right
---

## 🔪 例3：二分法求方程根

### 适用条件
连续函数 $f(x)$，且 $f(a) \cdot f(b) < 0$

**核心思想**：每次将区间减半，保证根在区间内

```c
float bisection(float x1, float x2) {
    float x, f, f1 = f(x1), f2 = f(x2);
    do {
        x = (x1 + x2) / 2;      // 取中点
        f = f(x);                // 使用统一的函数调用
        if(fabs(f) < 1e-6) break; // 收敛判断
        if(f1 * f > 0) { x1 = x; f1 = f; }  // 根在右半区间
        else { x2 = x; f2 = f; }             // 根在左半区间
    } while(1);
    return x;
}
```

### 📊 收敛分析

| 特性 | 说明 |
|------|------|
| 收敛速度 | 线性收敛（较慢） |
| 初始值 | 只需含根区间 |
| 条件 | 连续函数 |
| 复杂度 | $O(log_2(\frac{b-a}{\epsilon}))$ |

---
layout: end
class: 'text-center px-20'
transition: fade
---

# 🎉 谢谢观看

<div class="text-xl mt-8 text-blue-500">
算法设计与分析 - 第4章
</div>

<div class="text-base mt-4 opacity-75">
基本的算法策略 - 迭代算法
</div>

<div class="mt-6 flex flex-wrap justify-center gap-3">
  <span class="px-4 py-2 bg-blue-900/60 text-blue-300 rounded-full animate-bounce border border-blue-700/50">🔄 迭代</span>
  <span class="px-4 py-2 bg-green-900/60 text-green-300 rounded-full animate-bounce delay-100 border border-green-700/50">📈 递推</span>
  <span class="px-4 py-2 bg-orange-900/60 text-orange-300 rounded-full animate-bounce delay-200 border border-orange-700/50">🔙 倒推</span>
  <span class="px-4 py-2 bg-purple-900/60 text-purple-300 rounded-full animate-bounce delay-300 border border-purple-700/50">⚡ 逼近</span>
</div>

<div class="text-sm mt-8 text-gray-500">
Press <kbd class="px-2 py-1 bg-gray-200 rounded">Space</kbd> to continue
</div>

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/slidevjs/slidev" target="_blank" alt="GitHub" title="Open in GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>