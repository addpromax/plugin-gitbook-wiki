# 基础发现系统

Codex 的核心功能是发现系统，它通过多种触发机制让玩家在自然游戏过程中解锁各种发现条目。本章将详细介绍发现系统的工作原理、配置方法和高级用法。

## 发现系统概述

### 工作原理

发现系统基于事件监听机制：
1. 📡 **事件监听**：插件监听游戏中的各种事件
2. 🔍 **条件检查**：当事件发生时，检查是否满足发现条件
3. 🎯 **触发发现**：条件匹配时，触发相应的发现
4. 🎁 **执行奖励**：给予玩家配置的奖励

### 核心概念

- **发现条目 (Discovery)**：可被解锁的单个发现项目
- **触发器 (Trigger)**：定义何时解锁发现的条件
- **分类 (Category)**：发现条目的组织方式
- **奖励 (Reward)**：解锁发现时给予的奖励

## 触发器类型

### 1. 物品获取触发器 (ITEM_OBTAIN)

当玩家首次获得指定物品时触发发现。

#### 基础配置

```yaml
discoveries:
  first_diamond:
    name: "&b第一颗钻石"
    description:
      - "&7你获得了第一颗珍贵的钻石！"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        item_type: DIAMOND
```

#### 高级物品判定

**多种物品类型**：
```yaml
discovered_on:
  type: ITEM_OBTAIN
  value:
    item_type: "DIAMOND;EMERALD;NETHERITE_INGOT"  # 任意一种即可触发
```

**自定义模型数据**：
```yaml
discovered_on:
  type: ITEM_OBTAIN
  value:
    item_type: STICK
    custom_model_data: 100001  # 特定的自定义模型
```

**NBT 组件判定**：
```yaml
discovered_on:
  type: ITEM_OBTAIN
  value:
    item_type: DIAMOND_SWORD
    components: "codex:special;codex:legendary"  # 必须包含这些NBT键
```

### 2. 指令执行触发器 (COMMAND_RUN)

当玩家首次执行指定指令时触发。

```yaml
discoveries:
  first_command:
    name: "&6指令大师"
    description:
      - "&7你学会了使用服务器指令！"
    discovered_on:
      type: COMMAND_RUN
      value:
        command: "spawn;home;warp"  # 执行任意一个指令即触发
```

### 3. 生物击杀触发器 (MOB_KILL)

当玩家击杀指定生物时触发。

```yaml
discoveries:
  ender_dragon_slayer:
    name: "&5屠龙勇士"
    description:
      - "&7你击败了末影龙！"
      - "&7真正的勇者诞生了！"
    discovered_on:
      type: MOB_KILL
      value:
        entity_type: ENDER_DRAGON
```

#### MythicMobs 支持

```yaml
discovered_on:
  type: MOB_KILL
  value:
    mythic_mob_type: "AncientDragon"  # MythicMobs 生物类型
```

### 4. 区域进入触发器 (WORLDGUARD_REGION)

当玩家进入指定 WorldGuard 区域时触发。

```yaml
discoveries:
  secret_chamber:
    name: "&d神秘密室"
    description:
      - "&7你发现了隐藏的密室！"
    discovered_on:
      type: WORLDGUARD_REGION
      value:
        region_name: "secret_room"
        world_name: "world"  # 可选，不指定则任意世界
```

### 5. 附魔发现触发器 (ENCHANTMENT_DISCOVER)

当玩家首次接触到指定附魔时触发。

```yaml
discoveries:
  sharpness_discovery:
    name: "&e锋利附魔"
    description:
      - "&7你发现了锋利附魔！"
    discovered_on:
      type: ENCHANTMENT_DISCOVER
      value:
        enchantment_id: "minecraft:sharpness"
```

#### Aiyatsbus 附魔支持

```yaml
discovered_on:
  type: ENCHANTMENT_DISCOVER
  value:
    enchantment_id: "aiyatsbus:telekinesis"  # Aiyatsbus 自定义附魔
```

## 第三方插件集成

### ItemsAdder 物品

```yaml
discovered_on:
  type: ITEM_OBTAIN
  value:
    itemsadder_item_id: "myitems:magic_sword"
```

### MMOItems 物品

```yaml
# 仅指定类型
discovered_on:
  type: ITEM_OBTAIN
  value:
    mmoitems_type: "SWORD"

# 指定具体物品
discovered_on:
  type: ITEM_OBTAIN
  value:
    mmoitems_type: "SWORD"
    mmoitems_id: "EXCALIBUR"
```

### Craft-Engine 物品

```yaml
discovered_on:
  type: ITEM_OBTAIN
  value:
    craft_engine_id: "myplugin:legendary_blade"
```

## 奖励系统

### 奖励类型

#### 1. 消息奖励
```yaml
rewards:
  per_discovery:
    - "message: &e恭喜！你解锁了 %name%"
    - "centered_message: &6=== 发现解锁 ==="
```

#### 2. 音效奖励
```yaml
rewards:
  per_discovery:
    - "playsound: ENTITY_PLAYER_LEVELUP;1;1.5"  # 音效;音量;音调
    - "playsound: BLOCK_NOTE_BLOCK_PLING;0.8;2"
```

#### 3. 标题奖励
```yaml
rewards:
  per_discovery:
    - "title: 20;60;20;&6发现解锁!;&e%name%"  # 淡入;显示;淡出;主标题;副标题
```

