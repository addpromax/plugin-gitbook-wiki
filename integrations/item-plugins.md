# 物品插件集成

Codex 插件支持与多种主流物品插件的深度集成，让您能够在发现系统中使用自定义物品。本章详细介绍各种物品插件的集成配置和使用方法。

## 支持的物品插件

### 兼容性列表

| 插件名称 | 支持状态 | 自动检测 | 推荐版本 |
|----------|----------|----------|----------|
| **ItemsAdder** | ✅ 完全支持 | ✅ | 3.6.1+ |
| **MMOItems** | ✅ 完全支持 | ✅ | 6.9.4+ |
| **Craft-Engine** | ✅ 完全支持 | ✅ | 最新版 |
| **Oraxen** | 🔄 计划支持 | ❌ | - |
| **ExecutableItems** | 🔄 计划支持 | ❌ | - |

## ItemsAdder 集成

### 基础配置

ItemsAdder 是最受欢迎的自定义物品插件之一，Codex 提供完整的集成支持。

#### 启用检测

插件会在启动时自动检测 ItemsAdder：

```
[Codex] ItemsAdder Hook: 成功
[Codex] - 检测到 ItemsAdder 版本: 3.6.1
[Codex] - 已加载自定义物品: 247 个
[Codex] - 已加载命名空间: 15 个
```

### 发现配置

#### 基础物品发现

```yaml
discoveries:
  # ItemsAdder 自定义物品发现
  magic_sword_discovery:
    name: "&d魔法剑"
    description:
      - "&7你获得了传说中的魔法剑！"
      - "&7这把剑蕴含着古老的魔力。"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        # 使用 ItemsAdder 物品ID
        itemsadder_item_id: "myitems:magic_sword"
  
  # 支持命名空间
  ruby_gem_discovery:
    name: "&c红宝石"
    description:
      - "&7稀有的红宝石，散发着神秘光芒。"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        itemsadder_item_id: "gems:ruby"
```

#### 多物品匹配

```yaml
discoveries:
  # 匹配多个ItemsAdder物品
  gems_collection:
    name: "&6宝石收藏家"
    description:
      - "&7你收集了各种珍贵的宝石！"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        # 使用分号分隔多个物品ID
        itemsadder_item_id: "gems:ruby;gems:sapphire;gems:emerald"
```

#### 按命名空间匹配

```yaml
discoveries:
  # 匹配整个命名空间的物品
  weapons_master:
    name: "&c武器大师"
    description:
      - "&7你掌握了各种自定义武器！"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        # 使用通配符匹配命名空间
        itemsadder_item_id: "weapons:*"
```

### 高级配置

#### 结合原版判定

```yaml
discoveries:
  enhanced_diamond_sword:
    name: "&b增强钻石剑"
    description:
      - "&7经过特殊强化的钻石剑"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        # 同时满足原版类型和ItemsAdder ID
        item_type: "DIAMOND_SWORD"
        itemsadder_item_id: "weapons:enhanced_diamond_sword"
        # 还可以添加其他条件
        custom_model_data: 100001
```

#### NBT数据集成

```yaml
discoveries:
  legendary_artifact:
    name: "&6传奇神器"
    description:
      - "&7拥有特殊属性的传奇物品"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        itemsadder_item_id: "artifacts:legendary_staff"
        # ItemsAdder物品通常会有特定的NBT数据
        components: "itemsadder:id;itemsadder:namespace"
```

### ItemsAdder 特殊功能

#### 资源包物品

```yaml
# ItemsAdder的资源包物品配置
discoveries:
  custom_block_discovery:
    name: "&a自定义方块"
    description:
      - "&7发现了ItemsAdder的自定义方块！"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        itemsadder_item_id: "furniture:custom_chair"
  
  # 3D模型物品
  model_item_discovery:
    name: "&e3D模型物品"
    description:
      - "&7获得了带有3D模型的特殊物品！"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        itemsadder_item_id: "models:fancy_hat"
```

## MMOItems 集成

### 基础配置

MMOItems 是专业的RPG物品系统，提供丰富的物品属性和技能。

#### 启用检测

```
[Codex] MMOItems Hook: 成功
[Codex] - 检测到 MMOItems 版本: 6.9.4
[Codex] - 已加载物品类型: 12 个
[Codex] - 已加载自定义物品: 156 个
```

