# 附魔插件集成

Codex 插件支持与主流附魔插件的深度集成，不仅能够自动识别自定义附魔，还能将它们纳入附魔图鉴系统。本章详细介绍各种附魔插件的集成配置和使用方法。

## 支持的附魔插件

### 兼容性列表

| 插件名称 | 支持状态 | 自动检测 | 图鉴集成 | 推荐版本 |
|----------|----------|----------|----------|----------|
| **Aiyatsbus** | ✅ 完全支持 | ✅ | ✅ | 1.0.9+ |
| **EcoEnchants** | ✅ 完全支持 | ✅ | ✅ | 12.7.0+ |
| **ExcellentEnchants** | 🔄 计划支持 | ❌ | ❌ | - |
| **AdvancedEnchantments** | 🔄 计划支持 | ❌ | ❌ | - |

## Aiyatsbus 集成

### 基础配置

Aiyatsbus 是功能强大的自定义附魔系统，Codex 提供完整的集成支持。

#### 启用检测

插件会在启动时自动检测 Aiyatsbus：

```
[Codex] Aiyatsbus Hook: 成功
[Codex] - 检测到 Aiyatsbus 版本: 1.0.9
[Codex] - 已加载自定义附魔: 156 个
[Codex] - 已加载附魔类别: 12 个
[Codex] - 支持附魔图鉴: 是
```

### 发现配置

#### 基础附魔发现

```yaml
discoveries:
  # Aiyatsbus 自定义附魔发现
  telekinesis_discovery:
    name: "&d心灵手巧"
    description:
      - "&7你发现了心灵手巧附魔！"
      - "&7破坏方块时物品直接进入背包。"
    discovered_on:
      type: ENCHANTMENT_DISCOVER
      value:
        # 使用 Aiyatsbus 附魔ID
        enchantment_id: "aiyatsbus:telekinesis"
  
  # 高级自定义附魔
  soul_bound_discovery:
    name: "&6灵魂绑定"
    description:
      - "&7传说中的灵魂绑定附魔！"
      - "&7死亡时不会掉落绑定的物品。"
    discovered_on:
      type: ENCHANTMENT_DISCOVER
      value:
        enchantment_id: "aiyatsbus:soul_bound"
```

#### 按类别匹配

```yaml
discoveries:
  # 匹配特定类别的附魔
  combat_enchants_master:
    name: "&c战斗附魔大师"
    description:
      - "&7你掌握了所有战斗类附魔！"
    discovered_on:
      type: ENCHANTMENT_DISCOVER
      value:
        # 匹配战斗类别的所有附魔
        enchantment_category: "combat"
        min_enchants: 5  # 至少发现5个战斗附魔
```

#### 等级限制发现

```yaml
discoveries:
  # 高等级附魔发现
  max_level_enchant:
    name: "&6满级附魔大师"
    description:
      - "&7你获得了满级的稀有附魔！"
    discovered_on:
      type: ENCHANTMENT_DISCOVER
      value:
        enchantment_id: "aiyatsbus:fortune"
        min_level: 5  # 需要5级以上的时运附魔
```

### Aiyatsbus 高级功能

#### 附魔组合发现

```yaml
discoveries:
  # 多个Aiyatsbus附魔组合
  ultimate_weapon:
    name: "&6终极武器"
    description:
      - "&7你创造了拥有多个传奇附魔的终极武器！"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        item_type: "DIAMOND_SWORD"
        # 需要同时拥有多个Aiyatsbus附魔
        required_enchantments:
          - "aiyatsbus:soul_bound"
          - "aiyatsbus:lifesteal"
          - "aiyatsbus:critical_strike"
```

#### 冲突附魔检测

```yaml
discoveries:
  # 检测冲突的附魔组合
  forbidden_combination:
    name: "&4禁忌组合"
    description:
      - "&7你发现了理论上不应该存在的附魔组合！"
      - "&c这可能会产生意想不到的后果..."
    discovered_on:
      type: ITEM_OBTAIN
      value:
        # 检测冲突的附魔组合
        conflicting_enchantments:
          - "aiyatsbus:fire_aspect"
          - "aiyatsbus:frost_aspect"
```

### 自动图鉴集成

#### 启用Aiyatsbus附魔图鉴

编辑 `plugins/Codex/enchantments.yml`：

```yaml
settings:
  # 启用Aiyatsbus信息获取
  fetch_aiyatsbus_info: true
  
  filter:
    # 包含Aiyatsbus附魔
    include_aiyatsbus: true
    
    # 排除特定的Aiyatsbus附魔
    excluded:
      - "aiyatsbus:test_enchant"
      - "aiyatsbus:debug_enchant"

# Aiyatsbus附魔的自定义显示
enchantments:
  "aiyatsbus:telekinesis":
    name: "<purple>心灵手巧"
    description:
      - "<gray>破坏方块时物品直接进入背包"
      - "<gray>对于挖矿和伐木特别有用"
      - ""
      - "<gold>Aiyatsbus 提供"
  
  "aiyatsbus:auto_smelt":
    name: "<gold>自动熔炼"
    description:
      - "<gray>破坏方块时自动熔炼掉落物"
      - "<gray>节省熔炉燃料和时间"
      - ""
      - "<gold>Aiyatsbus 提供"
```

