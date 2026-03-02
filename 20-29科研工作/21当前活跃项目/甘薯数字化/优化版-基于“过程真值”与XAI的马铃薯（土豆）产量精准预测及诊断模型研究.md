# 优化版-基于“过程真值”与XAI的马铃薯（土豆）产量精准预测及诊断模型研究

#### **研究总目标**

构建一个基于“单一品种、统一农事、主副地块验证”的高频多模态数据的AI模型，实现：

1. **高精度末期预测：** 在收获前1-2周，精准预测单位面积总产量（公斤/亩），主地块内部验证 `MAPE ≤ 8%`，副地块外部验证 `MAPE ≤ 15%`（允许泛化能力略有下降）。
2. **可解释智能诊断：** 利用XAI技术，量化分析并诊断影响主地块产量的关键限制因子及其发生阶段。
3. **区域泛化能力初步评估：** 利用副地块数据验证模型在同一区域内相似条件下的预测稳定性。

---

#### **整体试验设计思路图**

```svg
<svg width="800" height="450" xmlns="http://www.w3.org/2000/svg">
    <defs>
        <marker id="arrowhead" markerWidth="10" markerHeight="7" refX="0" refY="3.5" orient="auto">
            <polygon points="0 0, 10 3.5, 0 7" fill="#333" />
        </marker>
    </defs>

    <rect x="20" y="20" width="160" height="50" rx="5" ry="5" fill="#f9f" stroke="#333" stroke-width="2"/>
    <text x="100" y="50" font-family="Arial" font-size="14" text-anchor="middle">1. 试验准备与布设</text>

    <rect x="20" y="100" width="160" height="100" rx="5" ry="5" fill="#ccf" stroke="#333" stroke-width="2"/>
    <text x="100" y="120" font-family="Arial" font-size="14" text-anchor="middle">2a. 主地块(P1)数据</text>
    <text x="100" y="140" font-family="Arial" font-size="12" text-anchor="middle">高频监测 (无人机/环境/病虫害)</text>
    <text x="100" y="160" font-family="Arial" font-size="12" text-anchor="middle" font-weight="bold" fill="#d00">高频破坏性采样 (12+节点)</text>
    <text x="100" y="180" font-family="Arial" font-size="12" text-anchor="middle">(LAI, 阶段产量 - 过程真值)</text>

    <rect x="200" y="100" width="160" height="100" rx="5" ry="5" fill="#ccf" stroke="#333" stroke-width="2"/>
    <text x="280" y="120" font-family="Arial" font-size="14" text-anchor="middle">2b. 副地块(S1,S2)数据</text>
    <text x="280" y="140" font-family="Arial" font-size="12" text-anchor="middle">高频监测 (无人机/环境/病虫害)</text>
    <text x="280" y="160" font-family="Arial" font-size="12" text-anchor="middle" font-weight="bold" fill="#00d">仅最终收获测产</text>
    <text x="280" y="180" font-family="Arial" font-size="12" text-anchor="middle">(Y_final)</text>

    <rect x="400" y="125" width="160" height="50" rx="5" ry="5" fill="#ffc" stroke="#333" stroke-width="2"/>
    <text x="480" y="155" font-family="Arial" font-size="14" text-anchor="middle">3. 多模态数据库</text>

    <rect x="600" y="125" width="160" height="50" rx="5" ry="5" fill="#ddd" stroke="#333" stroke-width="2"/>
    <text x="680" y="155" font-family="Arial" font-size="14" text-anchor="middle">4. 特征工程</text>

    <rect x="400" y="230" width="160" height="50" rx="5" ry="5" fill="#cfc" stroke="#333" stroke-width="2"/>
    <text x="480" y="260" font-family="Arial" font-size="14" text-anchor="middle">5. AI模型训练 (用P1数据)</text>

    <rect x="600" y="230" width="160" height="50" rx="5" ry="5" fill="#fdd" stroke="#333" stroke-width="2" stroke-dasharray="5 5"/>
    <text x="680" y="260" font-family="Arial" font-size="14" text-anchor="middle">6. 模型验证 (用S1,S2数据)</text>

    <rect x="400" y="310" width="160" height="50" rx="5" ry="5" fill="#fdd" stroke="#333" stroke-width="2" stroke-dasharray="5 5"/>
    <text x="480" y="340" font-family="Arial" font-size="14" text-anchor="middle">7. XAI诊断模型 (基于P1)</text>

    <rect x="600" y="310" width="160" height="50" rx="5" ry="5" fill="#ccf" stroke="#333" stroke-width="2"/>
    <text x="680" y="340" font-family="Arial" font-size="14" text-anchor="middle">8. 结果输出与决策</text>

    <line x1="100" y1="70" x2="100" y2="100" stroke="#333" stroke-width="1.5" marker-end="url(#arrowhead)" />
    <line x1="180" y1="150" x2="200" y2="150" stroke="#333" stroke-width="1.5" marker-end="url(#arrowhead)" />
    <line x1="100" y1="200" x2="100" y2="220" stroke="#333" stroke-width="1.5"/>
    <line x1="100" y1="220" x2="400" y2="150" stroke="#333" stroke-width="1.5" marker-end="url(#arrowhead)" />
    <line x1="280" y1="200" x2="280" y2="220" stroke="#333" stroke-width="1.5"/>
    <line x1="280" y1="220" x2="400" y2="150" stroke="#333" stroke-width="1.5" marker-end="url(#arrowhead)" />
    <line x1="560" y1="150" x2="600" y2="150" stroke="#333" stroke-width="1.5" marker-end="url(#arrowhead)" />
    <line x1="480" y1="175" x2="480" y2="230" stroke="#333" stroke-width="1.5" marker-end="url(#arrowhead)" />
    <line x1="680" y1="175" x2="680" y2="230" stroke="#333" stroke-width="1.5" marker-end="url(#arrowhead)" />
    <line x1="560" y1="255" x2="600" y2="255" stroke="#333" stroke-width="1.5" marker-end="url(#arrowhead)" />
    <line x1="480" y1="280" x2="480" y2="310" stroke="#333" stroke-width="1.5" marker-end="url(#arrowhead)" />
    <line x1="560" y1="335" x2="600" y2="335" stroke="#333" stroke-width="1.5" marker-end="url(#arrowhead)" />
    <line x1="680" y1="280" x2="680" y2="310" stroke="#333" stroke-width="1.5" marker-end="url(#arrowhead)" />

    <rect x="18" y="98" width="164" height="104" fill="none" stroke="#d00" stroke-width="1.5" stroke-dasharray="3 3"/>
    <rect x="398" y="308" width="164" height="54" fill="none" stroke="#d00" stroke-width="1.5" stroke-dasharray="3 3"/>
    <rect x="598" y="228" width="164" height="54" fill="none" stroke="#00d" stroke-width="1.5" stroke-dasharray="3 3"/>

    <text x="400" y="420" font-family="Arial" font-size="14" text-anchor="middle">图1: 整体试验设计思路图 (主副地块验证)</text>
</svg>
```