### 发现配置

#### 按物品类型

```yaml
discoveries:
  # 匹配所有剑类物品
  sword_master:
    name: "&c剑术大师"
    description:
      - "&7你掌握了各种剑类武器！"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        # 只指定物品类型
        mmoitems_type: "SWORD"
  
  # 匹配所有法杖
  staff_collector:
    name: "&5法杖收藏家"
    description:
      - "&7你收集了各种魔法法杖！"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        mmoitems_type: "STAFF"
```

#### 按具体物品

```yaml
discoveries:
  # 匹配特定的MMOItems物品
  excalibur_discovery:
    name: "&6王者之剑"
    description:
      - "&7传说中的王者之剑，只有真正的英雄才能举起！"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        mmoitems_type: "SWORD"
        mmoitems_id: "EXCALIBUR"
  
  # 稀有装备发现
  dragon_armor:
    name: "&4龙鳞铠甲"
    description:
      - "&7由龙鳞制作的强大铠甲！"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        mmoitems_type: "ARMOR"
        mmoitems_id: "DRAGON_SCALE_CHESTPLATE"
```

#### 多类型匹配

```yaml
discoveries:
  # 匹配多种物品类型
  equipment_master:
    name: "&e装备大师"
    description:
      - "&7你精通各种类型的装备！"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        # 使用分号分隔多个类型
        mmoitems_type: "SWORD;BOW;STAFF;ARMOR"
```

### MMOItems 高级功能

#### 属性判定

```yaml
discoveries:
  # 基于MMOItems属性的发现
  high_damage_weapon:
    name: "&c高伤害武器"
    description:
      - "&7你获得了攻击力超过100的武器！"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        mmoitems_type: "SWORD"
        # 可以添加自定义属性检查（需要额外配置）
        mmo_stat_requirements:
          attack_damage: ">100"
          durability: ">500"
```

#### 等级限制物品

```yaml
discoveries:
  # 高等级装备发现
  endgame_equipment:
    name: "&6终极装备"
    description:
      - "&7你已经强大到可以使用终极装备了！"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        mmoitems_type: "SWORD;ARMOR;ACCESSORY"
        # 等级要求（需要额外插件支持）
        level_requirement: ">=80"
```

## Craft-Engine 集成

### 基础配置

Craft-Engine 是另一个流行的自定义物品插件。

#### 启用检测

```
[Codex] CraftEngine Hook: 成功
[Codex] - 检测到 Craft-Engine 版本: 最新版
[Codex] - 已加载自定义物品: 89 个
[Codex] - 已加载配方: 156 个
```

### 发现配置

#### 基础物品发现

```yaml
discoveries:
  # Craft-Engine 自定义物品
  mythic_blade:
    name: "&6神话之刃"
    description:
      - "&7传说中的神话级武器！"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        # 使用 Craft-Engine 物品ID
        craft_engine_id: "myplugin:mythic_blade"
  
  # 魔法物品
  magic_crystal:
    name: "&d魔法水晶"
    description:
      - "&7蕴含纯净魔力的水晶"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        craft_engine_id: "magic:pure_crystal"
```

#### 命名空间支持

```yaml
discoveries:
  # 按命名空间匹配
  magic_items_collection:
    name: "&5魔法物品收藏"
    description:
      - "&7你收集了各种魔法物品！"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        # 匹配magic命名空间下的所有物品
        craft_engine_id: "magic:*"
```

## 混合集成配置

### 多插件物品发现

```yaml
discoveries:
  # 同时支持多种插件的物品
  legendary_weapon_collection:
    name: "&6传奇武器收藏家"
    description:
      - "&7你收集了来自各个插件的传奇武器！"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        # 可以同时配置多种插件的物品ID
        itemsadder_item_id: "weapons:legendary_sword"
        mmoitems_type: "SWORD"
        mmoitems_id: "LEGENDARY_BLADE"
        craft_engine_id: "weapons:mythic_sword"
        # 满足任意一个条件即可触发
```

### 优先级系统

```yaml
# 插件检测优先级配置
plugin_priority:
  # 物品检测优先级（数字越小优先级越高）
  detection_order:
    1: "itemsadder"     # 优先检测 ItemsAdder
    2: "mmoitems"       # 其次检测 MMOItems
    3: "craft_engine"   # 最后检测 Craft-Engine
    4: "vanilla"        # 最后检测原版物品
  
  # 冲突处理
  conflict_resolution:
    # 当多个插件都能匹配同一物品时的处理方式
    strategy: "first_match"  # first_match, all_match, priority
```

