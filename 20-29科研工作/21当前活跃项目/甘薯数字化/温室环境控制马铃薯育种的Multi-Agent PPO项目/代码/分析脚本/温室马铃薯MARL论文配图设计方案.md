# 温室环境控制马铃薯育种Multi-Agent PPO论文配图设计方案

## 1. 系统架构图设计

### 1.1 整体架构图 (Figure 1)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    Multi-Agent PPO Greenhouse Control System                │
├─────────────────────────────────────────────────────────────────────────────┤
│  Environmental Sensing Layer / 环境感知层                                   │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐   │
│  │Temperature│ │Humidity │ │Soil     │ │Light    │ │CO2      │ │Weather  │   │
│  │Sensor     │ │Sensor   │ │Moisture │ │Sensor   │ │Sensor   │ │Station  │   │
│  │温度传感器  │ │湿度传感器│ │土壤传感器│ │光照传感器│ │CO2传感器│ │气象站   │   │
│  └─────────┘ └─────────┘ └─────────┘ └─────────┘ └─────────┘ └─────────┘   │
│       │           │           │           │           │           │       │
│       └───────────┼───────────┼───────────┼───────────┼───────────┘       │
│                   │           │           │           │                   │
├─────────────────────────────────────────────────────────────────────────────┤
│  Multi-Agent Decision Layer / 多智能体决策层                                │
│                                                                             │
│  ┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐ ┌─────────────┐│
│  │Temperature Agent│ │Humidity Agent   │ │Irrigation Agent │ │Light Agent  ││
│  │温度控制智能体    │ │湿度控制智能体    │ │灌溉控制智能体    │ │光照控制智能体││
│  │                 │ │                 │ │                 │ │             ││
│  │┌───────────────┐│ │┌───────────────┐│ │┌───────────────┐│ │┌───────────┐││
│  ││Actor Network  ││ ││Actor Network  ││ ││Actor Network  ││ ││Actor      │││
│  ││策略网络       ││ ││策略网络       ││ ││策略网络       ││ ││Network    │││
│  │└───────────────┘│ │└───────────────┘│ │└───────────────┘│ │└───────────┘││
│  │┌───────────────┐│ │┌───────────────┐│ │┌───────────────┐│ │┌───────────┐││
│  ││Critic Network ││ ││Critic Network ││ ││Critic Network ││ ││Critic     │││
│  ││价值网络       ││ ││价值网络       ││ ││价值网络       ││ ││Network    │││
│  │└───────────────┘│ │└───────────────┘│ │└───────────────┘│ │└───────────┘││
│  └─────────────────┘ └─────────────────┘ └─────────────────┘ └─────────────┘│
│           │                   │                   │                 │      │
│           └───────────────────┼───────────────────┼─────────────────┘      │
│                               │                   │                        │
│  ┌─────────────────────────────────────────────────────────────────────────┐│
│  │              Coordination & Communication Module                       ││
│  │                     协调与通信模块                                      ││
│  │  • Information Sharing / 信息共享                                      ││
│  │  • Conflict Resolution / 冲突解决                                      ││
│  │  • Global State Estimation / 全局状态估计                             ││
│  │  • Cooperative Reward Distribution / 协作奖励分配                      ││
│  └─────────────────────────────────────────────────────────────────────────┘│
│                                       │                                     │
├─────────────────────────────────────────────────────────────────────────────┤
│  Actuator Control Layer / 执行器控制层                                      │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐   │
│  │Heating/ │ │Humidify/│ │Irrigation│ │LED Light│ │Ventila- │ │Nutrient │   │
│  │Cooling  │ │Dehumid  │ │System   │ │Control  │ │tion     │ │Supply   │   │
│  │加热制冷  │ │加湿除湿  │ │灌溉系统  │ │LED控制  │ │通风系统  │ │营养供给  │   │
│  └─────────┘ └─────────┘ └─────────┘ └─────────┘ └─────────┘ └─────────┘   │
│       │           │           │           │           │           │       │
├─────────────────────────────────────────────────────────────────────────────┤
│  Potato Growth Environment / 马铃薯生长环境                                 │
│  ┌─────────────────────────────────────────────────────────────────────────┐│
│  │  🥔 Potato Plants → Growth Monitoring → Yield Prediction               ││
│  │     马铃薯植株   →   生长监测      →    收获预测                        ││
│  │                                                                         ││
│  │  Growth Stages: Sprouting → Vegetative → Tuber Formation → Maturation ││
│  │  生长阶段：发芽期 → 营养生长期 → 块茎形成期 → 成熟期                    ││
│  └─────────────────────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────────────────┘
```

**图表标题**: Multi-Agent PPO System Architecture for Greenhouse Potato Cultivation
**中文标题**: 温室马铃薯种植的多智能体PPO系统架构

**LaTeX代码**:
```latex
\begin{figure}[htbp]
\centering
\includegraphics[width=\textwidth]{figures/system_architecture.pdf}
\caption{Multi-Agent PPO System Architecture for Greenhouse Potato Cultivation. The system consists of four layers: environmental sensing, multi-agent decision making, actuator control, and potato growth environment. Four specialized agents (temperature, humidity, irrigation, and light control) coordinate through a central communication module to optimize potato yield prediction and environmental control.}
\label{fig:system_architecture}
\end{figure}
```

### 1.2 智能体交互关系图 (Figure 2)

```
                    Global Environment State
                           全局环境状态
                              ↓
    ┌─────────────────────────────────────────────────────────┐
    │              Coordination Module                        │
    │                协调模块                                  │
    └─────────────────────────────────────────────────────────┘
              ↙        ↓        ↓        ↘
    ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐
    │Temperature  │ │Humidity     │ │Irrigation   │ │Light        │
    │Agent        │ │Agent        │ │Agent        │ │Agent        │
    │温度智能体    │ │湿度智能体    │ │灌溉智能体    │ │光照智能体    │
    └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘
           ↓               ↓               ↓               ↓
    ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐
    │Action:      │ │Action:      │ │Action:      │ │Action:      │
    │Heat/Cool    │ │Humid/Dehum  │ │Water Flow   │ │LED Power    │
    │加热/制冷     │ │加湿/除湿     │ │灌溉流量      │ │LED功率      │
    └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘
           ↓               ↓               ↓               ↓
    ┌─────────────────────────────────────────────────────────────┐
    │                Environment Response                         │
    │                    环境响应                                  │
    │  Temperature ↔ Humidity ↔ Soil Moisture ↔ Light Intensity │
    │    温度      ↔   湿度   ↔   土壤湿度    ↔    光照强度      │
    └─────────────────────────────────────────────────────────────┘
                              ↓
    ┌─────────────────────────────────────────────────────────────┐
    │                 Reward Calculation                          │
    │                    奖励计算                                  │
    │  R = α·R_yield + β·R_energy + γ·R_growth + δ·R_cooperation │
    └─────────────────────────────────────────────────────────────┘
