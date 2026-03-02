# Multi-Agent PPO训练策略与超参数配置指南

## 1. 数据预处理方案

### 1.1 传感器数据归一化
```python
class DataPreprocessor:
    def __init__(self):
        self.normalization_params = {
            'temperature': {'min': -10, 'max': 50, 'mean': 20, 'std': 8},
            'humidity': {'min': 0, 'max': 100, 'mean': 65, 'std': 15},
            'soil_moisture': {'min': 0, 'max': 100, 'mean': 60, 'std': 12},
            'light_intensity': {'min': 0, 'max': 1000, 'mean': 400, 'std': 200},
            'co2_level': {'min': 300, 'max': 1500, 'mean': 800, 'std': 300}
        }

    def normalize(self, data, feature_name):
        params = self.normalization_params[feature_name]
        # Z-score标准化
        normalized = (data - params['mean']) / params['std']
        # 限制在[-3, 3]范围内
        return np.clip(normalized, -3, 3)

    def min_max_normalize(self, data, feature_name):
        params = self.normalization_params[feature_name]
        # Min-Max归一化到[0, 1]
        return (data - params['min']) / (params['max'] - params['min'])
```

### 1.2 时序数据处理
```python
class TimeSeriesProcessor:
    def __init__(self, window_size=24, step_size=1):
        self.window_size = window_size  # 24小时滑动窗口
        self.step_size = step_size

    def create_sequences(self, data):
        """创建时序输入序列"""
        sequences = []
        for i in range(0, len(data) - self.window_size, self.step_size):
            seq = data[i:i + self.window_size]
            sequences.append(seq)
        return np.array(sequences)

    def add_temporal_features(self, data, timestamps):
        """添加时间特征"""
        # 小时特征 (0-23)
        hour_sin = np.sin(2 * np.pi * timestamps.hour / 24)
        hour_cos = np.cos(2 * np.pi * timestamps.hour / 24)

        # 日期特征 (1-365)
        day_sin = np.sin(2 * np.pi * timestamps.dayofyear / 365)
        day_cos = np.cos(2 * np.pi * timestamps.dayofyear / 365)

        # 生长阶段特征 (0-120天)
        growth_stage = (timestamps - timestamps.min()).days / 120

        temporal_features = np.column_stack([
            hour_sin, hour_cos, day_sin, day_cos, growth_stage
        ])

        return np.concatenate([data, temporal_features], axis=1)
```

### 1.3 数据增强技术
```python
class DataAugmentation:
    def __init__(self):
        self.noise_std = 0.01
        self.time_warp_sigma = 0.2

    def add_gaussian_noise(self, data):
        """添加高斯噪声"""
        noise = np.random.normal(0, self.noise_std, data.shape)
        return data + noise

    def time_warping(self, data):
        """时间扭曲"""
        # 随机选择扭曲点
        warp_points = np.random.choice(len(data), size=3, replace=False)
        warp_points = np.sort(warp_points)

        # 应用时间扭曲
        warped_data = data.copy()
        for i in range(len(warp_points) - 1):
            start, end = warp_points[i], warp_points[i + 1]
            warp_factor = np.random.normal(1.0, self.time_warp_sigma)
            segment_length = int((end - start) * warp_factor)

            if segment_length > 0:
                indices = np.linspace(start, end, segment_length)
                warped_segment = np.interp(indices, range(start, end + 1),
                                         data[start:end + 1])
                warped_data[start:start + len(warped_segment)] = warped_segment

        return warped_data
```

## 2. Multi-Agent PPO超参数配置

### 2.1 核心超参数
```python
class PPOConfig:
    def __init__(self):
        # PPO核心参数
        self.learning_rate = 3e-4
        self.clip_ratio = 0.2
        self.entropy_coef = 0.01
        self.value_coef = 0.5
        self.max_grad_norm = 0.5

        # 训练参数
        self.batch_size = 64
        self.buffer_size = 2048
        self.update_epochs = 10
        self.gamma = 0.99  # 折扣因子
        self.gae_lambda = 0.95  # GAE参数

        # 多智能体特定参数
        self.agent_lr_ratios = {
            'temp': 1.0,
            'humid': 1.0,
            'irrig': 1.2,  # 灌溉需要更快学习
            'light': 0.8   # 光照变化相对缓慢
        }

        # 探索参数
        self.initial_std = 0.5
        self.min_std = 0.1
        self.std_decay = 0.995
```

