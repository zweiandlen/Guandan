# 掼蛋纸牌游戏接口文档

## 概述

本文档定义了掼蛋纸牌游戏的公开接口,包括组件API、游戏引擎API、AI决策API等。

## 组件接口

### GameRoom

游戏房间主组件,负责整体游戏流程控制。

```typescript
interface GameRoomProps {
  gameId: string;
  onSave?: () => void;
  onLoad?: () => void;
}
```

**示例用法**:
```tsx
<GameRoom gameId="game-001" onSave={handleSave} onLoad={handleLoad} />
```

### PlayerArea

玩家手牌显示区域,分为4个方位(下、左、上、右)。

```typescript
interface PlayerAreaProps {
  player: Player;
  position: 'bottom' | 'left' | 'top' | 'right';
  isCurrentPlayer: boolean;
  onCardSelect: (cardId: string) => void;
}
```

**示例用法**:
```tsx
<PlayerArea
  player={player}
  position="bottom"
  isCurrentPlayer={true}
  onCardSelect={handleCardSelect}
/>
```

### Card

单张卡牌组件,支持正反面显示。

```typescript
interface CardProps {
  card: Card;
  isFaceUp: boolean;
  isSelected?: boolean;
  onSelect?: () => void;
  animation?: AnimationConfig;
}
```

**示例用法**:
```tsx
<Card
  card={card}
  isFaceUp={true}
  isSelected={true}
  onSelect={handleCardClick}
/>
```

### GameBoard

出牌区域,显示当前回合出的牌。

```typescript
interface GameBoardProps {
  playedCards: PlayedCard[];
  lastPlay: PlayResult | null;
}
```

**示例用法**:
```tsx
<GameBoard
  playedCards={playedCards}
  lastPlay={lastPlay}
/>
```

### ScoreBoard

计分板,显示两队得分。

```typescript
interface ScoreBoardProps {
  team1Score: number;
  team2Score: number;
  round: number;
  trump: CardRank;
}
```

**示例用法**:
```tsx
<ScoreBoard
  team1Score={5}
  team2Score={3}
  round={2}
  trump={CardRank.TWO}
/>
```

## 游戏引擎API

### 规则验证器 (Validator)

```typescript
class CardValidator {
  /**
   * 验证卡牌组合是否合法
   * @param cards 卡牌数组
   * @param lastPlay 上一轮出牌(可选)
   * @returns 验证结果
   */
  validate(cards: Card[], lastPlay?: CardCombination): ValidationResult;

  /**
   * 识别牌型
   * @param cards 卡牌数组
   * @returns 牌型信息
   */
  identifyCardType(cards: Card[]): CardType | null;

  /**
   * 比较两组牌的大小
   * @param combo1 牌组1
   * @param combo2 牌组2
   * @returns 比较结果(1: combo1大, -1: combo2大, 0: 相等)
   */
  compare(combo1: CardCombination, combo2: CardCombination): number;
}
```

**使用示例**:
```typescript
const validator = new CardValidator();

// 验证牌型
const result = validator.validate(cards, lastPlay);
if (!result.isValid) {
  console.error(result.error);
}

// 识别牌型
const cardType = validator.identifyCardType(cards);
console.log('牌型:', cardType);

// 比较大小
const comparison = validator.compare(combo1, combo2);
if (comparison > 0) {
  console.log('combo1更大');
}
```

### 牌堆管理器 (DeckManager)

```typescript
class DeckManager {
  /**
   * 创建标准牌堆(两副牌108张)
   * @returns 卡牌数组
   */
  createDeck(): Card[];

  /**
   * 洗牌
   * @param cards 卡牌数组
   * @returns 洗牌后的卡牌数组
   */
  shuffle(cards: Card[]): Card[];

  /**
   * 发牌
   * @param deck 牌堆
   * @param playerCount 玩家数量
   * @returns 玩家手牌数组
   */
  deal(deck: Card[], playerCount: number): Card[][];

  /**
   * 抽牌
   * @param deck 牌堆
   * @param count 抽牌数量
   * @returns 抽取的卡牌和剩余牌堆
   */
  draw(deck: Card[], count: number): { drawn: Card[]; remaining: Card[] };
}
```