```

## 2. 算法流程图设计

### 2.1 Multi-Agent PPO训练流程图 (Figure 3)

```
开始训练 Start Training
        ↓
┌─────────────────────────────────────────────────────────────────┐
│                    Environment Reset                            │
│                      环境重置                                    │
│  • Initialize greenhouse conditions / 初始化温室条件             │
│  • Set random weather patterns / 设置随机天气模式                │
│  • Plant potato seeds / 种植马铃薯种子                           │
└─────────────────────────────────────────────────────────────────┘
        ↓
┌─────────────────────────────────────────────────────────────────┐
│                  Observation Collection                         │
│                     观测数据收集                                 │
│  For each agent i ∈ {temp, humid, irrig, light}:              │
│  • s_i^t = [sensor_data, historical_actions, time_features]    │
└─────────────────────────────────────────────────────────────────┘
        ↓
┌─────────────────────────────────────────────────────────────────┐
│                    Action Selection                             │
│                      动作选择                                    │
│  For each agent i:                                             │
│  • π_i(a_i^t | s_i^t) → a_i^t                                 │
│  • Add exploration noise / 添加探索噪声                         │
└─────────────────────────────────────────────────────────────────┘
        ↓
┌─────────────────────────────────────────────────────────────────┐
│                 Coordination Check                              │
│                    协调检查                                      │
│  • Check action conflicts / 检查动作冲突                        │
│  • Apply coordination mechanism / 应用协调机制                  │
│  • Resolve conflicts if any / 解决冲突（如有）                  │
└─────────────────────────────────────────────────────────────────┘
        ↓