### 2.2 学习率调度策略
```python
class LearningRateScheduler:
    def __init__(self, initial_lr=3e-4, decay_type='cosine'):
        self.initial_lr = initial_lr
        self.decay_type = decay_type

    def get_lr(self, epoch, total_epochs):
        if self.decay_type == 'cosine':
            return self.initial_lr * 0.5 * (1 + np.cos(np.pi * epoch / total_epochs))
        elif self.decay_type == 'exponential':
            return self.initial_lr * (0.95 ** epoch)
        elif self.decay_type == 'step':
            if epoch < total_epochs * 0.5:
                return self.initial_lr
            elif epoch < total_epochs * 0.8:
                return self.initial_lr * 0.1
            else:
                return self.initial_lr * 0.01
        else:
            return self.initial_lr
```

## 3. 训练技巧

### 3.1 Curriculum Learning
```python
class CurriculumLearning:
    def __init__(self):
        self.stages = [
            {'name': 'basic', 'duration': 1000, 'complexity': 0.3},
            {'name': 'intermediate', 'duration': 2000, 'complexity': 0.6},
            {'name': 'advanced', 'duration': 3000, 'complexity': 1.0}
        ]
        self.current_stage = 0
        self.episode_count = 0

    def get_environment_config(self):
        stage = self.stages[self.current_stage]

        config = {
            'weather_variability': stage['complexity'],
            'sensor_noise': stage['complexity'] * 0.1,
            'equipment_failure_rate': stage['complexity'] * 0.05,
            'pest_disease_probability': stage['complexity'] * 0.1
        }

        return config

    def update_stage(self):
        self.episode_count += 1

        if (self.current_stage < len(self.stages) - 1 and
            self.episode_count >= self.stages[self.current_stage]['duration']):
            self.current_stage += 1
            self.episode_count = 0
            print(f"升级到课程阶段: {self.stages[self.current_stage]['name']}")
```

### 3.2 Reward Shaping
```python
class RewardShaper:
    def __init__(self):
        self.weights = {
            'yield_prediction': 0.4,
            'energy_efficiency': 0.2,
            'growth_optimization': 0.2,
            'cooperation': 0.1,
            'stability': 0.1
        }

    def shape_reward(self, raw_rewards, agent_actions, global_state):
        shaped_rewards = {}

        for agent_id, raw_reward in raw_rewards.items():
            # 基础奖励
            shaped_reward = raw_reward

            # 合作奖励
            cooperation_bonus = self.calculate_cooperation_bonus(
                agent_id, agent_actions, global_state
            )
            shaped_reward += self.weights['cooperation'] * cooperation_bonus

            # 稳定性奖励
            stability_bonus = self.calculate_stability_bonus(
                agent_id, global_state
            )
            shaped_reward += self.weights['stability'] * stability_bonus

            shaped_rewards[agent_id] = shaped_reward

        return shaped_rewards

    def calculate_cooperation_bonus(self, agent_id, actions, state):
        # 计算智能体间动作的协调性
        coordination_score = 0

        if agent_id == 'temp' and 'humid' in actions:
            # 温度和湿度的协调
            temp_action = actions['temp']
            humid_action = actions['humid']

            # 如果温度升高，湿度应该相应调整
            if temp_action > 0 and humid_action < 0:
                coordination_score += 0.1

        return coordination_score
```

### 3.3 经验回放策略
```python
class PrioritizedExperienceReplay:
    def __init__(self, capacity=10000, alpha=0.6, beta=0.4):
        self.capacity = capacity
        self.alpha = alpha  # 优先级指数
        self.beta = beta    # 重要性采样指数
        self.buffer = []
        self.priorities = np.zeros(capacity)
        self.position = 0

    def add(self, experience, td_error):
        priority = (abs(td_error) + 1e-6) ** self.alpha

        if len(self.buffer) < self.capacity:
            self.buffer.append(experience)
        else:
            self.buffer[self.position] = experience

        self.priorities[self.position] = priority
        self.position = (self.position + 1) % self.capacity

    def sample(self, batch_size):
        if len(self.buffer) < batch_size:
            return None

        # 计算采样概率
        priorities = self.priorities[:len(self.buffer)]
        probabilities = priorities / priorities.sum()

        # 采样索引
        indices = np.random.choice(len(self.buffer), batch_size,
                                 p=probabilities, replace=False)

        # 计算重要性权重
        weights = (len(self.buffer) * probabilities[indices]) ** (-self.beta)
        weights = weights / weights.max()

        experiences = [self.buffer[i] for i in indices]

        return experiences, indices, weights
```

## 4. 网络架构优化

