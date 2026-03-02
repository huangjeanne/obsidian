# CNKI高级检索技能 - 快速参考

## 📋 已安装确认

✅ 技能已安装到：`~/.claude/skills/cnki-advanced-search/`
✅ SKILL.md 文件完整（229行）
✅ 系统已自动识别并加载该技能

## 🚀 立即使用

在PAI/Claude Code中说：

```
帮我在知网检索"数字化转型"相关的CSSCI论文
```

```
用知网高级检索搜索"人工智能"主题的C刊论文
```

```
检索"机器学习 + 深度学习"的CSSCI论文
```

## 📦 团队安装文件

已为你创建以下文件用于团队共享：

### 1. 团队安装包（推荐）
- **位置**: `~/.claude/skills/cnki-advanced-search-team-package.tar.gz`
- **大小**: 6.3KB
- **用途**: 分发给团队成员，快速安装

**使用方法**:
```bash
# 将tar包复制到团队成员电脑，然后执行：
bash ~/Downloads/cnki-team-setup.sh
```

### 2. 一键安装脚本
- **位置**: `~/Downloads/cnki-team-setup.sh`
- **用途**: 团队成员运行此脚本自动安装

**使用方法**:
```bash
bash ~/Downloads/cnki-team-setup.sh
```

### 3. 详细安装指南
- **位置**: `~/.claude/skills/cnki-advanced-search/TEAM_INSTALLATION.md`
- **用途**: 完整的安装文档和使用说明

## 🔧 依赖安装

技能使用Python导出Excel，需要安装：

```bash
pip3 install openpyxl --user
```

或使用虚拟环境：

```bash
python3 -m venv ~/.venv
source ~/.venv/bin/activate
pip install openpyxl
```

## 🎯 技能功能

| 功能 | 说明 |
|------|------|
| **自动检索** | 在知网高级检索页面自动执行检索操作 |
| **CSSCI筛选** | 自动勾选CSSCI来源期刊 |
| **智能排序** | 按被引量降序排列，优先获取高质量文献 |
| **批量提取** | 自动提取前100篇论文的题录和摘要 |
| **Excel导出** | 保存为格式化的Excel文件，便于整理 |

## 📝 使用流程

1. **提供关键词** → AI自动构建检索表达式（含同义词扩展）
2. **打开知网** → 自动导航到高级检索页面
3. **设置条件** → 选择学术期刊、勾选CSSCI
4. **执行检索** → 输入关键词、点击检索
5. **提取数据** → 按被引排序、切换50条/页、打开摘要视图
6. **导出Excel** → 保存到 `~/Downloads/知网检索结果_{关键词}_{日期}.xlsx`

## ⚠️ 注意事项

- 知网有反爬机制，AI会自动控制操作间隔（1-2秒）
- 如遇验证码，AI会提示你手动完成
- 检索前AI会向你确认关键词分组和同义词扩展
- 全程AI会报告进度

## 🔄 更新技能

原仓库更新时，可以重新安装：

```bash
cd ~/.claude/skills/cnki-advanced-search
bash install.sh
```

## 📚 原始资源

- **GitHub仓库**: https://github.com/yipng05-max/-skills/tree/知网高级检索skill
- **作者**: yipng05-max

## 👥 团队协作建议

1. **共享检索策略**: 制定团队统一的关键词同义词表
2. **共享检索结果**: 将导出的Excel存放在团队共享目录
3. **定期同步**: 关注原仓库更新，及时同步优化

---

**安装日期**: 2026-02-11
**版本**: v1.0
**适用PAI版本**: v2.3+
