# 知网高级检索技能 - 团队安装指南

## 技能简介

**CNKI高级检索技能**是一个自动化工具，可以：
- 在知网自动检索CSSCI/C刊论文
- 按被引量排序获取高质量文献
- 自动提取前100篇论文的题录和摘要
- 导出为Excel文件便于整理

## 适用场景

- 文献综述前期调研
- 课题申报前的文献梳理
- 论文写作的相关研究检索
- 按关键词追踪前沿研究

## 安装方法

### 方法一：从GitHub直接安装（推荐）

```bash
# 1. 克隆仓库到临时目录
git clone https://github.com/yipng05-max/-skills.git /tmp/cnki-temp
cd /tmp/cnki-temp

# 2. 切换到知网技能分支
git checkout 知网高级检索skill

# 3. 解压skill文件（这是一个zip格式）
mkdir -p cnki-extracted
cd cnki-extracted
unzip ../cnki-advanced-search.skill

# 4. 安装到PAI系统
mkdir -p ~/.claude/skills/cnki-advanced-search
cp -r cnki-advanced-search/* ~/.claude/skills/cnki-advanced-search/

# 5. 清理临时文件
cd ~
rm -rf /tmp/cnki-temp

# 6. 验证安装
ls ~/.claude/skills/cnki-advanced-search/
# 应该看到 SKILL.md 文件
```

### 方法二：从团队成员复制安装

如果团队中已有人安装完成，可以直接复制：

```bash
# 从已安装的成员处复制（假设共享文件夹路径）
cp -r /shared/path/cnki-advanced-search ~/.claude/skills/

# 或者使用scp从远程服务器复制
scp user@server:/path/to/cnki-advanced-search ~/.claude/skills/
```

### 方法三：手动创建

如果上述方法都不行，可以手动创建：

```bash
# 1. 创建技能目录
mkdir -p ~/.claude/skills/cnki-advanced-search

# 2. 创建SKILL.md文件
nano ~/.claude/skills/cniki-advanced-search/SKILL.md

# 3. 从GitHub复制内容：
#    https://github.com/yipng05-max/-skills/tree/%E7%9F%A5%E7%BD%91%E9%AB%98%E7%BA%A7%E6%A3%80%E7%B4%A2skill
#    下载 cnki-advanced-search.skill，解压后复制 SKILL.md 内容
```

## 验证安装

在PAI/Claude Code中测试：

```
我需要在知网检索"数字化转型"相关的CSSCI论文
```

或者：

```
用知网高级检索搜索"人工智能 医疗"主题的C刊论文
```

如果AI开始执行检索流程，说明安装成功！

## 依赖安装

该技能使用Python导出Excel，需要安装：

```bash
pip3 install openpyxl
```

## 使用示例

### 基础检索
```
帮我在知网检索"机器学习"相关论文
```

### 多关键词检索
```
检索"数字化转型与企业绩效"的CSSCI论文
```

### 同义词扩展
```
用知网高级检索"深度学习 + 深度神经网络"的相关研究
```

## 团队协作建议

1. **统一检索策略**：团队共同制定关键词同义词表，提高检索一致性
2. **共享检索结果**：将导出的Excel文件存放在团队共享目录
3. **定期更新技能**：关注原GitHub仓库的更新，及时同步优化

## 原始仓库

https://github.com/yipng05-max/-skills/tree/知网高级检索skill

## 技术支持

如遇问题，请检查：
1. PAI系统路径是否正确（~/.claude/skills/）
2. SKILL.md文件是否完整
3. Python openpyxl库是否已安装
4. 网络连接是否正常（访问知网）

---

**安装日期**：2026-02-11
**版本**：v1.0
**适用PAI版本**：v2.3+