**使用示例**:
```typescript
const deckManager = new DeckManager();

// 创建牌堆
const deck = deckManager.createDeck();

// 洗牌
const shuffledDeck = deckManager.shuffle(deck);

// 发牌
const hands = deckManager.deal(shuffledDeck, 4);
// hands[0]: 玩家1手牌
// hands[1]: 玩家2手牌
// ...
```

### 得分计算器 (ScoreCalculator)

```typescript
class ScoreCalculator {
  /**
   * 计算回合得分
   * @param playedCards 本回合出牌记录
   * @param winner 回合赢家
   * @returns 得分结果
   */
  calculateRoundScore(
    playedCards: PlayResult[],
    winner: Player
  ): RoundScore;

  /**
   * 计算游戏总分
   * @param roundScores 所有回合得分
   * @returns 游戏总分
   */
  calculateTotalScore(roundScores: RoundScore[]): GameScore;

  /**
   * 判定游戏胜负
   * @param gameState 游戏状态
   * @returns 胜负结果
   */
  determineWinner(gameState: GameState): WinnerResult;
}
```

**使用示例**:
```typescript
const scoreCalculator = new ScoreCalculator();

// 计算回合得分
const roundScore = scoreCalculator.calculateRoundScore(
  playedCards,
  winner
);
console.log('回合得分:', roundScore);

// 判定胜负
const winner = scoreCalculator.determineWinner(gameState);
console.log('获胜队伍:', winner.team);
```

## AI决策API

### AI决策器 (AIDecisionMaker)

```typescript
class AIDecisionMaker {
  /**
   * 创建AI决策器
   * @param playerId 玩家ID
   * @param difficulty 难度级别
   */
  constructor(playerId: string, difficulty: 'easy' | 'medium' | 'hard');

  /**
   * 做出决策
   * @param gameState 游戏状态
   * @param lastPlay 上一轮出牌
   * @returns AI决策
   */
  makeDecision(
    gameState: GameState,
    lastPlay?: CardCombination
  ): AIDecision;

  /**
   * 更新AI记忆
   * @param play 出牌记录
   */
  updateMemory(play: PlayResult): void;

  /**
   * 切换策略
   * @param strategy 策略类型
   */
  switchStrategy(strategy: 'aggressive' | 'conservative' | 'balanced'): void;
}
```

**使用示例**:
```typescript
const ai = new AIDecisionMaker('player-2', 'medium');

// AI决策
const decision = ai.makeDecision(gameState, lastPlay);
if (decision.action === 'play') {
  console.log('AI出牌:', decision.cards);
} else {
  console.log('AI选择过牌');
}

// 更新记忆
ai.updateMemory(playResult);
```

## 存储API

### 存档管理器 (SaveManager)

```typescript
class SaveManager {
  /**
   * 保存游戏
   * @param gameState 游戏状态
   * @param slot 存档槽位
   * @returns 保存结果
   */
  save(gameState: GameState, slot: number = 1): Promise<SaveResult>;

  /**
   * 读取存档
   * @param slot 存档槽位
   * @returns 游戏状态
   */
  load(slot: number = 1): Promise<GameState>;

  /**
   * 获取存档列表
   * @returns 存档信息数组
   */
  getSaveSlots(): SaveSlotInfo[];

  /**
   * 删除存档
   * @param slot 存档槽位
   */
  delete(slot: number): void;

  /**
   * 清除所有存档
   */
  clearAll(): void;
}
```

**使用示例**:
```typescript
const saveManager = new SaveManager();

// 保存游戏
await saveManager.save(gameState, 1);
console.log('游戏已保存');

// 读取存档
const savedState = await saveManager.load(1);
console.log('读取存档:', savedState);

// 获取存档列表
const slots = saveManager.getSaveSlots();
console.log('可用存档:', slots);
```

## Redux Store API

### 游戏状态

```typescript
interface RootState {
  game: GameState;
  players: PlayersState;
  cards: CardsState;
  ui: UIState;
}
```

### 游戏Actions