## EcoEnchants 集成

### 基础配置

EcoEnchants 是另一个流行的附魔插件，注重生态友好和性能优化。

#### 启用检测

```
[Codex] EcoEnchants Hook: 成功
[Codex] - 检测到 EcoEnchants 版本: 12.7.0
[Codex] - 已加载自定义附魔: 87 个
[Codex] - 已加载附魔组: 8 个
[Codex] - 支持附魔图鉴: 是
```

### 发现配置

#### 基础附魔发现

```yaml
discoveries:
  # EcoEnchants 自定义附魔
  vampirism_discovery:
    name: "&4吸血"
    description:
      - "&7你发现了吸血附魔！"
      - "&7攻击时会恢复生命值。"
    discovered_on:
      type: ENCHANTMENT_DISCOVER
      value:
        enchantment_id: "ecoenchants:vampirism"
  
  # 环境友好附魔
  nature_blessing:
    name: "&a自然祝福"
    description:
      - "&7大自然赐予你特殊的力量！"
    discovered_on:
      type: ENCHANTMENT_DISCOVER
      value:
        enchantment_id: "ecoenchants:nature_blessing"
```

#### EcoEnchants 特殊功能

```yaml
discoveries:
  # 基于附魔组的发现
  combat_enchants_eco:
    name: "&eEco战斗专家"
    description:
      - "&7你精通EcoEnchants的战斗附魔！"
    discovered_on:
      type: ENCHANTMENT_DISCOVER
      value:
        # 匹配EcoEnchants的战斗组附魔
        eco_enchant_group: "combat"
        min_discovered: 3
```

### 自动图鉴集成

```yaml
settings:
  # 启用EcoEnchants信息获取
  fetch_ecoenchants_info: true
  
  filter:
    include_ecoenchants: true

# EcoEnchants附魔的自定义显示
enchantments:
  "ecoenchants:vampirism":
    name: "<dark_red>吸血"
    description:
      - "<gray>攻击时恢复生命值"
      - "<gray>等级越高恢复越多"
      - ""
      - "<green>EcoEnchants 提供"
  
  "ecoenchants:telekinesis":
    name: "<light_purple>心灵感应"
    description:
      - "<gray>破坏方块时直接收取物品"
      - "<gray>减少物品掉落造成的卡顿"
      - ""
      - "<green>EcoEnchants 提供"
```

## 混合附魔系统

### 多插件附魔检测

```yaml
# 附魔插件优先级配置
enchantment_priority:
  # 检测顺序（数字越小优先级越高）
  detection_order:
    1: "aiyatsbus"     # 优先检测 Aiyatsbus
    2: "ecoenchants"   # 其次检测 EcoEnchants
    3: "vanilla"       # 最后检测原版附魔
  
  # 冲突处理
  conflict_resolution:
    # 当多个插件都有同名附魔时的处理方式
    strategy: "namespace_prefix"  # namespace_prefix, first_match, merge
    
    # 命名空间前缀
    namespace_prefixes:
      aiyatsbus: "AY"
      ecoenchants: "ECO"
      vanilla: ""
```

### 统一附魔发现

```yaml
discoveries:
  # 跨插件的附魔大师称号
  enchantment_grandmaster:
    name: "&6附魔宗师"
    description:
      - "&7你掌握了来自各个插件的附魔知识！"
      - "&7无论是原版、Aiyatsbus还是EcoEnchants，"
      - "&7都无法逃脱你的掌控！"
    discovered_on:
      type: ENCHANTMENT_DISCOVER
      value:
        # 需要发现足够多的不同来源附魔
        total_enchantments: 50
        vanilla_enchantments: 15
        aiyatsbus_enchantments: 20
        ecoenchants_enchantments: 15
```

## 高级配置

### 附魔检测优化

```yaml
# 附魔检测性能配置
enchantment_detection:
  # 检测模式
  detection_mode: "smart"  # passive, active, smart
  
  # 智能检测配置
  smart_detection:
    # 检测触发器
    triggers:
      - "item_pickup"      # 拾取物品时检测
      - "inventory_click"  # 点击背包时检测
      - "enchant_table"    # 使用附魔台时检测
      - "anvil_use"        # 使用铁砧时检测
    
    # 检测冷却时间（毫秒）
    cooldown: 1000
    
    # 批量检测
    batch_detection: true
    batch_size: 10
  
  # 缓存配置
  cache:
    enabled: true
    max_size: 5000
    expire_time: 1800  # 30分钟
```

### 附魔分类系统