---

#### **一、 试验准备与布设阶段 (T₀ - T\_播种后)**

**目标：** 建立标准化的、可重复的、主副分工明确的试验基础。

1. **地块选择 (T₀ - 2周)：**

   - 选择**同一区域内**、**土壤类型和肥力相似**、**地形条件相近**的**3-4块**地块。
   - 确定**1块为主地块 (P1)**，面积1-3亩。
   - 确定**2-3块为副地块 (S1, S2, ...)**，面积1-3亩。
   - **记录：** 所有地块的GPS边界、历史信息、详细土壤理化性质（多点取样分析）。

2. **品种与农事操作确定 (T₀)：**

   - 选择**单一马铃薯品种**。
   - 制定**完全统一**的全生育期农事操作规程（整地、播种密度/深度、灌溉制度、施肥方案（种类、时期、用量）、病虫草害防治方案等）。**这是确保不同地块间可比性的关键**。

3. **样方规划与标记 (T₀)：**

   - **主地块 (P1)：**
     - 规划**N≥12**个破坏性采样节点。
     - 每个节点设**R≥3**个重复样方 (1m x 1m)。
     - 规划**F≥5**个最终收获测产样方 (1m x 1m)。
     - 总样方数 = `(N*R) + F`。
     - **使用RTK-GPS精确定位并标记所有样方**，建立ID与坐标对应表（如 `P1-T1R1`, `P1-F1`）。
   - **副地块 (S1, S2, ...):**
     - **不设置**破坏性采样样方。
     - 只规划**F≥5**个最终收获测产样方 (1m x 1m)。
     - **同样使用RTK-GPS精确定位并标记**，建立ID与坐标对应表（如 `S1-F1`, `S2-F1`）。
   - **布点原则：** 所有样方均需随机或系统布点，避开边缘效应。
   - **可视化示意图 (图2):**