┌─────────────────────────────────────────────────────────────────┐
│                Environment Interaction                          │
│                    环境交互                                      │
│  • Execute joint actions A^t = {a_1^t, a_2^t, a_3^t, a_4^t}   │
│  • Environment step: s^{t+1}, r^t, done = env.step(A^t)       │
└─────────────────────────────────────────────────────────────────┘
        ↓
┌─────────────────────────────────────────────────────────────────┐
│                  Reward Calculation                             │
│                     奖励计算                                     │
│  Individual rewards: r_i^t = f_i(s^t, a_i^t, s^{t+1})         │
│  Cooperative bonus: r_coop^t = g(A^t, s^t)                    │
│  Total reward: R_i^t = r_i^t + λ·r_coop^t                     │
└─────────────────────────────────────────────────────────────────┘
        ↓
┌─────────────────────────────────────────────────────────────────┐
│                Experience Storage                               │
│                    经验存储                                      │
│  Store (s_i^t, a_i^t, R_i^t, s_i^{t+1}, done) in buffer_i    │
└─────────────────────────────────────────────────────────────────┘
        ↓
      Episode Done? / 回合结束？
        ↓ No
    [Return to Observation Collection]
        ↓ Yes
┌─────────────────────────────────────────────────────────────────┐
│                   Policy Update                                 │
│                    策略更新                                      │
│  For each agent i:                                             │
│  1. Calculate advantages: A_i^t = R_i^t - V_i(s_i^t)          │
│  2. Policy loss: L_π = -min(r_t(θ)A_t, clip(r_t(θ))A_t)      │
│  3. Value loss: L_V = (V_i(s_i^t) - R_i^t)²                   │
│  4. Update networks: θ_i ← θ_i - α∇(L_π + L_V)                │
└─────────────────────────────────────────────────────────────────┘
        ↓
    Training Complete? / 训练完成？
        ↓ No
    [Return to Environment Reset]
        ↓ Yes
      End Training / 结束训练
```

## 3. 实验结果对比图设计

### 3.1 收获预测准确性对比图 (Figure 4)

**设计说明**:
- 柱状图显示不同方法的RMSE、MAE、MAPE指标
- 使用色盲友好配色：蓝色(Multi-Agent PPO)、橙色(Single PPO)、绿色(PID)、红色(Manual)
- 添加误差棒显示置信区间
- 双语标注

```
Yield Prediction Accuracy Comparison
收获预测准确性对比

    RMSE (kg/m²)     MAE (kg/m²)      MAPE (%)
        ↑               ↑               ↑
    2.5 |           2.0 |           15 |
        |               |              |
    2.0 |           1.5 |           12 |
        |               |              |
    1.5 |  ████        1.0 |  ████      9 |  ████
        |  ████           |  ████         |  ████
    1.0 |  ████  ████  0.5 |  ████  ████  6 |  ████  ████
        |  ████  ████     |  ████  ████     |  ████  ████  ████
    0.5 |  ████  ████  ████|  ████  ████  ████|  ████  ████  ████  ████
        |  ████  ████  ████|  ████  ████  ████|  ████  ████  ████  ████
    0.0 |____________________|____________________|____________________
         MA-PPO Single PID Manual  MA-PPO Single PID Manual  MA-PPO Single PID Manual

图例 Legend:
████ Multi-Agent PPO (本方法)
████ Single Agent PPO (单智能体PPO)
████ PID Control (PID控制)
████ Manual Control (人工控制)
```

### 3.2 训练曲线图 (Figure 5)

**设计说明**:
- 双Y轴图：左轴显示累积奖励，右轴显示损失函数
- 平滑曲线显示训练趋势
- 阴影区域显示标准差

```
Training Curves / 训练曲线

