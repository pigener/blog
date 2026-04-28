`while` 循环 (Indefinite Loops)

**核心目标：** 理解何时使用“不确定性循环”，并掌握其控制逻辑。

## 1. 核心概念：为什么需要 `while`？

* **`for` 循环** 是“确定性”的：你知道要循环多少次（比如“遍历这个列表”或“重复10次”）。
* 
**`while` 循环** 是“不确定性”的：你不知道要循环多少次，只知道**什么时候该停** 。


**逻辑流程：**

`while` 循环会在条件为真（）时重复执行代码块。每次执行前都会检查条件 。

### 基础语法

```python
while condition:
    # 只要 condition 是 True，这就一直运行
    # 这里必须有改变 condition 的代码，否则会死循环

```
一旦条件变为假（），循环终止，程序继续向下执行 。
---

## 2. 进阶控制：`break`, `continue`, `else`

为了精准控制流程，Python 提供了三个关键词：
1. **`break` (立即停止):** 无论条件是否满足，立即强行跳出循环 。

2. **`continue` (跳过本次):** 停止当前的循环体执行，直接跳回到开头进行下一次条件判断 。

3. **`else` (正常结束):** 只有当循环是**正常结束**（即条件变成 False）时执行。如果是被 `break` 打断的，则**不**执行 `else` 块 。

---

## 3. 行业应用实战 (Real-world Scenarios)

不要只练习 `i = i + 1`。我们来看两个真实的场景。

### 场景一：金融科技 (FinTech) —— 财富目标计算器

**场景：** 你想存钱买房。你每月存一笔钱，这笔钱还会产生复利。你不知道具体**哪一年**能存够首付，这取决于增长率。这是一个典型的“未知迭代次数”问题 。

**代码与解析：**

```python
target = 500000       # 目标金额：50万
balance = 20000       # 初始存款
rate = 0.05           # 年利率 5%
monthly_deposit = 3000 # 每月定投
months = 0            # 计数器

# 逻辑：只要余额 < 目标，就必须继续存，不能停
while balance < target:
    # 1. 加上每月的存款
    balance += monthly_deposit
    
    # 2. 加上当月的利息 (简化算法)
    balance += balance * (rate / 12)
    
    # 3. 时间流逝
    months += 1

print(f"需要 {months // 12} 年 {months % 12} 个月才能达成目标。")

```

**关键点：** 我们无法预知 `months` 最终是多少，必须由 `while` 根据 `balance` 的实时变化来决定。

---

### 场景二：后端开发 —— 服务器连接重试 (Retry Logic)

**场景：** 在网络编程中，连接服务器并不总是一次成功。如果连接失败，我们不能无限等待（死循环），也不能只试一次就放弃。通常做法是：“尝试连接，直到成功，或者达到最大重试次数” 。

**代码与解析：**

```python
import random # 用于模拟连接结果

connected = False      # 连接状态
attempts = 0           # 当前尝试次数
max_retries = 3        # 最大允许重试次数

# 逻辑：两个条件同时满足才循环
# 1. 还没连上 (not connected)
# 2. 还没超过最大次数 (attempts < max_retries)
while not connected and attempts < max_retries:
    attempts += 1
    print(f"正在尝试第 {attempts} 次连接...")
    
    # 模拟连接：假设只有 20% 概率成功
    if random.random() > 0.8:
        connected = True  # 改变状态，这将导致循环终止
    else:
        print("连接失败，准备重试。")

# 善后处理：利用 else 区分是“连上了”还是“次数用完了”
if connected:
    print("系统：连接成功！[200 OK]")
else:
    print("错误：连接超时，请检查网络。")

```

**循序渐进解析：**

1. **复合条件：** `while not connected and attempts < max_retries`。这是 `while` 最强大的地方，可以同时监控多个状态。
2. **状态变更：** `connected = True` 是循环退出的“成功”出口。
3. **计数器限制：** `attempts` 是循环退出的“失败”出口（防止无限死循环）。