```svg
<svg width="600" height="300" xmlns="http://www.w3.org/2000/svg">
    <style>
        .plot-label { font-family: Arial; font-size: 14px; text-anchor: middle; }
        .sample-label { font-family: Arial; font-size: 10px; text-anchor: middle; }
    </style>
    <rect x="20" y="20" width="260" height="200" rx="5" ry="5" fill="#fdd" stroke="#333" stroke-width="1"/>
    <text x="150" y="40" class="plot-label">主地块 (P1)</text>
    <circle cx="60" cy="70" r="3" fill="red"/> <text x="60" y="85" class="sample-label">T1R1</text>
    <circle cx="100" cy="90" r="3" fill="red"/> <text x="100" y="105" class="sample-label">T2R1</text>
    <circle cx="140" cy="70" r="3" fill="red"/> <text x="140" y="85" class="sample-label">...</text>
    <circle cx="180" cy="90" r="3" fill="red"/> <text x="180" y="105" class="sample-label">T13R1</text>
    <circle cx="220" cy="70" r="3" fill="red"/>
    <circle cx="70" cy="130" r="3" fill="red"/> <text x="70" y="145" class="sample-label">T1R2</text>
    <circle cx="110" cy="150" r="3" fill="red"/> <text x="110" y="165" class="sample-label">T2R2</text>
    <circle cx="150" cy="130" r="3" fill="red"/> <text x="150" y="145" class="sample-label">...</text>
    <circle cx="190" cy="150" r="3" fill="red"/> <text x="190" y="165" class="sample-label">T13R3</text>
    <circle cx="60" cy="190" r="3" fill="blue"/> <text x="60" y="205" class="sample-label">P1-F1</text>
    <circle cx="100" cy="190" r="3" fill="blue"/> <text x="100" y="205" class="sample-label">P1-F2</text>
    <circle cx="140" cy="190" r="3" fill="blue"/> <text x="140" y="205" class="sample-label">...</text>
    <circle cx="180" cy="190" r="3" fill="blue"/> <text x="180" y="205" class="sample-label">P1-F5</text>
    <text x="150" y="230" class="plot-label" font-size="12">红点: 破坏性采样点; 蓝点: 最终测产点</text>

    <rect x="320" y="20" width="260" height="200" rx="5" ry="5" fill="#ccf" stroke="#333" stroke-width="1"/>
    <text x="450" y="40" class="plot-label">副地块 (S1)</text>
    <circle cx="360" cy="190" r="3" fill="blue"/> <text x="360" y="205" class="sample-label">S1-F1</text>
    <circle cx="400" cy="190" r="3" fill="blue"/> <text x="400" y="205" class="sample-label">S1-F2</text>
    <circle cx="440" cy="190" r="3" fill="blue"/> <text x="440" y="205" class="sample-label">...</text>
    <circle cx="480" cy="190" r="3" fill="blue"/> <text x="480" y="205" class="sample-label">S1-F5</text>
     <text x="450" y="230" class="plot-label" font-size="12">蓝点: 最终测产点</text>

    <text x="300" y="270" font-family="Arial" font-size="14" text-anchor="middle">图2: 主副地块样方布设示意图</text>
</svg>
```