Cumulative Reward                                    Policy Loss
累积奖励        ↑                                         ↑ 策略损失
    1000        |                                    0.5 |
                |        ╭─────────────────────────      |
     800        |      ╱                               0.4 |
                |    ╱                                    |
     600        |  ╱                                   0.3 |     ╲
                |╱                                        |      ╲
     400        |                                      0.2 |       ╲╲
                |                                         |         ╲╲
     200        |                                      0.1 |          ╲╲____
                |                                         |               ╲____
       0        |_________________________________________|_____________________╲____
                0    2000   4000   6000   8000  10000    0    2000   4000   6000   8000  10000
                        Training Episodes / 训练回合数

━━━ Multi-Agent PPO Reward / 多智能体PPO奖励
─ ─ Single Agent PPO Reward / 单智能体PPO奖励
••• Multi-Agent PPO Loss / 多智能体PPO损失
--- Single Agent PPO Loss / 单智能体PPO损失
```

## 4. 智能体行为分析图设计

### 4.1 智能体决策轨迹图 (Figure 6)

```
Agent Decision Trajectories Over Growth Cycle
生长周期内智能体决策轨迹

Action Value / 动作值
    1.0 ↑
        |
    0.5 |    Temperature Agent / 温度智能体
        |    ～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～
    0.0 |____________________________________________________
        |
   -0.5 |         Humidity Agent / 湿度智能体
        |         ▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲▲
   -1.0 |____________________________________________________

    1.0 ↑
        |
    0.5 |              Irrigation Agent / 灌溉智能体
        |              ████████████████████████████████████
    0.0 |____________________________________________________
        |
   -0.5 |
        |                   Light Agent / 光照智能体
   -1.0 |                   ●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●●
        |____________________________________________________
        0    20    40    60    80   100   120
                    Days After Planting / 种植后天数

Growth Stages / 生长阶段:
├─ Sprouting ─┤─ Vegetative ─┤─ Tuber Formation ─┤─ Maturation ─┤
   发芽期        营养生长期       块茎形成期          成熟期
```

### 4.2 环境参数变化热图 (Figure 7)

```
Environment Parameter Heatmap / 环境参数热图

Time (Hours) / 时间(小时)
 0  ┌─────────────────────────────────────────────────────────┐
    │ Temperature (°C) / 温度                                  │
 6  │ ████████████████████████████████████████████████████████ │ 25
    │ ████████████████████████████████████████████████████████ │
12  │ ████████████████████████████████████████████████████████ │ 20
    │ ████████████████████████████████████████████████████████ │
18  │ ████████████████████████████████████████████████████████ │ 15
    │ ████████████████████████████████████████████████████████ │
24  └─────────────────────────────────────────────────────────┘ 10

 0  ┌─────────────────────────────────────────────────────────┐
    │ Humidity (%) / 湿度                                      │
 6  │ ████████████████████████████████████████████████████████ │ 80
    │ ████████████████████████████████████████████████████████ │
12  │ ████████████████████████████████████████████████████████ │ 60
    │ ████████████████████████████████████████████████████████ │
18  │ ████████████████████████████████████████████████████████ │ 40
    │ ████████████████████████████████████████████████████████ │
24  └─────────────────────────────────────────────────────────┘ 20
    0    20    40    60    80   100   120
              Days After Planting / 种植后天数

Color Scale / 颜色标度:
████ High / 高    ████ Medium / 中    ████ Low / 低
```

## 5. 实验设置图设计

### 5.1 温室布局和传感器分布图 (Figure 8)

```
Greenhouse Layout and Sensor Distribution
温室布局和传感器分布

    10m
    ←→
