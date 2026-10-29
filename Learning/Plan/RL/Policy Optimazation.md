## 1.2 马尔科夫决策过程（Markov Desision Process，MDP）
### 1.2.1 基本概念
- 智能体：
- 环境
- 状态：
- 状态空间:
- 动作
- 动作空间
- 奖励
### 1.2.2 有限任务和无限任务
- 轨迹(Trajectory, τ)：表示一串数据流：状态、动作、奖励、下一个状态
- 回合(Episode)：是Trajectory的一个特例，有限任务里，从开始到结束的这完整的一段轨迹，叫一个回合
- 采样(Rollout)：让这个策略与环境交互，并采集Trajectory数据
### 1.2.3 MDP五元组
$$M=<S,A,P,R,\gamma>$$
- S：状态空间（可能出现哪些状态）
- A：动作空间
- P：状态转移概率
- R：奖励函数
- γ：折扣因子
**期望**
>在一个随机事件中，重复做无数次，最终得到的长期平均

$$R(s,a)=E[R_{t+1}|S_t=s,A_t=a]$$
**折扣因子**
>智能体的目标是追求一个轨迹里所有奖励的累计值最大，我们把奖励的累积叫做回报（Return），记作$G_t$.

$$G_t=R_{t+1}+R_{t+2}+R_{t+3}+...$$
直接累加的问题：
- 数学上的发散
- 未来的不确定性：未来的奖励没有眼前的奖励把握大
$$
G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \dots = \sum_{k=0}^{\infty} \gamma^k R_{t+k+1} \qquad 0 \le \gamma \le 1
$$
### 1.2.4 马尔可夫性
>未来只取决于现在的状态，而与过去的状态无关

$$
P(S_{t+1} \mid S_t) = P(S_{t+1} \mid S_t, S_{t-1}, S_{t-2}, \dots S_0)
$$
**完全可观测MDP**和**部分可观测MDP**

**策略（Policy）**
常用$\pi$表示，根据状态决定要执行的动作，可以看作是一个从状态到动作的映射函数
- 确定性策略：$a=\pi(s)$
- 随机策略：$\pi(a|s)=P[A_t=a|S_t=s]$
### 1.2.5 轨迹的随机性

### 1.2.6 ε-Greedy策略
$\epsilon$-Greedy策略

$$
a_t = 
\begin{cases} 
\text{最优动作} & \text{以概率 } 1 - \epsilon \text{ （利用）} \\ 
\text{随机动作} & \text{以概率 } \epsilon \text{ （探索）} 
\end{cases}
$$

逐步衰减$\epsilon$ 的 $\epsilon$-Greedy 策略：训练初期，设置大的 $\epsilon$，随着训练进行，逐渐减小 $\epsilon$。
训练时$\epsilon$-Greedy策略，部署时常采用贪婪策略

---
## 1.3 价值函数
累积折扣回报：
$$
G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \dots = \sum_{k=0}^{\infty} \gamma^k R_{t+k+1}
$$价值函数计算的是对回报的期望。

**状态价值函数**
状态价值函数(State-Value Function), 用符号 \(V\) 表示，从状态 \(s\) 出发，一直按照策略 $\pi$来行动，可以获得的期望回报。

$$
V_\pi(s) = E_\pi[G_t|S_t = s]
$$
**最优状态价值函数**

$$
\begin{array}{ll}
\text{最优状态价值函数：} & V_*(s) = \max\limits_{\pi} V_\pi(s)
\end{array}
$$

最优策略保证可以获得最大的期望回报。但不保证具体某一轨迹可以拿到比其他策略更大的回报。

最优状态价值函数可以帮助我们找到最优策略。

**动作价值函数**
动作价值函数 (Action-Value Function), 用符号 $Q$ 表示，从状态 $s$ 出发，并做出动作 $a$，然后按照策略 $\pi$ 来行动，可以获得的期望回报。

$$
Q_\pi(s, a) = E_\pi[G_t \mid S_t = s, A_t = a]
$$

1. 状态 $s$ 下，对动作 $a$ 的评估。
2. 在状态 $s$ 下强制执行 $a$，和策略 $\pi$ 无关。
3. 动作价值和策略密切相关。

**最优动作价值函数**
用符号 $Q_*$ 表示，在状态 $s$ 下，并做出动作 $a$，然后由最优策略接管，可以获得的期望回报。

$$
\begin{aligned}
\text{动作价值函数：} \quad & Q_\pi(s, a) = E_\pi[G_t \mid S_t = s, A_t = a] \\
\text{最优动作价值函数：} \quad & Q_*(s, a) = \max_{\pi} Q_\pi(s, a)
\end{aligned}
$$
有了 $Q_*$ 对所有动作的评分，就可以选择最优动作，做出最优决策了。

---
## 1.4 贝尔曼方程
贝尔曼方程可以**简化对价值函数的计算**。它的本质可以用两个字来概括，那就是：**递归**。

回报的定义：
$$
G_t = R_{t+1} + \gamma R_{t+2} + \gamma^2 R_{t+3} + \gamma^3 R_{t+4} + \dots
$$
$$
G_t = R_{t+1} + \gamma(R_{t+2} + \gamma R_{t+3} + \gamma^2 R_{t+4} + \dots)
$$
$$
G_t = R_{t+1} + \gamma G_{t+1} \qquad \text{回报的递归定义}
$$