4. **传感器布设与校准 (T₀ - T\_播种后1周)：**

   - 在**主地块 (P1)** 和 **所有副地块 (S1, S2, ...)** 内部，均布设**至少3个**监测点的土壤传感器（温湿度、EC值，多深度）。**确保所有地块的环境监测标准一致。**
   - 确保所有地块都能获取**相同来源**的精确气象数据。
   - 无人机与相机准备同上。

5. **制定SOPs (T₀)：**

   - 制定涵盖所有地块（区分主副地块操作差异）、所有步骤的SOP。**尤其强调主副地块除破坏性采样外的所有操作完全一致。**

6. **播种与统一管理 (T\_播种)：**

   - 在**所有**地块上，**同一天**或尽可能接近的时间，使用**相同批次**的种薯，按照**完全相同**的密度、深度、方式进行播种。
   - **严格执行统一的农事操作规程**，确保所有地块的管理一致性。详细记录所有操作。

---

#### **二、 全生育周期数据采集阶段 (T\_出苗 - T\_收获)**

**目标：** 获取主地块高频过程真值，以及所有地块一致的高频监测数据和最终产量数据。

**（一） 高频常规（非破坏性）监测 (覆盖所有地块 P1, S1, S2, ...; 频率：例如每周或每10天)**

- **执行标准：** 对**所有**地块（主地块和副地块）执行**完全相同**的常规监测。
  1. **无人机遥感数据采集：** 同一架次或连续架次，使用相同参数（航线、高度、时间段、相机设置）覆盖**所有**地块，采集多光谱影像。
  2. **环境数据采集：** 同步读取**所有**地块的土壤传感器数据，获取气象数据。
  3. **病虫害定量调查：** 在**所有**地块内，采用**相同**的方法和标准进行定量调查。
- **数据记录：** 所有数据必须清晰标记来源地块（P1, S1, S2）。

**（二） 主地块 (P1) 高频破坏性采样与测量 (频率：按照预设的N≥12个节点进行)**

- **执行标准：** 严格按照方案上一版本的 **“二、（二）破坏性采样与测量”** 步骤，**仅在主地块P1** 内执行。
  - 确定采样节点 (Tn)。
  - 在P1内找到对应样方 (`P1-TnR1`, `P1-TnR2`, `P1-TnR3`)。
  - 执行现场取样（地上+地下）。
  - 进行实验室测量（LAI, 各部分鲜/干重, 块茎数, **块茎总鲜重 - 过程真值**）。
  - 数据精确录入，关联样方ID `P1-TnRx`。
- **关键：** 确保每次采样后，**及时在数据处理流程中标记该样方已被破坏**，以便在后续的无人机数据分析中进行掩膜处理。

**（三） 最终收获测产 (覆盖所有地块 P1, S1, S2, ...; T\_收获)**

- **执行标准：** 在**所有**地块（主地块和副地块）内，找到预留的最终测产样方（`P1-F1..F5`, `S1-F1..F5`, `S2-F1..F5`）。
- 使用**完全相同**的SOP 03 进行挖掘、清洗、计数、**称块茎总鲜重 (公斤/m²)**。
- 换算成各地块的最终产量标签 **Y_final (公斤/亩)**。
- 数据精确录入，关联样方ID。

**可视化示意图 (图3 - 数据流区分主副地块):**