### 4.1 Actor-Critic网络设计
```python
class ImprovedPPOAgent(nn.Module):
    def __init__(self, state_dim, action_dim, hidden_dim=256):
        super().__init__()

        # 特征提取网络
        self.feature_extractor = nn.Sequential(
            nn.Linear(state_dim, hidden_dim),
            nn.LayerNorm(hidden_dim),
            nn.ReLU(),
            nn.Dropout(0.1),
            nn.Linear(hidden_dim, hidden_dim // 2),
            nn.LayerNorm(hidden_dim // 2),
            nn.ReLU()
        )

        # Actor网络 (策略网络)
        self.actor_mean = nn.Sequential(
            nn.Linear(hidden_dim // 2, hidden_dim // 4),
            nn.ReLU(),
            nn.Linear(hidden_dim // 4, action_dim),
            nn.Tanh()
        )

        self.actor_std = nn.Parameter(torch.ones(action_dim) * 0.5)

        # Critic网络 (价值网络)
        self.critic = nn.Sequential(
            nn.Linear(hidden_dim // 2, hidden_dim // 4),
            nn.ReLU(),
            nn.Linear(hidden_dim // 4, 1)
        )

        # 网络初始化
        self.apply(self._init_weights)

    def _init_weights(self, module):
        if isinstance(module, nn.Linear):
            nn.init.orthogonal_(module.weight, gain=np.sqrt(2))
            nn.init.constant_(module.bias, 0)

    def forward(self, state):
        features = self.feature_extractor(state)

        # 策略输出
        action_mean = self.actor_mean(features)
        action_std = torch.clamp(self.actor_std, min=0.1, max=1.0)

        # 价值输出
        value = self.critic(features)

        return action_mean, action_std, value
```

### 4.2 智能体间信息融合
```python
class AttentionFusionModule(nn.Module):
    def __init__(self, feature_dim, num_agents=4):
        super().__init__()
        self.feature_dim = feature_dim
        self.num_agents = num_agents

        # 注意力机制
        self.attention = nn.MultiheadAttention(
            embed_dim=feature_dim,
            num_heads=4,
            dropout=0.1
        )

        # 融合网络
        self.fusion_net = nn.Sequential(
            nn.Linear(feature_dim * num_agents, feature_dim),
            nn.ReLU(),
            nn.Linear(feature_dim, feature_dim)
        )

    def forward(self, agent_features):
        # agent_features: [num_agents, batch_size, feature_dim]

        # 自注意力
        attended_features, attention_weights = self.attention(
            agent_features, agent_features, agent_features
        )

        # 特征融合
        batch_size = agent_features.size(1)
        concatenated = attended_features.transpose(0, 1).reshape(
            batch_size, -1
        )

        fused_features = self.fusion_net(concatenated)

        return fused_features, attention_weights
```

## 5. 训练流程

### 5.1 三阶段训练策略
```python
class ThreeStageTraining:
    def __init__(self):
        self.stages = {
            'simulation': {
                'episodes': 10000,
                'environment': 'simulation',
                'learning_rate': 3e-4,
                'exploration_noise': 0.5
            },
            'small_scale': {
                'episodes': 5000,
                'environment': 'real_small',
                'learning_rate': 1e-4,
                'exploration_noise': 0.3
            },
            'large_scale': {
                'episodes': 20000,
                'environment': 'real_large',
                'learning_rate': 5e-5,
                'exploration_noise': 0.1
            }
        }

    def train_stage(self, stage_name, agents, environment):
        config = self.stages[stage_name]

        print(f"开始{stage_name}阶段训练...")

        for episode in range(config['episodes']):
            # 重置环境
            states = environment.reset()
            episode_rewards = {agent_id: 0 for agent_id in agents.keys()}

            done = False
            step = 0

            while not done and step < 1000:
                # 智能体动作选择
                actions = {}
                for agent_id, agent in agents.items():
                    action = agent.select_action(
                        states[agent_id],
                        exploration_noise=config['exploration_noise']
                    )
                    actions[agent_id] = action

                # 环境交互
                next_states, rewards, done, info = environment.step(actions)

                # 存储经验
                for agent_id, agent in agents.items():
                    agent.store_experience(
                        states[agent_id], actions[agent_id],
                        rewards[agent_id], next_states[agent_id], done
                    )
                    episode_rewards[agent_id] += rewards[agent_id]

                states = next_states
                step += 1

            # 更新策略
            if episode % 32 == 0:  # 每32个episode更新一次
                for agent_id, agent in agents.items():
                    agent.update_policy(learning_rate=config['learning_rate'])

            # 记录训练进度
            if episode % 100 == 0:
                avg_rewards = {k: v/step for k, v in episode_rewards.items()}
                print(f"Episode {episode}, 平均奖励: {avg_rewards}")
```