#### 4. 指令奖励
```yaml
rewards:
  per_discovery:
    - "console_command: experience add %player% 100 points"
    - "console_command: give %player% diamond 1"
    - "player_command: spawn"  # 让玩家执行指令
```

#### 5. 物品奖励
```yaml
rewards:
  per_discovery:
    - "item: DIAMOND 1 name:&6奖励钻石 lore:&7发现奖励"
```

### 变量系统

在奖励中可以使用以下变量：

| 变量 | 说明 | 示例 |
|------|------|------|
| `%player%` | 玩家名称 | `Steve` |
| `%name%` | 发现名称 | `第一颗钻石` |
| `%description%` | 发现描述 | `你获得了...` |
| `%date%` | 发现日期 | `2024-11-04` |
| `%time%` | 发现时间 | `14:30:25` |
| `%category%` | 分类名称 | `物品收集` |

#### PlaceholderAPI 支持

如果安装了 PlaceholderAPI，还可以使用：
- `%player_displayname%` - 玩家显示名
- `%player_world%` - 玩家所在世界
- `%vault_eco_balance%` - 玩家金钱（需要Vault）

## 分类配置

### 分类结构

每个分类都是一个独立的 YAML 文件，包含：

```yaml
# 分类配置
config:
  inventory_items:
    # 分类图标
    category:
      id: DIAMOND
      name: "&b物品收集"
      lore:
        - "&7收集各种珍贵物品"
        - "&7进度：&e%progress_bar%"
    
    # 已解锁发现显示
    discovery_unlocked:
      id: "%material%"  # 使用物品本身的材质
      name: "%name%"
      lore:
        - "%description%"
        - ""
        - "&7解锁时间：&f%date% %time%"
    
    # 未解锁发现显示
    discovery_blocked:
      id: GRAY_DYE
      name: "&8未知发现"
      lore:
        - "&7继续游戏以解锁此发现"
  
  # 奖励配置
  rewards:
    # 单个发现奖励
    per_discovery:
      - "playsound: ENTITY_PLAYER_LEVELUP;1;1"
      - "console_command: experience add %player% 50 points"
    
    # 完成所有发现的奖励
    all_discoveries:
      - "title: 20;60;20;&6大师级收藏家!;&e完成了所有发现"
      - "console_command: give %player% nether_star 1"

# 发现定义
discoveries:
  # 在这里定义具体的发现条目
```

### 进度显示

分类支持进度显示功能：

```yaml
category:
  lore:
    - "&7进度：&e%progress_bar%"      # ████░░░░ 4/8
    - "&7已解锁：&e%unlocked%&7/&e%total%"  # 已解锁：4/8
    - "&7完成度：&e%percentage%%"     # 完成度：50%
```

## 高级配置技巧

### 1. 复杂触发条件

#### 多重条件物品
```yaml
discovered_on:
  type: ITEM_OBTAIN
  value:
    item_type: "DIAMOND_SWORD"
    custom_model_data: 100001
    components: "codex:legendary;codex:rare"
    # 必须同时满足所有条件
```

#### 世界限制
```yaml
discovered_on:
  type: MOB_KILL
  value:
    entity_type: "ZOMBIE"
    world: "world_nether"  # 仅在地狱世界触发
```

### 2. 条件发现

某些发现可以依赖其他发现：

```yaml
discoveries:
  diamond_master:
    name: "&b钻石大师"
    description:
      - "&7你已经是钻石收集专家了！"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        item_type: DIAMOND
        required_discoveries:  # 需要先解锁这些发现
          - "first_diamond"
          - "diamond_tools"
```

### 3. 时间限制发现

```yaml
discoveries:
  night_hunter:
    name: "&8夜行猎手"
    description:
      - "&7在夜晚击败怪物！"
    discovered_on:
      type: MOB_KILL
      value:
        entity_type: "ZOMBIE"
        time_range: "18000-6000"  # 游戏时间范围（夜晚）
```

### 4. 数量要求

```yaml
discoveries:
  diamond_collector:
    name: "&b钻石收藏家"
    description:
      - "&7收集了大量钻石！"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        item_type: DIAMOND
        amount: 64  # 需要一次获得64个钻石
```

## 调试与测试

### 启用调试模式

在 `config.yml` 中：
```yaml
settings:
  debug: true
```

### 调试信息

启用调试模式后，控制台会显示：
- 触发器检查过程
- 条件匹配结果
- 发现解锁情况
- 性能统计信息

### 测试命令

```bash
# 强制触发发现（管理员）
/codex force <玩家> <发现ID>

# 重置玩家发现（管理员）
/codex reset <玩家> [分类]

# 查看玩家数据
/codex player <玩家>
```

## 性能优化

### 1. 合理配置触发器

- 避免过于宽泛的触发条件
- 使用具体的物品类型而非通配符
- 合理设置检查频率

### 2. 数据库优化

```yaml
# 使用批量保存
batch_save:
  enabled: true
  interval: 300  # 5分钟保存一次
  max_queue_size: 1000
```

### 3. 内存管理

```yaml
# 自动清理离线玩家数据
memory_management:
  cleanup_interval: 1800  # 30分钟
  clean_offline_players: true
```

## 下一步

现在您已经了解了发现系统的基础知识，可以继续学习：

1. **[命令与权限](commands.md)** - 了解所有可用命令
2. **[自动图鉴系统](enchantment-codex.md)** - 配置自动图鉴
3. **[第三方插件集成](../integrations/item-plugins.md)** - 深度集成其他插件
4. **[配置文件详解](../configuration/discovery-config.md)** - 高级配置技巧