```svg
<svg width="700" height="400" xmlns="http://www.w3.org/2000/svg">
    <defs>
        <marker id="arrowhead2" markerWidth="10" markerHeight="7" refX="0" refY="3.5" orient="auto">
            <polygon points="0 0, 10 3.5, 0 7" fill="#333" />
        </marker>
    </defs>
    <style> .label { font-family: Arial; font-size: 14px; text-anchor: middle; } </style>

    <rect x="20" y="20" width="120" height="40" rx="5" ry="5" fill="#fdd"/>
    <text x="80" y="45" class="label">主地块 (P1)</text>

    <rect x="20" y="80" width="120" height="60" rx="5" ry="5" fill="#ccf"/>
    <text x="80" y="100" class="label">常规监测</text>
    <text x="80" y="120" class="label" font-size="12">(无人机/环境/病害)</text>
    <line x1="80" y1="60" x2="80" y2="80" stroke="#333" stroke-width="1.5" marker-end="url(#arrowhead2)" />

    <rect x="20" y="160" width="120" height="80" rx="5" ry="5" fill="#fdd" stroke="#d00" stroke-width="1.5" stroke-dasharray="3 3"/>
    <text x="80" y="180" class="label">破坏性采样</text>
    <text x="80" y="200" class="label" font-size="12">(12+ 节点)</text>
    <text x="80" y="220" class="label" font-size="12">(LAI, 阶段产量)</text>
    <line x1="80" y1="140" x2="80" y2="160" stroke="#333" stroke-width="1.5" marker-end="url(#arrowhead2)" />

    <rect x="20" y="260" width="120" height="50" rx="5" ry="5" fill="#ccf"/>
    <text x="80" y="285" class="label">最终收获测产</text>
    <line x1="80" y1="240" x2="80" y2="260" stroke="#333" stroke-width="1.5" marker-end="url(#arrowhead2)" />

    <rect x="200" y="20" width="120" height="40" rx="5" ry="5" fill="#ccf"/>
    <text x="260" y="45" class="label">副地块 (S1, S2)</text>

    <rect x="200" y="80" width="120" height="60" rx="5" ry="5" fill="#ccf"/>
    <text x="260" y="100" class="label">常规监测</text>
    <text x="260" y="120" class="label" font-size="12">(无人机/环境/病害)</text>
    <line x1="260" y1="60" x2="260" y2="80" stroke="#333" stroke-width="1.5" marker-end="url(#arrowhead2)" />

    <rect x="200" y="260" width="120" height="50" rx="5" ry="5" fill="#ccf" stroke="#00d" stroke-width="1.5" stroke-dasharray="3 3"/>
    <text x="260" y="285" class="label">最终收获测产</text>
    <line x1="260" y1="140" x2="260" y2="260" stroke="#333" stroke-width="1.5" marker-end="url(#arrowhead2)" />


    <rect x="400" y="150" width="140" height="60" rx="5" ry="5" fill="#ffc"/>
    <text x="470" y="180" class="label">多模态数据库</text>
    <line x1="140" y1="110" x2="400" y2="170" stroke="#333" stroke-width="1.5" marker-end="url(#arrowhead2)" />
    <line x1="140" y1="200" x2="400" y2="180" stroke="#333" stroke-width="1.5" marker-end="url(#arrowhead2)" />
    <line x1="140" y1="285" x2="400" y2="190" stroke="#333" stroke-width="1.5" marker-end="url(#arrowhead2)" />
    <line x1="320" y1="110" x2="400" y2="170" stroke="#333" stroke-width="1.5" marker-end="url(#arrowhead2)" />
    <line x1="320" y1="285" x2="400" y2="190" stroke="#333" stroke-width="1.5" marker-end="url(#arrowhead2)" />

    <text x="350" y="370" font-family="Arial" font-size="14" text-anchor="middle">图3: 主副地块数据采集流程示意图</text>
</svg>
```

---

#### **三、 数据管理、处理与特征工程阶段**

**目标：** 整合所有数据，提取有效特征，构建用于训练和验证的数据集。 **( bổ sung )** **核心思想：** 从原始时序数据中，派生出能够描述过程动态的衍生特征。

1. **数据管理：**

   - 建立**中心数据库**，包含元数据（地块ID, 样方ID, 日期, 时间, GPS坐标, 传感器ID等）。
   - 存储所有原始数据（影像、传感器读数、调查表、测量记录）和处理后的数据。
   - 定期备份。