┌─────────────────────────────────────────────────────────────┐ ↑
│  T₁H₁L₁     T₂H₂L₂     T₃H₃L₃     T₄H₄L₄     T₅H₅L₅      │ │
│    │          │          │          │          │         │ │
│  🥔🥔🥔     🥔🥔🥔     🥔🥔🥔     🥔🥔🥔     🥔🥔🥔      │ │
│  🥔🥔🥔     🥔🥔🥔     🥔🥔🥔     🥔🥔🥔     🥔🥔🥔      │ │
│    │          │          │          │          │         │ │
│  S₁C₁       S₂C₂       S₃C₃       S₄C₄       S₅C₅       │ │ 20m
│                                                          │ │
│  T₆H₆L₆     T₇H₇L₇     T₈H₈L₈     T₉H₉L₉     T₁₀H₁₀L₁₀  │ │
│    │          │          │          │          │         │ │
│  🥔🥔🥔     🥔🥔🥔     🥔🥔🥔     🥔🥔🥔     🥔🥔🥔      │ │
│  🥔🥔🥔     🥔🥔🥔     🥔🥔🥔     🥔🥔🥔     🥔🥔🥔      │ │
│    │          │          │          │          │         │ │
│  S₆C₆       S₇C₇       S₈C₈       S₉C₉       S₁₀C₁₀      │ │
└─────────────────────────────────────────────────────────────┘ ↓

图例 Legend:
T - Temperature Sensor / 温度传感器
H - Humidity Sensor / 湿度传感器
L - Light Sensor / 光照传感器
S - Soil Moisture Sensor / 土壤湿度传感器
C - CO2 Sensor / CO2传感器
🥔 - Potato Plants / 马铃薯植株

Control Equipment / 控制设备:
┌─────────────────────────────────────────────────────────────┐
│ ◊ Heating/Cooling Units / 加热制冷设备                       │
│ ◈ Humidification/Dehumidification / 加湿除湿设备             │
│ ◉ LED Light Arrays / LED灯阵列                              │
│ ◎ Irrigation Nozzles / 灌溉喷头                             │
│ ◐ Ventilation Fans / 通风扇                                 │
└─────────────────────────────────────────────────────────────┘
```

### 5.2 实验对照组设计图 (Figure 9)

```
Experimental Design and Control Groups
实验设计和对照组

┌─────────────────────────────────────────────────────────────────────────────┐
│                          Experimental Setup                                │
│                             实验设置                                        │
├─────────────────────────────────────────────────────────────────────────────┤
│  Control Group 1: Traditional PID Control / 对照组1：传统PID控制            │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐           │
│  │Greenhouse A1│ │Greenhouse A2│ │Greenhouse A3│ │Greenhouse A4│           │
│  │   温室A1    │ │   温室A2    │ │   温室A3    │ │   温室A4    │           │
│  └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘           │
├─────────────────────────────────────────────────────────────────────────────┤
│  Control Group 2: Single Agent PPO / 对照组2：单智能体PPO                   │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐           │
│  │Greenhouse B1│ │Greenhouse B2│ │Greenhouse B3│ │Greenhouse B4│           │
│  │   温室B1    │ │   温室B2    │ │   温室B3    │ │   温室B4    │           │
│  └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘           │
├─────────────────────────────────────────────────────────────────────────────┤
│  Control Group 3: Manual Expert Control / 对照组3：人工专家控制             │
│  ┌─────────────┐ ┌─────────────┐                                           │
│  │Greenhouse C1│ │Greenhouse C2│                                           │
│  │   温室C1    │ │   温室C2    │                                           │
│  └─────────────┘ └─────────────┘                                           │
├─────────────────────────────────────────────────────────────────────────────┤
│  Experimental Group: Multi-Agent PPO / 实验组：多智能体PPO                  │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐           │
│  │Greenhouse D1│ │Greenhouse D2│ │Greenhouse D3│ │Greenhouse D4│           │
│  │   温室D1    │ │   温室D2    │ │   温室D3    │ │   温室D4    │           │
│  └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘           │
└─────────────────────────────────────────────────────────────────────────────┘

