# 路面与钢轨

路面和钢轨都是游戏内与玩家球发生主要碰撞的对象。

## 路面

路面即游戏中随处可见的道路，会被游戏赋予物理属性，作为一个恒固定不变的物体，玩家球可以行走其上。路面的网格决定了其碰撞形状。

在游戏中，若想要路面正常工作，需要将路面物体归入以下几个组：

- `Phys_Floors`：使路面物理化，否则无法与玩家球发生碰撞。
- `Sound_HitID_01`：玩家与该组物体碰撞时会发出水泥地面的碰撞音效。
- `Sound_RollID_01`：玩家球在该组物体滚动时会发出水泥地面的滚动音效。
- `Shadow`：玩家球在该组物体上方一定距离内时会投射出影子。

## 钢轨

钢轨可以看作一类特殊的路面，他和路面的物理属性完全一致，区别主要有以下几点：

- 钢轨组的物体在游戏中会有视觉上的 ~~模糊~~ 平滑效果。
- 钢轨不需要归入影子组。

若想要钢轨正常工作，需要将钢轨物体归入以下几个组：

- `Phys_FloorRails`：使钢轨物理化，否则无法与玩家球发生碰撞。
- `Sound_HitID_03`：玩家与该组物体碰撞时会发出钢轨的碰撞音效。
- `Sound_RollID_03`：玩家球在该组物体滚动时会发出钢轨的滚动音效。

### 钢轨参数

> ~~如果我想去做钢轨而不是在这里写些乱七八糟的东西，那么下面的话可能会有用（~~

Ballance 中的钢轨看起来是圆柱形，但实际上其截面是 **正八边形**。

钢轨一般分为平顶和尖顶，即八边形的边朝上还是角朝上。原版的单轨全部是平顶，在对齐的情况下能保证完美球形可以行走而不掉落。原版的双轨都是尖顶，~~猜测可能是为了防止玩家手动对齐而逃课~~。

当需要自行制作钢轨时，以下参数可能有用：

| 数据             | 取值      | 备注                               |
| ---------------- | --------- | ---------------------------------- |
| 单轨截面半径     | `0.35`    |                                    |
| 双轨中心间距     | `3.75`    | 约为该值，便于记忆                 |
| 单双轨 Z 轴偏移  | `0.62`    | 取玩家球半径为 `2`，由失衡之梦计算 |
| 普通侧轨倾斜角度 | `79.563°` | 由 BallanceBug 提供                |
| 普通侧轨间距     | `3.864`   | 由 BallanceBug 提供                |
| 螺旋轨层间距     | `5`       | 从 Level 10 测量                   |
| 侧边螺旋轨层间距 | `3.6`     | 由 Level 13 结尾，Level 9 开头测量 |

## Stopper

Stopper 是游戏中一类特殊的碰撞物体，玩家球无法与其发生碰撞，但场景中的机关道具可以。

Stopper 需要归入以下组：

- `Phys_FloorStopper`：使 Stopper 物理化，否则无法与机关道具发生碰撞。

### 常见用途

- 机关支撑：在原版中最为常见的 Stopper 用途，例如翘翘板、推板等倒下时会有一个木制组件进行支撑。
- 隐形道具支撑：在自制图中更为常见，将 Stopper 隐形，一般用于限制道具球、道具箱子的移动，在不影响玩家视觉体验的前提下减少道具的随机性。

::: tip 热知识
原版也有隐形 Stopper，例如 Level 4 - 3 从天而降的石球就是用隐形 Stopper 缓存的。
:::

### 声音 Bug

由于 Ballance 原版脚本编写的失误，只有位于 `Phys_FloorStopper` 组中的第一个物体才会发出碰撞声音。

因此，为了让地图中所有 Stopper 都有声音，建议在 **地图发布前** 将所有 Stopper 合并成一个物体。具体操作为：在 Blender 中选中所有 Stopper 然后按 `Ctrl + J` 合并即可。
