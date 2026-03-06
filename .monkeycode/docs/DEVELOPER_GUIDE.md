# 掼蛋纸牌游戏开发者指南

## 项目目的

掼蛋纸牌游戏是一个单机版网页应用,实现完整的掼蛋游戏规则,支持1个人类玩家对战3个AI对手。项目旨在提供流畅的游戏体验和友好的用户界面,适合掼蛋游戏爱好者和想要学习掼蛋规则的新手玩家。

**核心职责**:
- 实现掼蛋游戏的核心规则和逻辑
- 提供3个具有基本策略的AI对手
- 实现流畅的卡牌动画效果
- 提供详细的游戏帮助和规则说明
- 支持游戏存档和读档功能

**相关系统**:
- Material-UI - UI组件库
- Redux Toolkit - 状态管理
- Framer Motion - 动画效果
- Vitest - 测试框架

## 环境搭建

### 前置条件

- Node.js >= 18.0.0
- npm >= 9.0.0 或 yarn >= 1.22.0
- Git >= 2.0.0

### 安装

```bash
# 克隆仓库
git clone https://github.com/your-org/guandan-card-game.git
cd guandan-card-game

# 安装依赖
npm install

# 配置环境
cp .env.example .env
# 编辑 .env 填入你的值
```

### 环境变量

| 变量 | 必需 | 描述 | 示例 |
|------|------|------|------|
| `VITE_API_URL` | 否 | API地址(如需网络功能) | `https://api.example.com` |
| `VITE_DEBUG` | 否 | 启用调试模式 | `true` |

⚠️ **绝不提交密钥**。使用 `.env` 文件或密钥管理器。

### 运行

```bash
# 开发服务器
npm run dev

# 生产构建
npm run build
npm run preview

# 运行测试
npm run test

# 类型检查
npm run typecheck

# 代码检查
npm run lint

# 代码格式化
npm run format
```

## 开发工作流

### 代码质量工具

| 工具 | 命令 | 目的 |
|------|------|------|
| TypeScript | `npm run typecheck` | 类型检查 |
| ESLint | `npm run lint` | 代码检查 |
| Prettier | `npm run format` | 代码格式化 |
| Vitest | `npm run test` | 单元测试 |
| Playwright | `npm run test:e2e` | 端到端测试 |

### 提交前检查

这些会在提交时自动运行:
1. TypeScript类型检查
2. ESLint代码检查
3. Prettier代码格式化
4. 单元测试

手动运行: `npm run validate`

### 分支策略

- `main` - 生产就绪代码
- `develop` - 开发分支
- `feature/*` - 新功能
- `fix/*` - Bug修复
- `docs/*` - 文档更新

### Pull Request流程

1. 从 `main` 创建功能分支
2. 编写代码和测试
3. 运行 `npm run validate`
4. 创建 PR 并填写描述
5. 处理审查反馈
6. Squash 合并

## 常见任务

### 添加新牌型

**需修改的文件**:
1. `src/game/engine/cardTypes.ts` - 添加牌型枚举
2. `src/game/engine/rules.ts` - 添加牌型识别规则
3. `src/game/engine/validator.ts` - 添加牌型验证逻辑
4. `tests/unit/cardTypes.test.ts` - 添加测试

**步骤**:
1. 在枚举中添加新牌型
2. 实现牌型识别函数
3. 实现牌型比较逻辑
4. 编写测试用例
5. 更新帮助文档

**示例提交**: `feat: add new card type`

### 优化AI策略

**需修改的文件**:
1. `src/game/ai/strategy.ts` - 策略逻辑
2. `src/game/ai/decision.ts` - 决策逻辑
3. `tests/unit/ai.test.ts` - AI测试

**步骤**:
1. 分析当前AI策略弱点
2. 设计新的策略算法
3. 实现策略逻辑
4. 测试AI表现
5. 调整参数

**示例提交**: `refactor(ai): improve decision making strategy`

### 添加新动画

**需修改的文件**:
1. `src/components/Card/index.tsx` - 卡牌动画
2. `src/hooks/useAnimation.ts` - 动画Hook
3. `src/game/constants.ts` - 动画常量

**步骤**:
1. 定义动画类型
2. 实现动画配置
3. 应用到组件
4. 测试动画效果

**示例提交**: `feat: add card deal animation`

### 添加帮助内容

**需修改的文件**:
1. `src/components/Help/HelpModal.tsx` - 帮助弹窗
2. `src/components/Help/RuleSection.tsx` - 规则章节
3. `.monkeycode/docs/HELP.md` - 帮助文档

**步骤**:
1. 编写规则内容
2. 创建示例图片/动画
3. 更新导航索引
4. 测试链接和显示

**示例提交**: `docs: add rule explanation`

### 修复Bug

**流程**:
1. 编写复现bug的失败测试
2. 在代码中定位根因
3. 用最小改动修复
4. 验证测试通过
5. 检查其他地方是否有类似问题

