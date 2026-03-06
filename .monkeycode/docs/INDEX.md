# 掼蛋纸牌游戏文档

本文档为掼蛋纸牌游戏提供完整的项目说明、架构设计、接口定义和开发指南。

**快速链接**: [架构](./ARCHITECTURE.md) | [接口](./INTERFACES.md) | [开发者指南](./DEVELOPER_GUIDE.md)

---

## 项目概述

掼蛋纸牌游戏是一个单机版网页应用,实现完整的掼蛋游戏规则,支持1个人类玩家对战3个AI对手。游戏使用两副扑克牌(共108张),分为两队对抗,每队2人。

**目标用户**: 掼蛋游戏爱好者、想要学习掼蛋规则的新手玩家、休闲娱乐用户

**核心价值**:
- 完整实现掼蛋游戏规则,包括多种牌型识别和比较
- 提供3个具有基本策略的AI对手
- 流畅的卡牌动画效果和音效反馈
- 详细的游戏帮助和规则说明系统
- 支持游戏存档和读档功能
- 响应式设计,适配桌面和移动端

---

## 核心文档

### [架构设计](./ARCHITECTURE.md)
系统设计、技术栈、组件结构和数据流程。从这里开始了解系统如何运作。

**包含内容**:
- 系统概述和技术栈
- 项目结构和子系统划分
- 系统架构图和流程图
- 关键技术决策
- 性能和安全性考虑

### [接口定义](./INTERFACES.md)
公开API、组件接口、游戏引擎接口。集成或使用此系统的参考。

**包含内容**:
- 组件API文档
- 游戏引擎API
- AI决策API
- 存储API
- Redux Store API
- 类型定义

### [开发者指南](./DEVELOPER_GUIDE.md)
环境搭建、开发工作流、编码规范和常见任务。贡献者必读。

**包含内容**:
- 环境搭建和快速开始
- 开发工作流和代码质量工具
- 常见开发任务指南
- 编码规范和最佳实践
- 构建与发布流程
- 故障排除和调试技巧

---

## 核心概念

理解这些领域概念有助于导航代码库:

| 概念 | 描述 |
|------|------|
| 卡牌 (Card) | 游戏的基本单位,包含花色、点数、是否为主牌等属性 |
| 牌型 (CardType) | 卡牌组合的类型,如单张、对子、顺子、炸弹等 |
| 玩家 (Player) | 游戏参与者,包含手牌、队伍、位置等信息 |
| 游戏状态 (GameState) | 游戏的完整状态,包括玩家、回合、出牌记录等 |
| AI决策 (AIDecision) | AI玩家的决策结果,包含出牌或过牌的选择 |

---

## 技术栈

**前端框架**
- React 18+ - UI框架
- TypeScript - 类型安全
- Redux Toolkit - 状态管理

**UI组件与动画**
- Material-UI (MUI) - UI组件库
- Framer Motion - 动画效果

**构建工具**
- Vite - 构建工具和开发服务器
- ESLint - 代码检查
- Prettier - 代码格式化

**测试框架**
- Vitest - 单元测试
- React Testing Library - 组件测试
- Playwright - 端到端测试

---

## 入门指南

### 项目新人?

按此路径学习:
1. **[架构](./ARCHITECTURE.md)** - 了解全局
2. **[核心概念](#核心概念)** - 学习领域术语
3. **[开发者指南](./DEVELOPER_GUIDE.md)** - 搭建环境
4. **[接口](./INTERFACES.md)** - 探索公开API

### 首次开发?

1. **[开发者指南](./DEVELOPER_GUIDE.md)** - 搭建和工作流
2. **[编码规范](./DEVELOPER_GUIDE.md#编码规范)** - 代码风格
3. **[常见任务](./DEVELOPER_GUIDE.md#常见任务)** - 分步指南

### 想了解规则?

查看游戏内的帮助系统:
- 游戏中按 `H` 键打开帮助
- 阅读详细规则说明
- 查看牌型示例和动画

---

## 快速参考

### 命令

```bash
npm run dev          # 启动开发服务器
npm run test         # 运行测试
npm run build        # 生产构建
npm run lint         # 检查代码风格
npm run format       # 格式化代码
npm run typecheck    # TypeScript类型检查
```

### 键盘快捷键

| 快捷键 | 功能 |
|--------|------|
| Space | 出牌 |
| P | 过牌 |
| H | 帮助 |
| Esc | 取消选择 |
| S | 存档 |
| L | 读档 |

### 重要文件

| 文件 | 目的 |
|------|------|
| `src/main.tsx` | 应用入口 |
| `src/App.tsx` | 主应用组件 |
| `src/store/index.ts` | Redux store配置 |
| `package.json` | 依赖和脚本 |
| `.env.example` | 环境变量模板 |

---

## 项目结构

```
guandan-card-game/
├── src/
│   ├── components/      # React组件
│   ├── store/          # Redux状态管理
│   ├── game/           # 游戏核心逻辑
│   │   ├── engine/     # 规则引擎
│   │   ├── ai/         # AI决策系统
│   │   └── utils/      # 工具函数
│   ├── hooks/          # React Hooks
│   ├── types/          # TypeScript类型定义
│   └── utils/          # 通用工具
├── public/             # 静态资源
├── tests/              # 测试文件
└── .monkeycode/        # 项目文档
```

---

## 开发进度

- [x] 需求分析
- [x] 技术方案设计
- [ ] 项目初始化
- [ ] 基础组件开发
- [ ] 游戏逻辑实现
- [ ] AI系统开发
- [ ] 界面完善
- [ ] 测试优化

---

## 联系方式

如有问题或建议,请通过以下方式联系:

- 提交Issue: [GitHub Issues](https://github.com/your-org/guandan-card-game/issues)
- 发起讨论: [GitHub Discussions](https://github.com/your-org/guandan-card-game/discussions)