## 性能优化

### 缓存配置

```yaml
# 物品插件集成的性能优化
integration_performance:
  # 物品ID缓存
  item_id_cache:
    enabled: true
    max_size: 10000
    expire_time: 3600  # 1小时
  
  # 插件检测缓存
  plugin_detection_cache:
    enabled: true
    max_size: 1000
    expire_time: 1800  # 30分钟
  
  # 异步处理
  async_processing:
    enabled: true
    thread_pool_size: 2
```

### 检测优化

```yaml
integration_performance:
  # 减少不必要的检测
  optimization:
    # 跳过已知的原版物品
    skip_vanilla_items: true
    
    # 使用快速匹配模式
    fast_matching: true
    
    # 批量处理物品检测
    batch_detection: true
    batch_size: 50
```

## 调试与故障排除

### 调试模式

启用调试模式查看物品插件集成状态：

```yaml
# 在主配置文件中
settings:
  debug: true
  
  # 物品集成调试
  debug_categories:
    - "item_plugins"
    - "item_detection"
    - "plugin_hooks"
```

调试信息包括：
- 物品插件挂钩状态
- 物品ID解析过程
- 匹配结果详情
- 性能统计信息

### 常见问题

#### 问题：物品插件未被检测到
**排查步骤**：
1. 确认插件已正确安装并启用
2. 检查插件版本兼容性
3. 查看控制台启动日志
4. 验证插件加载顺序

**解决方案**：
```yaml
# 强制启用插件挂钩
force_plugin_hooks:
  itemsadder: true
  mmoitems: true
  craft_engine: true
```

#### 问题：自定义物品不被识别
**排查步骤**：
1. 验证物品ID格式是否正确
2. 检查命名空间和物品名称
3. 确认物品在插件中已正确配置
4. 使用调试模式查看检测过程

**解决方案**：
```yaml
# 手动物品映射
manual_item_mapping:
  "custom_item_1":
    plugin: "itemsadder"
    id: "myitems:special_sword"
  "custom_item_2":
    plugin: "mmoitems"
    type: "SWORD"
    id: "LEGENDARY_BLADE"
```

#### 问题：性能影响过大
**排查步骤**：
1. 检查缓存配置是否启用
2. 监控物品检测频率
3. 分析处理时间统计
4. 调整批处理设置

**解决方案**：
```yaml
# 性能调优配置
performance_tuning:
  # 增大缓存
  cache_size: 20000
  
  # 减少检测频率
  detection_cooldown: 100  # 毫秒
  
  # 启用延迟处理
  lazy_loading: true
```

## 实际应用示例

### RPG 服务器配置

```yaml
# 为RPG服务器配置的物品发现系统
discoveries:
  # 职业装备系统
  warrior_equipment:
    name: "&c战士装备精通"
    description:
      - "&7你已经掌握了战士的专业装备！"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        mmoitems_type: "SWORD;ARMOR"
        itemsadder_item_id: "warrior:*"
  
  # 稀有材料收集
  rare_materials:
    name: "&6稀有材料大师"
    description:
      - "&7你收集了各种稀有的制作材料！"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        itemsadder_item_id: "materials:dragon_scale;materials:phoenix_feather"
        craft_engine_id: "materials:*"
```

### 生存服务器配置

```yaml
# 为生存服务器配置的工具发现系统
discoveries:
  # 高级工具收集
  advanced_tools:
    name: "&e工具大师"
    description:
      - "&7你拥有了最先进的工具！"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        itemsadder_item_id: "tools:diamond_drill;tools:auto_miner"
        mmoitems_type: "TOOL"
```

## 下一步

了解了物品插件集成后，您可以继续学习：

1. **[附魔插件集成](enchantment-plugins.md)** - Aiyatsbus、EcoEnchants 等附魔插件
2. **[其他插件支持](other-plugins.md)** - WorldGuard、MythicMobs 等插件集成
3. **[配置优化](../configuration/discovery-config.md)** - 高级发现配置技巧
4. **[性能调优](../admin/performance.md)** - 系统性能优化