**示例提交**: `fix(game): handle edge case in card validation`

### 添加环境变量

**需修改的文件**:
1. `.env.example` - 添加示例值
2. `src/config/env.ts` - 添加验证
3. `.monkeycode/docs/DEVELOPER_GUIDE.md` - 文档化变量

**步骤**:
1. 在 `.env.example` 中添加占位符
2. 在配置中添加验证
3. 在本指南中文档化
4. 测试加载和使用

## 编码规范

### 文件组织

- 每个文件一个组件/类
- 文件以其默认导出命名
- 相关文件放在同一目录

### 命名

| 类型 | 约定 | 示例 |
|------|------|------|
| 文件 | kebab-case | `card-component.tsx` |
| 类/组件 | PascalCase | `CardComponent` |
| 函数 | camelCase | `getCardValue` |
| 常量 | SCREAMING_SNAKE | `MAX_PLAYERS` |
| 接口/类型 | PascalCase | `CardInterface` |

### 代码风格

```typescript
// 推荐: 使用箭头函数
const getCardValue = (card: Card): number => {
  return card.value;
};

// 推荐: 使用解构
const { id, suit, rank } = card;

// 推荐: 使用可选链
const playerName = player?.name || 'Unknown';
```

### 错误处理

```typescript
// 推荐: 特定错误类型
throw new InvalidCardError('Invalid card combination');

// 避免: 通用错误
throw new Error('Something went wrong');
```

### 日志

```typescript
// 包含上下文
logger.info('Player action', { playerId, action, timestamp });

// 使用适当级别
logger.debug();  // 开发详情
logger.info();   // 正常操作
logger.warn();   // 可恢复问题
logger.error();  // 需要关注的故障
```

### 测试

- 测试文件: `[name].test.ts` 与源码同目录
- describe 块: 匹配类/函数名
- 测试名: "should [预期行为] when [条件]"

```typescript
describe('CardValidator', () => {
  describe('validate', () => {
    it('should return valid result when cards form valid pair', () => {
      // 测试代码
    });

    it('should return invalid result when cards do not match', () => {
      // 测试代码
    });
  });
});
```

## 构建与发布

### 构建

```bash
# 开发构建
npm run build:dev

# 生产构建
npm run build:prod

# 分析构建产物
npm run build:analyze
```

### 发布

```bash
# 版本更新
npm version patch  # 小版本
npm version minor  # 中版本
npm version major  # 大版本

# 发布到npm(如需要)
npm publish

# 推送到GitHub
git push origin main --tags
```

### CI/CD

项目使用GitHub Actions进行持续集成:

```yaml
# .github/workflows/ci.yml
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
      - run: npm install
      - run: npm run validate
      - run: npm run test
```

## 故障排除

### 常见问题

**问题**: TypeScript类型错误
```bash
# 清除缓存
rm -rf node_modules/.vite
npm run typecheck
```

**问题**: 测试失败
```bash
# 运行特定测试
npm run test -- --run CardValidator

# 查看详细输出
npm run test -- --reporter=verbose
```

**问题**: 构建失败
```bash
# 检查Node版本
node --version

# 清理并重新安装
rm -rf node_modules package-lock.json
npm install
```

**问题**: 性能问题
```bash
# 分析性能
npm run build:analyze

# 检查包大小
npm run size-check
```

## 调试技巧

### Redux DevTools

1. 安装Redux DevTools浏览器扩展
2. 在开发环境中自动启用
3. 可以查看状态变化和操作历史

### React DevTools

1. 安装React DevTools浏览器扩展
2. 查看组件树和props
3. 性能分析

### 控制台日志

```typescript
// 开发环境日志
if (import.meta.env.DEV) {
  console.log('Debug info', data);
}
```

### 断点调试

在VS Code中设置断点:
```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "chrome",
      "request": "launch",
      "url": "http://localhost:5173",
      "webRoot": "${workspaceFolder}/src"
    }
  ]
}
```

## 贡献指南

### 提交规范

遵循Conventional Commits规范:

- `feat`: 新功能
- `fix`: Bug修复
- `refactor`: 代码重构
- `docs`: 文档更新
- `test`: 测试相关
- `chore`: 构建/工具链相关

**示例**:
```
feat(game): add new card type validation
fix(ai): resolve decision timeout issue
docs(readme): update installation instructions
```

### Code Review

提交PR前检查:
- [ ] 代码符合规范
- [ ] 测试通过
- [ ] 文档更新
- [ ] 无敏感信息泄露
- [ ] 提交信息清晰

## 资源链接

- [React文档](https://react.dev/)
- [Redux Toolkit文档](https://redux-toolkit.js.org/)
- [TypeScript文档](https://www.typescriptlang.org/)
- [Material-UI文档](https://mui.com/)
- [Framer Motion文档](https://www.framer.com/motion/)
- [Vite文档](https://vitejs.dev/)