Evaluation Metrics / 评估指标:
┌─────────────────────────────────────────────────────────────────────────────┐
│ • Yield Prediction Accuracy (RMSE, MAE, MAPE) / 收获预测准确性              │
│ • Energy Consumption (kWh/m²) / 能耗 (千瓦时/平方米)                        │
│ • Water Usage Efficiency (L/kg) / 用水效率 (升/千克)                        │
│ • Growth Rate Optimization / 生长速率优化                                   │
│ • Environmental Control Stability / 环境控制稳定性                          │
└─────────────────────────────────────────────────────────────────────────────┘
```

## 6. LaTeX插图代码模板

```latex
% 系统架构图
\begin{figure*}[htbp]
\centering
\includegraphics[width=\textwidth]{figures/system_architecture.pdf}
\caption{Multi-Agent PPO System Architecture for Greenhouse Potato Cultivation. The system integrates environmental sensing, multi-agent decision making, actuator control, and growth monitoring to optimize potato yield prediction and environmental management.}
\label{fig:system_architecture}
\end{figure*}

% 算法流程图
\begin{figure}[htbp]
\centering
\includegraphics[width=0.8\textwidth]{figures/algorithm_flowchart.pdf}
\caption{Multi-Agent PPO Training Algorithm Flowchart. The training process includes environment interaction, coordination mechanism, reward calculation, and policy updates for all four agents.}
\label{fig:algorithm_flowchart}
\end{figure}

% 实验结果对比
\begin{figure}[htbp]
\centering
\begin{subfigure}[b]{0.48\textwidth}
    \includegraphics[width=\textwidth]{figures/accuracy_comparison.pdf}
    \caption{Prediction accuracy comparison}
    \label{fig:accuracy_comparison}
\end{subfigure}
\hfill
\begin{subfigure}[b]{0.48\textwidth}
    \includegraphics[width=\textwidth]{figures/training_curves.pdf}
    \caption{Training curves}
    \label{fig:training_curves}
\end{subfigure}
\caption{Experimental Results. (a) Comparison of yield prediction accuracy across different methods. (b) Training curves showing convergence of Multi-Agent PPO algorithm.}
\label{fig:experimental_results}
\end{figure}

% 智能体行为分析
\begin{figure}[htbp]
\centering
\includegraphics[width=\textwidth]{figures/agent_behavior.pdf}
\caption{Agent Behavior Analysis. Decision trajectories of four agents throughout the potato growth cycle, showing coordinated control strategies for different growth stages.}
\label{fig:agent_behavior}
\end{figure}

% 实验设置
\begin{figure}[htbp]
\centering
\begin{subfigure}[b]{0.48\textwidth}
    \includegraphics[width=\textwidth]{figures/greenhouse_layout.pdf}
    \caption{Greenhouse layout and sensor distribution}
    \label{fig:greenhouse_layout}
\end{subfigure}
\hfill
\begin{subfigure}[b]{0.48\textwidth}
    \includegraphics[width=\textwidth]{figures/experimental_design.pdf}
    \caption{Experimental design and control groups}
    \label{fig:experimental_design}
\end{subfigure}
\caption{Experimental Setup. (a) Layout of greenhouse facility with sensor and actuator placement. (b) Design of control groups for comparative evaluation.}
\label{fig:experimental_setup}
\end{figure}
```

## 7. 配色方案和设计规范

### 7.1 色盲友好配色方案
- **主色调**: 深蓝色 (#1f77b4) - Multi-Agent PPO
- **对比色1**: 橙色 (#ff7f0e) - Single Agent PPO
- **对比色2**: 绿色 (#2ca02c) - PID Control
- **对比色3**: 红色 (#d62728) - Manual Control
- **辅助色**: 灰色 (#7f7f7f) - 背景和网格

### 7.2 字体和标注规范
- **英文字体**: Times New Roman, 12pt
- **中文字体**: 宋体, 12pt
- **图表标题**: 14pt, 加粗
- **坐标轴标签**: 11pt
- **图例**: 10pt

### 7.3 图表尺寸规范
- **单栏图**: 宽度 8.5cm
- **双栏图**: 宽度 17.8cm
- **分辨率**: 300 DPI
- **格式**: PDF矢量格式

---

**配图设计方案完成时间**: 2026年2月9日
**设计者**: 可视化设计师
**状态**: ✅ 设计方案完成
**后续工作**: 使用专业绘图软件实现具体图表