### 5.2 超参数调优策略
```python
class HyperparameterTuning:
    def __init__(self):
        self.search_space = {
            'learning_rate': [1e-5, 1e-4, 3e-4, 1e-3],
            'clip_ratio': [0.1, 0.2, 0.3],
            'entropy_coef': [0.001, 0.01, 0.1],
            'batch_size': [32, 64, 128],
            'hidden_dim': [128, 256, 512]
        }

    def random_search(self, num_trials=20):
        best_config = None
        best_performance = -float('inf')

        for trial in range(num_trials):
            # 随机采样超参数
            config = {}
            for param, values in self.search_space.items():
                config[param] = np.random.choice(values)

            # 训练和评估
            performance = self.train_and_evaluate(config)

            if performance > best_performance:
                best_performance = performance
                best_config = config

            print(f"Trial {trial}: {config}, Performance: {performance}")

        return best_config, best_performance
```

## 6. 性能监控和调试

### 6.1 训练监控指标
```python
class TrainingMonitor:
    def __init__(self):
        self.metrics = {
            'episode_rewards': [],
            'policy_loss': [],
            'value_loss': [],
            'entropy': [],
            'gradient_norm': [],
            'exploration_rate': []
        }

    def log_metrics(self, episode, metrics_dict):
        for key, value in metrics_dict.items():
            if key in self.metrics:
                self.metrics[key].append(value)

        # 每100个episode保存一次
        if episode % 100 == 0:
            self.save_metrics(f"metrics_episode_{episode}.json")

    def plot_training_curves(self):
        fig, axes = plt.subplots(2, 3, figsize=(15, 10))

        for i, (metric_name, values) in enumerate(self.metrics.items()):
            row, col = i // 3, i % 3
            axes[row, col].plot(values)
            axes[row, col].set_title(metric_name)
            axes[row, col].grid(True)

        plt.tight_layout()
        plt.savefig('training_curves.png')
        plt.show()
```

### 6.2 常见问题解决方案
```python
class TroubleshootingGuide:
    def __init__(self):
        self.solutions = {
            'training_not_converging': [
                "降低学习率",
                "增加batch size",
                "检查奖励函数设计",
                "使用梯度裁剪"
            ],
            'agents_conflicting': [
                "增加协作奖励权重",
                "使用中心化训练",
                "添加通信机制",
                "调整动作空间"
            ],
            'overfitting': [
                "增加dropout",
                "使用正则化",
                "减少网络复杂度",
                "增加训练数据"
            ],
            'slow_convergence': [
                "使用预训练模型",
                "调整探索策略",
                "优化网络架构",
                "使用curriculum learning"
            ]
        }

    def diagnose_problem(self, symptoms):
        recommendations = []

        for problem, solutions in self.solutions.items():
            if any(symptom in problem for symptom in symptoms):
                recommendations.extend(solutions)

        return list(set(recommendations))
```

## 7. 部署优化

### 7.1 模型压缩和加速
```python
class ModelOptimization:
    def __init__(self):
        pass

    def quantize_model(self, model):
        """模型量化"""
        quantized_model = torch.quantization.quantize_dynamic(
            model, {nn.Linear}, dtype=torch.qint8
        )
        return quantized_model

    def prune_model(self, model, pruning_ratio=0.3):
        """模型剪枝"""
        for name, module in model.named_modules():
            if isinstance(module, nn.Linear):
                torch.nn.utils.prune.l1_unstructured(
                    module, name='weight', amount=pruning_ratio
                )
        return model

    def knowledge_distillation(self, teacher_model, student_model,
                             train_loader, temperature=3.0):
        """知识蒸馏"""
        criterion = nn.KLDivLoss()
        optimizer = torch.optim.Adam(student_model.parameters())

        for epoch in range(10):
            for batch in train_loader:
                states, actions = batch

                # 教师模型输出
                with torch.no_grad():
                    teacher_logits = teacher_model(states)
                    teacher_probs = F.softmax(teacher_logits / temperature, dim=1)

                # 学生模型输出
                student_logits = student_model(states)
                student_log_probs = F.log_softmax(student_logits / temperature, dim=1)

                # 蒸馏损失
                loss = criterion(student_log_probs, teacher_probs)

                optimizer.zero_grad()
                loss.backward()
                optimizer.step()

        return student_model
```

---

**训练策略指南完成时间**: 2026年2月9日
**编写者**: 训练指导专家
**状态**: ✅ 完成
**适用阶段**: 仿真训练 → 小规模试验 → 大规模部署