```typescript
// 游戏初始化
interface InitGameAction {
  type: 'game/init';
  payload: {
    playerId: string;
    difficulty: string;
  };
}

// 出牌
interface PlayCardsAction {
  type: 'game/playCards';
  payload: {
    playerId: string;
    cards: Card[];
  };
}

// 过牌
interface PassAction {
  type: 'game/pass';
  payload: {
    playerId: string;
  };
}

// 更新回合
interface NextTurnAction {
  type: 'game/nextTurn';
}

// 游戏结束
interface GameOverAction {
  type: 'game/gameOver';
  payload: {
    winner: Player;
    scores: GameScore;
  };
}
```

**使用示例**:
```typescript
import { useDispatch, useSelector } from 'react-redux';

const dispatch = useDispatch();
const gameState = useSelector((state: RootState) => state.game);

// 初始化游戏
dispatch({
  type: 'game/init',
  payload: { playerId: 'player-1', difficulty: 'medium' }
});

// 出牌
dispatch({
  type: 'game/playCards',
  payload: { playerId: 'player-1', cards: selectedCards }
});

// 过牌
dispatch({
  type: 'game/pass',
  payload: { playerId: 'player-1' }
});
```

## 键盘快捷键

| 快捷键 | 功能 | 说明 |
|--------|------|------|
| Space | 出牌 | 出选中的卡牌 |
| P | 过牌 | 跳过当前回合 |
| H | 帮助 | 打开帮助弹窗 |
| Esc | 取消选择 | 取消选中的卡牌 |
| S | 存档 | 保存游戏进度 |
| L | 读档 | 读取游戏存档 |
| M | 设置 | 打开设置菜单 |
| R | 重新开始 | 重新开始游戏 |

## 事件系统

### 游戏事件

```typescript
// 游戏开始事件
interface GameStartEvent {
  type: 'GAME_START';
  gameId: string;
  timestamp: number;
}

// 回合开始事件
interface TurnStartEvent {
  type: 'TURN_START';
  playerId: string;
  round: number;
}

// 出牌事件
interface CardPlayEvent {
  type: 'CARD_PLAY';
  playerId: string;
  cards: Card[];
  combination: CardCombination;
}

// 游戏结束事件
interface GameOverEvent {
  type: 'GAME_OVER';
  winner: Player;
  scores: GameScore;
  duration: number;
}

// 错误事件
interface ErrorEvent {
  type: 'ERROR';
  error: Error;
  context?: any;
}
```

## 类型定义

### 卡牌类型

```typescript
enum Suit {
  SPADES = 'spades',
  HEARTS = 'hearts',
  DIAMONDS = 'diamonds',
  CLUBS = 'clubs',
}

enum CardRank {
  TWO = 2,
  THREE,
  FOUR,
  FIVE,
  SIX,
  SEVEN,
  EIGHT,
  NINE,
  TEN,
  JACK = 11,
  QUEEN = 12,
  KING = 13,
  ACE = 14,
  SMALL_JOKER = 15,
  BIG_JOKER = 16,
}

interface Card {
  id: string;
  suit: Suit;
  rank: CardRank;
  value: number;
  isTrump: boolean;
  isRedHeart?: boolean;
}
```

### 玩家类型

```typescript
interface Player {
  id: string;
  name: string;
  hand: Card[];
  team: 1 | 2;
  isHuman: boolean;
  position: 'south' | 'west' | 'north' | 'east';
  stats: {
    wins: number;
    losses: number;
    totalGames: number;
  };
}
```

### 游戏状态类型

```typescript
interface GameState {
  gameId: string;
  status: 'waiting' | 'dealing' | 'playing' | 'finished';
  currentRound: number;
  trumpRank: CardRank;
  trumpSuit: Suit | null;
  players: Player[];
  currentPlayerIndex: number;
  teamScores: [number, number];
  currentTurn: {
    playerId: string;
    startTime: number;
    isThinking: boolean;
  };
  playedCards: {
    playerId: string;
    cards: Card[];
    combination: CardCombination | null;
  }[];
  playHistory: PlayResult[];
  lastValidPlay: PlayResult | null;
  isGonging: boolean;
  gongCards: {
    fromPlayer: string;
    toPlayer: string;
    card: Card;
  }[];
}
```