```yaml
# 附魔分类配置
enchantment_categories:
  # 自定义分类
  custom_categories:
    "utility":
      name: "实用附魔"
      enchantments:
        - "aiyatsbus:telekinesis"
        - "ecoenchants:telekinesis"
        - "minecraft:mending"
      
    "combat":
      name: "战斗附魔"
      enchantments:
        - "aiyatsbus:lifesteal"
        - "ecoenchants:vampirism"
        - "minecraft:sharpness"
    
    "protection":
      name: "防护附魔"
      enchantments:
        - "aiyatsbus:soul_bound"
        - "minecraft:protection"
        - "minecraft:unbreaking"
```

### 附魔信息增强

```yaml
# 附魔信息增强配置
enchantment_enhancement:
  # 显示额外信息
  show_extra_info: true
  
  extra_info:
    # 显示附魔来源
    show_source_plugin: true
    
    # 显示冲突附魔
    show_conflicts: true
    
    # 显示适用物品
    show_applicable_items: true
    
    # 显示获取方式
    show_obtain_methods: true
  
  # 信息格式化
  info_format:
    source_format: "<dark_gray>来源: <white>%plugin%"
    conflict_format: "<red>冲突: <white>%enchants%"
    applicable_format: "<blue>适用: <white>%items%"
```

## 调试与故障排除

### 调试模式

```yaml
# 附魔系统调试配置
debug:
  enchantment_system: true
  
  # 调试类别
  debug_categories:
    - "enchant_detection"
    - "plugin_hooks"
    - "enchant_conflicts"
    - "enchant_categories"
  
  # 详细日志
  verbose_logging: true
  log_file: "plugins/Codex/logs/enchantments.log"
```

### 常见问题

#### 问题：附魔插件未被检测到
**排查步骤**：
1. 确认插件版本兼容性
2. 检查插件加载顺序
3. 查看控制台启动信息
4. 验证插件API可用性

**解决方案**：
```yaml
# 强制启用附魔插件挂钩
force_enchant_hooks:
  aiyatsbus: true
  ecoenchants: true
  
# 手动注册附魔
manual_enchant_registry:
  "custom_enchant_1":
    plugin: "aiyatsbus"
    id: "telekinesis"
    name: "心灵手巧"
```

#### 问题：附魔识别不准确
**排查步骤**：
1. 检查附魔ID格式
2. 验证命名空间正确性
3. 查看附魔插件的实际配置
4. 测试原版附魔是否正常

**解决方案**：
```yaml
# 附魔ID映射
enchantment_mapping:
  "telekinesis": "aiyatsbus:telekinesis"
  "vampirism": "ecoenchants:vampirism"
  
# 别名支持
enchantment_aliases:
  "心灵手巧": "aiyatsbus:telekinesis"
  "吸血": "ecoenchants:vampirism"
```

#### 问题：性能影响
**解决方案**：
```yaml
# 性能优化配置
performance_optimization:
  # 减少检测频率
  detection_cooldown: 2000
  
  # 启用异步处理
  async_enchant_detection: true
  
  # 限制同时检测的附魔数量
  max_concurrent_detections: 5
  
  # 禁用某些耗时的检测
  disable_complex_detection: true
```

## 实际应用示例

### RPG服务器配置

```yaml
# 职业专精附魔系统
discoveries:
  # 法师专精
  mage_mastery:
    name: "&5法师专精"
    description:
      - "&7你掌握了法师专用的附魔技艺！"
    discovered_on:
      type: ENCHANTMENT_DISCOVER
      value:
        enchantment_categories: ["magic", "elemental"]
        min_enchants: 8
        required_enchantments:
          - "aiyatsbus:frost_aspect"
          - "ecoenchants:lightning"
  
  # 战士专精
  warrior_mastery:
    name: "&c战士专精"
    description:
      - "&7你精通战士的战斗附魔！"
    discovered_on:
      type: ENCHANTMENT_DISCOVER
      value:
        enchantment_categories: ["combat", "weapon"]
        min_enchants: 6
```

### PVP服务器配置

```yaml
# PVP专用附魔发现
discoveries:
  # 决斗大师
  duel_master:
    name: "&4决斗大师"
    description:
      - "&7你在PVP中展现了真正的实力！"
    discovered_on:
      type: ENCHANTMENT_DISCOVER
      value:
        # PVP相关的附魔
        required_enchantments:
          - "aiyatsbus:soul_bound"    # 防掉落
          - "aiyatsbus:lifesteal"     # 吸血
          - "ecoenchants:vampirism"   # 吸血
        min_pvp_kills: 10  # 需要额外的PVP击杀条件（需要其他插件支持）
```

## 下一步

了解了附魔插件集成后，您可以继续学习：

1. **[其他插件支持](other-plugins.md)** - WorldGuard、MythicMobs 等插件集成
2. **[附魔图鉴配置](../features/enchantment-codex.md)** - 深入了解附魔图鉴系统
3. **[配置优化](../configuration/gui-config.md)** - 界面和显示优化
4. **[性能调优](../admin/performance.md)** - 系统性能优化