- **贝尔曼方程**
  - **贝尔曼期望方程**
    - 状态价值函数的贝尔曼期望方程
    - 动作价值函数的贝尔曼期望方程
  - **贝尔曼最优方程**
    - 最优状态价值函数的贝尔曼最优方程
    - 最优动作价值函数的贝尔曼最优方程

### 1.4.1 状态价值函数的贝尔曼期望方程
状态价值函数定义：
$$
V_\pi(s) = E_\pi[G_t|S_t = s]=E_\pi[R_{t+1} + \gamma G_{t+1}]
$$
利用递归思想：

当前状态价值，由即时奖励的期望和下一时刻的状态价值期望构成。

在 $t$ 时刻，$S_t = s$。状态价值 $V_\pi(s)$ 由 $R_{t+1}$ 和 $\gamma V_\pi(S_{t+1})$ 构成。

状态价值函数的贝尔曼期望方程：
$$
V_\pi(s) = E_\pi[\textcolor{red}{R_{t+1}} + \textcolor{blue}{\gamma V_\pi(S_{t+1})}|S_t = s]
$$

对期望形式展开：
$$
\textcolor{red}{E_\pi[R_{t+1}|S_t = s] = \sum_a [\pi(a|s)r(s,a)] }
$$
在状态s下，每个动作概率是不同的，每个动作的概率是$\pi(a|s)$,它获得的奖励是$r(s,a)$，对不同动作的奖励乘以即时奖励，然后累加，就可以获得即时奖励的期望
$$
\textcolor{blue}{E_\pi[\gamma V_\pi(S_{t+1})|S_t = s] = \sum_a \left\{ \pi(a|s) \sum_{s'} [p(s'|s,a)\gamma V_\pi(s')] \right\}}
$$
下一个状态$S_{t+1}$随机性来自两个
- 下一步动作a是随机的
- 状态转移的随机性，状态确定后，转移到$s'$是随机的
-上面公式的外层是对各种动作进行累加，内层则是对各种状态进行累加
$$
V_\pi(s) = \sum_a \left\{ \pi(a|s) \left[ r(s,a) + \sum_{s'} [p(s'|s,a)\gamma V_\pi(s')] \right] \right\}
$$
总结：当前状态价值=即时奖励的期望+下一状态价值的期望

### 1.4.2 例子
### 计算右偏状态价值

**\(P\)：**

| | \(a_{左}\) | \(a_{平}\) | \(a_{右}\) |
| :---: | :---: | :---: | :---: |
| **\(s_{左}\)** | \(s_{左}: 1.0\)<br>\(s_{平}: 0\)<br>\(s_{右}: 0\) | \(s_{左}: 1.0\)<br>\(s_{平}: 0\)<br>\(s_{右}: 0\) | \(s_{左}: 0.1\)<br>\(s_{平}: 0.4\)<br>\(s_{右}: 0.5\) |
| **\(s_{平}\)** | \(s_{左}: 1.0\)<br>\(s_{平}: 0\)<br>\(s_{右}: 0\) | \(s_{左}: 0.1\)<br>\(s_{平}: 0.8\)<br>\(s_{右}: 0.1\) | \(s_{左}: 0\)<br>\(s_{平}: 0\)<br>\(s_{右}: 1.0\) |
| **\(s_{右}\)** | \(s_{左}: 0.5\)<br>\(s_{平}: 0.4\)<br>\(s_{右}: 0.1\) | \(s_{左}: 0\)<br>\(s_{平}: 0\)<br>\(s_{右}: 1.0\) | \(s_{左}: 0\)<br>\(s_{平}: 0\)<br>\(s_{右}: 1.0\) |

**\(R\)：**

| | \(a_{左}\) | \(a_{平}\) | \(a_{右}\) |
| :---: | :---: | :---: | :---: |
| **\(s_{左}\)** | 0 | 0 | 1 |
| **\(s_{平}\)** | 0 | 2 | 0 |
| **\(s_{右}\)** | 1 | 0 | 0 |

**$(\pi)$：**

|               | \(a_{左}\) | \(a_{平}\) | \(a_{右}\) |
| :-----------: | :-------: | :-------: | :-------: |
| **\(s_{左}\)** |    0.2    |     0     |    0.8    |
| **\(s_{平}\)** |    0.1    |    0.8    |    0.1    |
| **\(s_{右}\)** |    0.8    |     0     |    0.2    |


贝尔曼期望方程：
$$
V_\pi(s_{右}) = E_\pi[R_{t+1} + \gamma V_\pi(S_{t+1})|S_t = s_{右}]
$$
$$
\begin{aligned}
E_\pi[R_{t+1}|S_t = s_{右}] &= \sum_a [\pi(a|s_{右})r(s_{右}, a)] \\
&= \pi(a_{左}|s_{右})r(s_{右}, a_{左}) + \pi(a_{右}|s_{右})r(s_{右}, a_{右}) \\
&= 0.8 \times 1 + 0.2 \times 0 = 0.8
\end{aligned}
$$
$$
\begin{aligned}
E_\pi[\gamma V_\pi(S_{t+1})|S_t = s_{右}] &= \sum_a \left\{ \pi(a|s_{右}) \sum_{s'} [p(s'|s_{右}, a)\gamma V_\pi(s')] \right\} \\
&= \gamma \left\{ \pi(a_{左}|s_{右}) \sum_{s'} [p(s'|s_{右}, a_{左})V_\pi(s')] + \pi(a_{右}|s_{右}) \sum_{s'} [p(s'|s_{右}, a_{右})V_\pi(s')] \right\} \\
&= 0.8 \left[ 0.8(0.5V_\pi(s_{左}) + 0.4V_\pi(s_{平}) + 0.1V_\pi(s_{右})) + 0.2V_\pi(s_{右}) \right] \\
&= 0.32V_\pi(s_{左}) + 0.256V_\pi(s_{平}) + 0.224V_\pi(s_{右})
\end{aligned}
$$

$$
V_\pi(s_{右}) = 0.8 + 0.32V_\pi(s_{左}) + 0.256V_\pi(s_{平}) + 0.224V_\pi(s_{右})
$$
$$
V_\pi(s_{右}) = 0.8 + 0.32V_\pi(s_{左}) + 0.256V_\pi(s_{平}) + 0.224V_\pi(s_{右})
$$
$$
V_\pi(s_{平}) = 1.6 + 0.144V_\pi(s_{左}) + 0.512V_\pi(s_{平}) + 0.144V_\pi(s_{右})
$$
$$
\begin{cases}
V_\pi(s_{左}) \approx 5.38 \\
V_\pi(s_{平}) \approx 6.45 \\
V_\pi(s_{右}) \approx 5.38
\end{cases}
$$

### 1.4.3 贝尔曼方程的思想
	你只考虑一步，把当前状态的价值转化为这一步得到的即时奖励和下一个状态的价值。
需要考虑从这一步到下一步的各种可能性，不同的动作，得到不同的奖励，不同的动作，导致下一步不同的状态。

### 1.4.4 动作价值函数的贝尔曼期望方程
$$
Q_\pi(s, a) = E_\pi[R_{t+1} + \gamma Q_\pi(S_{t+1}, A_{t+1})|S_t = s, A_t = a]
$$
期望展开形式：
$$
Q_\pi(s, a) = r(s, a) + \gamma \sum_{s'} \left\{ p(s'|s, a) \sum_{a'} [\pi(a'|s')Q_\pi(s', a')] \right\}
$$
### 1.4.5 **贝尔曼最优方程

贝尔曼期望方程：
$$
当前步的价值 = 即时奖励 + \gamma \times 下一步的价值
$$
全局最优策略的任意局部片段，也必然是该局部的最优策略。

### 1.4.6 最优状态价值函数的贝尔曼最优方程

状态价值函数的贝尔曼期望方程：

$$
V_\pi(s) = E_\pi[R_{t+1} + \gamma V_\pi(S_{t+1})|S_t = s]
$$

最优状态价值函数的贝尔曼最优方程：

$$
V_*(s) = \max_a E[R_{t+1} + \gamma V_*(S_{t+1})|S_t = s, A_t = a]
$$

展开期望形式：

$$
V_*(s) = \max_a \left\{ r(s,a) + \gamma \sum_{s'} [p(s'|s,a)V_*(s')] \right\}
$$

最优方程里含有 $\max$ 这个非线性的操作。所以我们无法通过解线性方程组的方式得到最优状态价值函数的值。

### 1.4.7 最优动作价值函数的贝尔曼最优方程

动作价值函数的贝尔曼期望方程：
$$
Q_\pi(s, a) = E_\pi[R_{t+1} + \gamma Q_\pi(S_{t+1}, A_{t+1})|S_t = s, A_t = a]
$$

最优动作价值函数的贝尔曼最优方程：
$$
Q_*(s, a) = E\left[R_{t+1} + \gamma \max_{a'} Q_*(S_{t+1}, a')|S_t = s, A_t = a\right]
$$

展开期望形式：
$$
Q_*(s, a) = r(s, a) + \gamma \sum_{s'} \left[ p(s'|s, a) \max_{a'} Q_*(s', a') \right]
$$
---

## 2.1 动态规划
### 2.1.1 有环境模型

**模型：环境模型**
1. 状态转移概率
2. 奖励函数

**有模型方法**：智能体已知（或者学习到）一个环境模型，在决策时，利用这个环境模型进行推演规划。

**无模型方法**：智能体在决策时不依赖一个明确的环境模型。
> 环境总是存在状态转移与奖励生成机制，有/无模型的区别是Agent是否显式学习和利用这个机制来做决策。

### 2.1.2 动态规划

有模型方法，根据环境模型，在状态的时序动态变化中，规划出最优决策。
分为**策略迭代**和**价值迭代**

**策略迭代**
	策略和状态价值函数是绑定的，策略改变状态价值函数也会随之改变
1. **策略评估**：计算出当前策略下所有的状态价值。
		通过列出状态价值函数的贝尔曼方程，解线性方程组的问题：
			1.状态数多时：变量多，方程多，导致计算量过大，解方程组变得不可求解。最优状态价值函数的贝尔曼最优方程，包含 $\max$ 操2. 作，不能通过解线性方程组求解。
		![[Pasted image 20261021202657.png]]
2. **策略改进**：根据状态价值，找出每个状态下的最优动作，用最优动作改进策略。
		![[Pasted image 20261021203533.png]]
		![[Pasted image 20261021204326.png]]![[Pasted image 20261022091755.png]]![[Pasted image 20261022091902.png]]
**代码实现：**
```python
# 定义状态和动作的映射，方便阅读
STATES = ['s左', 's平', 's右']
ACTIONS = ['a左', 'a平', 'a右']
NUM_STATES = len(STATES)
NUM_ACTIONS = len(ACTIONS)
# 初始化状态转移概率矩阵 P[state][action][next_state]
P = np.zeros((NUM_STATES, NUM_ACTIONS, NUM_STATES))
# === 填充状态转移概率 ===
# 在 s左 (0)
P[0, 0] = [1.0, 0.0, 0.0]  # 做 a左
P[0, 1] = [1.0, 0.0, 0.0]  # 做 a平
P[0, 2] = [0.1, 0.4, 0.5]  # 做 a右
# 在 s平 (1)
P[1, 0] = [1.0, 0.0, 0.0]  # 做 a左
P[1, 1] = [0.1, 0.8, 0.1]  # 做 a平
P[1, 2] = [0.0, 0.0, 1.0]  # 做 a右
# 在 s右 (2)
P[2, 0] = [0.5, 0.4, 0.1]  # 做 a左
P[2, 1] = [0.0, 0.0, 1.0]  # 做 a平
P[2, 2] = [0.0, 0.0, 1.0]  # 做 a右
# === 填充奖励矩阵 R[state][action] ===
R = np.zeros((NUM_STATES, NUM_ACTIONS))
R[0, 2] = 1.0  # 在 s左, 做 a右
R[1, 1] = 2.0  # 在 s平, 做 a平
R[2, 0] = 1.0  # 在 s右, 做 a左
```

```python
# === 价值迭代算法 ===
gamma = 0.8  # 折扣因子 (Discount factor)
theta = 1e-5 # 收敛阈值 (Threshold for convergence)
V = np.zeros(NUM_STATES)  # 初始化所有状态的价值为 0
iteration = 0
while True:
    delta = 0
    V_new = np.zeros(NUM_STATES)
    for s in range(NUM_STATES):
        # 计算当前状态下，采取每种动作的 Q 值: Q(s,a) = R(s,a) + gamma * sum(P(s'|s,a) * V(s'))
        Q_values = np.zeros(NUM_ACTIONS)
        for a in range(NUM_ACTIONS):
            expected_value = sum(P[s, a, s_prime] * V[s_prime] for s_prime in range(NUM_STATES))
            Q_values[a] = R[s, a] + gamma * expected_value
            
        # 价值迭代的核心：取最大的 Q 值作为当前状态的新价值
        V_new[s] = np.max(Q_values)
        
        # 计算最大变化量，用于判断是否收敛
        delta = max(delta, abs(V[s] - V_new[s]))
        
    V = V_new
    iteration += 1
    if delta < theta:
        break
print(f"价值迭代在 {iteration} 次后收敛。\n")
```

```python
# === 提取最优策略 (Policy Extraction) ===
optimal_policy = np.zeros(NUM_STATES, dtype=int)

for s in range(NUM_STATES):
    Q_values = np.zeros(NUM_ACTIONS)
    for a in range(NUM_ACTIONS):
        expected_value = sum(P[s, a, s_prime] * V[s_prime] for s_prime in range(NUM_STATES))
        Q_values[a] = R[s, a] + gamma * expected_value
    # 找到使 Q 值最大的动作
    optimal_policy[s] = np.argmax(Q_values)

# === 打印最终结果 ===
print("=== 最终状态价值 (State Values) ===")
for s in range(NUM_STATES):
    print(f"状态 {STATES[s]}: {V[s]:.4f}")

print("\n=== 最优策略 (Optimal Policy) ===")
for s in range(NUM_STATES):
    best_action = ACTIONS[optimal_policy[s]]
    print(f"在状态 {STATES[s]}, 机器人应该采取动作: {best_action}")
```

## 2.2 无模型方法

动态规划：基于环境模型，每一步根据 **环境模型**（奖励函数和状态转移概率），算出状态价值。状态价值就是这个状态下回报的期望。

> **大数定律**：当你把一个随机实验重复很多次时，实际结果的平均值，会越来越接近理论上的期望值。

```text
             ┌── 无模型预测
无模型方法 ──┤
             └── 无模型控制
```
在给定一个策略 $\pi$ 的情况下，计算出状态价值函数 $V_\pi(s)$。

```text
             ┌── 蒙特卡洛 (Monte Carlo MC)
无模型预测 ──┤
             └── 时序差分 (Temporal Difference TD)
```

### 2.2.1 蒙特卡洛
> 利用大数定理，从环境中多次采样，然后平均

![[Pasted image 20261022111124.png]]

对状态 $s_t$，进行n次采样，得到n个真实的回报，记为：$g_t^1, g_t^2, \dots, g_t^n$。
$$
V_\pi^n(s_t) = \frac{1}{n} \sum_{i=1}^{n} g_t^i
$$
**不保存历史值：**
	因为数据是不断产生的，不希望一直保存所有历史的回报值，用于每次重新计算均值。所以希望在上一步计算的均值上进行修改。$V_\pi^{n-1}(s_t)$是前面$n-1$次回报的均值，再乘以$n-1$就是前面$n-1$次的回报累积值了。再加上$g_t^n$，就是前面n次回报的累计值了
$$
V_\pi^n(s_t) = \frac{1}{n} [g_t^n + (n - 1)V_\pi^{n-1}(s_t)]
$$
$$
V_\pi^n(s_t) = \frac{1}{n} [g_t^n + nV_\pi^{n-1}(s_t) - V_\pi^{n-1}(s_t)]
$$
$$
\underbrace{V_\pi^n(s_t)}_{\text{新均值}} = \underbrace{V_\pi^{n-1}(s_t)}_{\text{历史均值}} + \underbrace{\frac{1}{n}}_{\text{学习率}} [\underbrace{g_t^n - V_\pi^{n-1}(s_t)}_{\text{误差}}]
$$
**$\frac{1}{n}$ 作为学习率的问题：**
随着n的增大，学习率越来越小。新采样数据对均值（状态价值）影响越来越小。
采用固定学习率：$\alpha \in (0, 1]$
$$
V_\pi^n(s_t) = V_\pi^{n-1}(s_t) + \alpha [g_t^n - V_\pi^{n-1}(s_t)]
$$
每次都是对一个回合的完整回报值的采样。
没有偏差的，但方差很大。

**限制**：只能针对有限任务，必须有终止状态。

### 2.2.2 时序差分（TD）
> 不用等回合结束就可以更新状态价值
	从A到B导航估计20小时，现在已经驾驶10个小时，导航估计还要13小时。那么从共23小时更准确，因为前面的10个小时是已经确定的。这就是时序差分的思想。

蒙特卡洛：
$$
V_\pi^n(s_t) = V_\pi^{n-1}(s_t) + \alpha[g_t^n - V_\pi^{n-1}(s_t)]
$$
时序差分：
$$
V_\pi(s_t) \leftarrow V_\pi(s_t) + \alpha\underbrace{[\underbrace{r_{t+1} + \gamma V_\pi(s_{t+1})}_{\text{TD Target}} - V_\pi(s_t)]}_{\text{TD Error}}
$$
TD Target是由，$r_{t+1}$走了一步之后的真实奖励，下一步状态价值的折扣估计$\gamma V_\pi(s_{t+1})$组成。$V_\pi(s_t)]$原来预测的状态价值

时序差分结合了动态规划和蒙特卡洛的优点：
像蒙特卡洛法一样不需要知道环境模型。
像动态规划一样每走一步就更新一次。

**SARSA**
$(S_t, A_t, R_t, S_{t+1}, A_{t+1})$ : SARSA
$$
Q(s_t, a_t) \leftarrow Q(s_t, a_t) + \alpha\underbrace{[\underbrace{r_{t+1} + \gamma Q(s_{t+1}, a_{t+1})}_{\text{TD Target}} - Q(s_t, a_t)]}_{\text{TD Error}}
$$

$\epsilon - Greedy$ 策略：
$$
\begin{cases}
\text{最优动作} & \text{以概率 } 1-\epsilon \text{ （利用）} \\
\text{随机动作} & \text{以概率 } \epsilon \text{ （探索）}
\end{cases}
$$
每个动作价值，都反映了在 $\epsilon - Greedy$ 策略下的真实动作价值。
采样策略和评估改进的策略是同一个的强化学习算法叫作同策略（On-policy）算法。

> 与环境交互的策略是 $\epsilon - Greedy$ 策略
> Q表里的动作价值绑定的是 $\epsilon - Greedy$ 策略

**SARSA是On-policy算法**

![[Pasted image 20261022140604.png]]
在$s_0$状态下，通过动作$a_0$，获得奖励$r_1$，更新到状态$s_1$
上面图的公式中的 $a_1$,$a_2$就是未知的，其他的都是已知的，未知的参数就是通过 $\epsilon - Greedy$ 策略生成的

**Q-Learning**
$$
Q(s_t, a_t) \leftarrow Q(s_t, a_t) + \alpha \underbrace{\left[ \underbrace{r_{t+1} + \gamma \max_a Q(s_{t+1}, a)}_{\text{TD Target}} - Q(s_t, a_t) \right]}_{\text{TD Error}}
$$

> 与环境交互的策略是 $\epsilon - Greedy$ 策略
> Q表里的动作价值绑定的是贪婪策略

与环境交互策略和评估改进的策略**不是**同一个的强化学习算法叫作异策略（Off-policy）算法。

Q-Learning是Off-policy算法
![[Pasted image 20261022143325.png]]

## 2.3 动手实现悬崖漫步
**悬崖漫步问题**

| | | | | | | | | | | | |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| | | | | | | | | | | | |
| | | | | | | | | | | | |
| **S** | 🟦 | 🟦 | 🟦 | 🟦 | 🟦 | 🟦 | 🟦 | 🟦 | 🟦 | 🟦 | **G** |


**状态空间**：网格中地每一个格子就是一个状态。起点（Start），左下角地格子，智能体从这里出发。终点（Goal），右下角地格子，智能体到达终点意味着当前回合结束。悬崖，最下边一行里除了起点和终点，剩下地格子都是悬崖。上边3行格子都是普通路径。

**动作空间**：在每个状态下，智能体可以执行上下左右四个方向地移动动作。如果动作导致撞墙，智能体会停留在原地。

**奖励函数**：正常移动，每走一步，获得奖励是-1。这样促使智能体寻找到达终点地最短路径。掉下悬崖，如果智能体踩入“悬崖”区域，会受到一个巨大地惩罚，通常奖励是-100。

**状态转移**：所有的转移都是确定性地。正常情况下向哪个方向移动，就会到哪个方向地下一个格子。碰到墙壁会停在原地。一旦掉下悬崖，智能体会被传送回起点。而且当前地回合不会结束，智能体必须继续走，直到抵达终点。

**折扣因子**：因为这个任务是个有限任务，不会出现奖励是无穷地情况。我们简单地设置折扣因子为1。

![[Pasted image 20261023103113.png]]```

```python
import gymnasium as gym
# 1. 创建悬崖漫步环境
env = gym.make('CliffWalking-v1')
# 2. 初始化环境，获取初始状态
state, info = env.reset()
done = False
total_reward = 0
print(f"初始状态: {state}")
# 3. 开始交互循环
while not done:
    # 随机采样一个动作（在悬崖漫步中是 0, 1, 2, 3 代表上下左右）
    action = env.action_space.sample()
    # 与环境交互
    next_state, reward, terminated, truncated, info = env.step(action)
    # 只要自然终止或被截断，回合就结束
    done = terminated or truncated
    total_reward += reward
    # 状态更新
    state = next_state
print(f"回合结束！ 随机瞎逛的总奖励为: {total_reward}")
# 4. 释放环境资源
env.close()


**$\epsilon - Greedy$ 策略定义**
```python
# ==========================================
# 2. 策略定义 (Epsilon-Greedy)
# ==========================================
def epsilon_greedy(state, q_tabl12e, epsilon=EPSILON):
    if np.random.rand() < epsilon:
        return np.random.choice(4)  # 随机探索
    else:
        return np.argmax(q_table[state])  # 利用已知最优动作
        

# 创建 Q table。用 0 初始化。形状为: 状态数 x 动作数
q_table = np.zeros((env.observation_space.n, env.action_space.n))


# 创建环境并互动
env = gym.make('CliffWalking-v1')
state, _ = env.reset()
action = epsilon_greedy(state, q_table)
next_state, reward, terminated, truncated, _ = env.step(action)
```
### 代码实现

**SARSA 更新 (On-policy)**
$$
Q(s_t, a_t) \leftarrow Q(s_t, a_t) + \alpha[r_{t+1} + \gamma Q(s_{t+1}, a_{t+1}) - Q(s_t, a_t)]
$$

**Q-Learning 更新 (Off-policy)**
$$
Q(s_t, a_t) \leftarrow Q(s_t, a_t) + \alpha \left[ r_{t+1} + \gamma \max_a Q(s_{t+1}, a) - Q(s_t, a_t) \right]
$$

**完整代码逻辑：**
```python
while not done:
    next_state, reward, terminated, truncated, _ = env.step(action)
    done = terminated or truncated
    total_reward += reward

    # SARSA 更新 (On-policy)
    if algo == 'sarsa':
        next_action = epsilon_greedy(next_state, q_table)
        td_target = reward + GAMMA * q_table[next_state][next_action] * (not done)
        q_table[state][action] += ALPHA * (td_target - q_table[state][action])
        action = next_action

    # Q-Learning 更新 (Off-policy)
    elif algo == 'q_learning':
        best_next_action = np.argmax(q_table[next_state])
        td_target = reward + GAMMA * q_table[next_state][best_next_action] * (not done)
        q_table[state][action] += ALPHA * (td_target - q_table[state][action])
        action = epsilon_greedy(next_state, q_table)

    state = next_state
```
![[Pasted image 20261023113051.png]]

---
## 3.1 DQN原理及核心机制（DQN：Deep Q Network)
### 3.1.1 表格型方法的问题
![[Pasted image 20261023113246.png]]
	表格型方法是没有泛化能力的，每个表格之间是相互独立的。
比如玩游戏，假设屏幕是210×160分辨率的彩色图像，每个像素有RGB3个通道，每个通道有256种可能的取值。如果用Q表存储，即使两个图像只有一个像素的差异，也会用完全不同的Q值去存储。而且这样也导致数据稀疏，绝大部分的状态，智能体在训练数据里根本没有遇到过。Q值永远都是初始值。同一个动作对于每个状态的Q值都是独立学习的。两个图像非常相似，哪怕只有一个像素的差异也不能共享学习到的信息。

### DQN (Deep Q Network)

寻找一个函数：$F$

$$
F(s) = [q_{a_1}, q_{a_2}, q_{a_3}]
$$
<div align="center">
　↑　　　　　↑<br>
输入当前状态s　　输出每个动作的Q值
</div>

不需要存储所有状态-动作的Q值。每次输入一个状态，函数实时计算出所有动作的Q值。

神经网络的又是：1.极强表达能力；2.极强的特征表达能力；3.结构灵活，适应性强
### 3.1.2 DQN的核心思想
更新的是神经网络参数$\theta$
![[Pasted image 20261023115525.png]]
### 3.1.3 DQN的训练过程
![[Pasted image 20261023115922.png]]
但是如果用上述方法进行训练，会发现根本训练不起来
因为这里的label会随着$θ$的变化而变化

![[Pasted image 20261026093503.png]]
因此在训练的时候使用在线网络$θ$是变化的，而label使用的是目标网络$θ^-$。这个$θ^-$的参数又是怎么来的呢，它一开始会先复制一份在线网络的参数然后冻结，每当训练一段时间，比方说1000步之后把新的在线网络的参数再复制过来再冻结，进入下一个周期。这样可以保证一段时间内，在线网络的训练目标是固定的，可以稳定训练

**经验回放**
每次和环境交互，得到一条经验：
$$
e_t = (s_t, a_t, r_{t+1}, s_{t+1}, done_{t+1})
$$
$done_{t+1})$，布尔值，表示是否是结束状态

## 3.2 DQN进阶
### Double DQN

> DQN存在高估问题

| DQN的Label值： | $\max_a Q(s_{t+1}, a; \theta^-)$ 进行两部分操作： |
| :--- | :--- |
| $$r_{t+1} + \gamma \max_a Q(s_{t+1}, a; \theta^-)$$ | 1. 找出状态 $s_{t+1}$ 下的最优动作。<br>2. 给出最优动作的价值。<br><br>*(以上根据 Target Network 输出的 Q 值)* |

| 理论值： | 目标网络预测值： | DQN系统性高估的两个原因： |
| :--- | :--- | :--- |
| $\begin{cases} a_1 = 4.1 \\ a_2 = 5.2 \\ \color{blue}{a_3 = 5.5} \\ a_4 = 5.4 \\ a_5 = 0.5 \end{cases}$ | $\begin{cases} a_1 = 3.8 \\ \color{red}{a_2 = 5.7} \\ a_3 = 5.0 \\ a_4 = 5.4 \\ a_5 = 1.0 \end{cases}$ | 1. $\max$ 操作保留了正向误差。<br><br>2. 下游Q值高估会传递给上游Q值。 |

## 4.1 策略网络（Policy Network）
策略网络是通过神经网络来拟合策略函数，记作$\pi_θ(a|s)$，θ是神经网络的参数，它的输入为智能体当前观测到的特征s，输出为在该状态下采取各个动作a的概率分布
![[Pasted image 20261026094851.png]]

**基于价值和基于策略的方法**

**基于价值的方法（Value-based）：**

DQN，学习最优动作价值，然后根据最优动作价值间接得到策略。

**基于策略的方法（Policy-based）：**

直接学习一个策略函数：$\pi_\theta(a|s)$，这个策略函数可以直接给出在状态 $s$ 下，做不同的动作的概率。通过学习，让这个策略不断逼近最优策略。

## 4.2 策略梯度
### 4.2.1 策略网络如何训练
![[Pasted image 20261026102301.png]]
### 4.2.2 策略期望回报最大
比方：网游戏，利用策略网络的策略来玩超级玛丽，每玩一局都会得到轨迹$\tau$，$\tau=(s_0,a_0,r_1,s_1.a_1,...,s_t)$，这条策略的总回报是：$$
G(\tau) = \sum_{t=1}^{T} \gamma^{t-1} r_t
$$$\gamma$：折扣因子
因为目标不是某一局的回报最大，而是总体平均期望最大，所以目标就是最大化这个期望：$$
J(\theta) = E_{\tau \sim p_\theta(\tau)}[G(\tau)]
$$
对于具体的轨迹$\tau$，它的回报$G(\tau)$，这是由环境决定的。我们可以改变的是策略，也就是通过改变神经网络的参数$\theta$，让高回报的轨迹出现的概率高一些，让低回报的轨迹出现的概率低一些，这样就可以让回报的期望$J(\theta)$变大

数学实现：
$$
\nabla_\theta J(\theta)
$$
我们可以让目标函数$J(\theta)$对$\theta$求梯度$\nabla_\theta J(\theta)$，$\theta$沿梯度方向进行改变是$J(\theta)$增长最快的方向。利用梯度上升，我们就可以让$J(\theta)$变大
$$
\theta \leftarrow \theta + \alpha \nabla_\theta J(\theta)
$$
**求解策略梯度**
$$
J(\theta) = E_{\tau \sim p_\theta(\tau)}[G(\tau)]
$$
$$
\begin{aligned}
\nabla_\theta J(\theta) &= \nabla_\theta \sum_{\tau} p_\theta(\tau)G(\tau) \\
&= \sum_{\tau} \nabla_\theta p_\theta(\tau)G(\tau)
\end{aligned}
$$
把期望展开成轨迹概率与轨迹累计折扣回报的乘积，再把求导符号移入累加符号内，其中θ之和前两项有关，最后一项可以看成是常数。
因为上面是累加，所以要找出所有可能的轨迹τ再累加，这是不现实的。

**蒙特卡洛方法：**
当我们无法直接计算一个随机变量的期望时，可以通过大量随机采样，使用这些样本结果的平均值来近似真实期望。
**餐厅每人消费金额的期望值：**

|                      期望公式计算                       |          蒙特卡洛近似          |
| :-----------------------------------------------: | :----------------------: |
| $\sum_{\text{不同金额}} (\text{金额} \times \text{概率})$ | 客户里采样一批数据，<br>计算他们消费的平均值 |
$$
\begin{aligned}
\nabla_\theta J(\theta) &= \nabla_\theta \sum_{\tau} p_\theta(\tau)G(\tau) \\
&= \sum_{\tau} \nabla_\theta p_\theta(\tau)G(\tau) \\
&= \sum_{\tau} {\color{red}{p_\theta(\tau)}} \frac{\color{blue}{\nabla_\theta p_\theta(\tau)}}{\color{blue}{p_\theta(\tau)}} {\color{blue}{G(\tau)}} \\
&= E_{\tau \sim p_\theta(\tau)} \left[ \frac{\color{blue}{\nabla_\theta p_\theta(\tau)}}{\color{blue}{p_\theta(\tau)}} G(\tau) \right]
\end{aligned}
$$
通过同时×和÷$\color{red}{p_\theta(\tau)}$，得到期望的表达形式。$\color{red}{p_\theta(\tau)}$是概率$\frac{\color{blue}{\nabla_\theta p_\theta(\tau)}}{\color{blue}{p_\theta(\tau)}} {\color{blue}{G(\tau)}}$是统计值。就可以把第三行公式的累加形式写成期望的形式，再根据对数函数求导法则
$$
\nabla_x \log f(x) = \frac{\nabla_x f(x)}{f(x)}
$$
一个函数的梯度除以函数本身，等于对函数先取对数，再求梯度
$$ \begin{aligned} &= E_{\tau \sim p_\theta(\tau)} \left[ \frac{\nabla_\theta p_\theta(\tau)}{p_\theta(\tau)} G(\tau) \right] \\ &= E_{\tau \sim p_\theta(\tau)} [\nabla_\theta \log p_\theta(\tau) G(\tau)] \end{aligned} $$ | **蒙特卡洛法：**<br>利用 $\pi_\theta$ 采样多个轨迹，求 $\nabla_\theta \log p_\theta(\tau) G(\tau)$ 的平均值，就得到 $\nabla_\theta J(\theta)$
$$ \theta \leftarrow \theta + \alpha \nabla_\theta J(\theta) $$

$$
\boxed{\nabla_\theta J(\theta) = E_{\tau \sim p_\theta(\tau)}[\nabla_\theta \log p_\theta(\tau)G(\tau)]}
$$
$$
p_\theta(\tau) = p(s_0) \prod_{t=0}^{T} [\pi_\theta(a_t|s_t)p(s_{t+1}|s_t, a_t)]
$$
$p_\theta(\tau)$计算的是一条轨迹出现的概率，$p(s_0)$ 是轨迹初始状态概率，后面是每一步状态的概率的连乘。每一步状态转移的概率都是由两步构成的，首先是策略选择动作的概率$\pi_\theta(a_t|s_t)$ （这个是神经网络的输出，这个我们可以知道），下一步是状态转移概率$p(s_{t+1}|s_t, a_t)$。剩下的我们因为没有环境模型，就没有办法知道了
$$
p_\theta(\tau) = p(s_0) \prod_{t=0}^{T} [\pi_\theta(a_t|s_t)p(s_{t+1}|s_t, a_t)]
$$
两边同时取对数，再把右边的对数展开
$$
\begin{aligned}
\log p_\theta(\tau) &= \log \left[ p(s_0) \prod_{t=0}^{T} [\pi_\theta(a_t|s_t)p(s_{t+1}|s_t, a_t)] \right] \\
&= \log p(s_0) + \sum_{t=0}^{T} \log \pi_\theta(a_t|s_t) + \sum_{t=0}^{T} \log p(s_{t+1}|s_t, a_t)
\end{aligned}
$$
第一项和第三项是没有θ的，所以求导后为0
$$
\nabla_\theta \log p_\theta(\tau) = \sum_{t=0}^{T} \nabla_\theta \log \pi_\theta(a_t|s_t)
$$
再把该结果带入之前的期望公式
$$
\boxed{
\begin{aligned}
\nabla_\theta J(\theta) &= E_{\tau \sim p_\theta(\tau)} \left[ \left( \sum_{t=0}^{T} \nabla_\theta \log \pi_\theta(a_t|s_t) \right) G(\tau) \right] \\
&\approx \frac{1}{N} \sum_{n} \left[ \left( \sum_{t=0}^{T} \nabla_\theta \log \pi_\theta(a_t^n|s_t^n) \right) G(\tau^n) \right]
\end{aligned}
}
$$
### 4.2.3 策略梯度的直观理解
$$
\nabla_\theta J(\theta) = E_{\tau \sim p_\theta(\tau)} \left[ \left( \sum_{t=0}^{T} \nabla_\theta \log \pi_\theta(a_t|s_t) \right) G(\tau) \right]
$$
$$
\nabla_\theta J(\theta) = E_{\tau \sim p_\theta(\tau)} \left[ \sum_{t=0}^{T} [\color{blue}{\nabla_\theta \log \pi_\theta(a_t|s_t) G(\tau)}] \right]
$$

* 如果一条轨迹的总回报 $G(\tau)$ 是一个正数，那么梯度方向就是增加这个轨迹里所有动作出现的概率。回报越大，提升幅度越大。
* 如果一条轨迹的总回报 $G(\tau)$ 是一个负数，那么梯度方向就是减少这个轨迹里所有动作出现的概率。回报越小，降低幅度越大。