2. **数据处理与特征工程：**

   - **数据清洗与验证。**
   - **无人机影像处理：** 生成所有地块、所有日期的正射影像、NDVI图、CHM图。**在P1地块，对已被破坏的样方区域进行掩膜处理。**
   - **时空对齐与特征提取：**
     - 对**所有地块**提取样方级的无人机特征（NDVI, 高度）和环境、胁迫特征的时间序列。
     - **仅对主地块 (P1)** 提取生理特征（LAI, **过程真值**）。
   - **( bổ sung )** **衍生特征工程 (针对P1地块):**
     - **时序特征：** 计算关键指标（如NDVI, LAI, 块茎干重）的滚动平均值、增长率（例如，节点之间块茎干重的变化）和累计值（例如，累计生长积温）。
     - **滞后特征：** 创建代表过去状态的特征（例如，10天前的NDVI值），以捕捉生长影响的延迟效应。
   - **构建特征数据库：**
     - **训练/内部验证数据集：** 来自**主地块P1**的特征 + **P1**的最终产量标签 (Y_final)。
     - **外部验证数据集：** 来自**副地块S1, S2,...** 的特征 + **S1, S2,...** 的最终产量标签 (Y_final)。

- **可视化示意图 (图4 - 数据处理流程):**

  ![Data Preprocessing Workflow](https://api.dessix.io/v1/image/generate/380ded73-d0c6-46a4-9aa8-fb6134cc97ae)
  

---

#### **四、 AI模型构建、验证与诊断阶段**

**目标：** 建立高精度预测模型，评估其泛化能力，并实现可解释的诊断功能。

1. **模型选择：**

   - **首选模型：** **LSTM (长短期记忆网络)**。这种循环神经网络（RNN）专为学习序列数据而设计，非常适合捕捉高频数据集中的时间依赖性。
   - **备选模型：** **Transformer网络**。其注意力机制可以识别出对最终产量预测影响最大的时间步。
   - **基线模型：** XGBoost。用于对比验证深度学习模型的性能优势。

2. **模型训练与内部验证：**

   - 使用\*\*主地块P1的数据集（包含衍生特征）\*\*进行模型训练（如LSTM）和超参数调优。确保来自同一个样方的所有时间点数据保留在同一个集合中（训练集或验证集），以防数据泄露。
   - 得到在P1地块上性能最优的模型。

3. **模型外部验证 (核心步骤)：**

   - 将训练好的最优模型，应用于**副地块S1, S2,... 的数据集**进行预测。
   - 计算模型在副地块上的预测性能指标（MAPE, R², RMSE）。**这是评估模型泛化能力的关键。**

- **可视化示意图 (图5 - 模型开发流程):**

  ![Model Development Workflow](https://api.dessix.io/v1/image/generate/6e8f5f8e-332d-486a-8b03-1f37bb92a9a1)
  

4. **XAI诊断模型开发与分析：**
   - 将在P1数据上训练并验证过的最优模型（LSTM/Transformer），应用**SHAP (SHapley Additive exPlanations)** 进行分析。
   - **全局诊断：** 分析在整个生长季中，对最终产量影响最大的Top-5关键特征变量（例如，“块茎膨大期的累计光照”、“80天时的LAI”）。
   - **局部（个案）诊断：** 为P1地块内的**典型样本**（如高产样方、低产样方）生成**个体诊断瀑布图 (SHAP force plot)**，可视化每个特征值如何将预测值从基线向上或向下推动。
   - 进行农学逻辑验证。

---

#### **五、 系统集成与成果总结阶段**

**目标：** 固化研究成果，评估模型泛化性，提出应用建议。

1. **可视化系统原型开发：**

   - 开发一个简单的Web界面原型。
   - 后端托管训练好的预测模型和SHAP诊断逻辑。
   - 用户输入监测数据后，系统输出**预测产量**值，并生成一份**诊断报告**。

2. **成果总结与报告撰写：**

   - **重点分析和讨论模型在主地块（内部验证）和副地块（外部验证）上的性能差异**，评估模型的泛化能力和潜在局限性。
   - 撰写研究报告、论文、专利、软著。

3. **提出优化建议：** 基于主地块的XAI诊断结果和副地块的验证表现，为该区域、该品种在类似管理模式下的种植提供管理优化建议。

- **可视化示意图 (图6 - 系统集成与原型):**

  ![XAI Diagnostics & System Prototype](https://api.dessix.io/v1/image/generate/a1bc0fff-9205-4162-a32d-89c333f5a25a)
  

---
