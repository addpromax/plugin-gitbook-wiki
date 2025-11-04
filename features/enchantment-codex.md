# 附魔图鉴

附魔图鉴是 Codex 插件的核心特色功能之一，它能够自动识别并记录玩家遇到的所有附魔，包括原版附魔和第三方附魔插件的自定义附魔。

## 功能概述

### 主要特性

- 🔮 **自动识别**：无需手动配置，自动检测游戏中的所有附魔
- 📊 **详细统计**：记录附魔的发现时间、等级信息、适用物品等
- 🎨 **美观界面**：精心设计的GUI界面，支持分页浏览
- 🔌 **插件兼容**：支持 Aiyatsbus、EcoEnchants 等附魔插件
- ⚡ **高性能**：优化的检测机制，最小化性能影响

### 工作原理

1. **事件监听**：监听物品交互、容器操作、附魔台使用等事件
2. **附魔检测**：分析物品上的附魔信息
3. **数据记录**：首次遇到新附魔时自动记录
4. **界面更新**：实时更新图鉴界面显示

## 基础配置

### 启用附魔图鉴

编辑 `plugins/Codex/enchantments.yml`：

```yaml
# 基础设置
settings:
  # 启用附魔图鉴功能
  enabled: true
  
  # 自动生成附魔图鉴条目
  auto_generate: true
  
  # 分类名称（在主菜单中显示）
  category_name: "enchantments"
  
  # 在主菜单中的位置
  main_menu_slot: 24
  
  # 附魔检测冷却时间（秒）
  check_cooldown: 5
  
  # 是否从容器中检测附魔
  detect_from_containers: true
  
  # 支持检测的容器类型
  container_types:
    - CHEST
    - BARREL
    - SHULKER_BOX
    - ENDER_CHEST
```

### 过滤设置

```yaml
settings:
  filter:
    # 包含原版附魔
    include_vanilla: true
    
    # 包含 Aiyatsbus 附魔（需要安装插件）
    include_aiyatsbus: true
    
    # 包含 EcoEnchants 附魔（需要安装插件）  
    include_ecoenchants: true
    
    # 排除的附魔列表（使用命名空间ID）
    excluded:
      - "minecraft:mending"
      - "minecraft:curse_of_vanishing"
```

## 界面配置

### GUI 外观设置

```yaml
# GUI 配置
gui:
  # 界面标题
  title: "<dark_red>Codex <gray>» <dark_gray>附魔图鉴"
  
  # 界面大小（必须是9的倍数）
  size: 54
  
  # 边框设置
  border:
    enabled: true
    item:
      id: PURPLE_STAINED_GLASS_PANE
      name: " "
  
  # 分页设置
  pagination:
    enabled: true
    items_per_page: 28
    
    # 上一页按钮
    previous_button:
      slot: 48
      item:
        id: PLAYER_HEAD
        name: "<gray>上一页"
        skull_texture: "eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvMzdhZWU5YTc1YmYwZGY3ODk3MTgzMDE1Y2NhMGIyYTdkNzU1YzYzMzg4ZmYwMTc1MmQ1ZjQ0MTlmYzY0NSJ9fX0="
    
    # 下一页按钮
    next_button:
      slot: 50
      item:
        id: PLAYER_HEAD
        name: "<gray>下一页"
        skull_texture: "eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvNjgyYWQxYjljYjRkZDIxMjU5YzBkNzVhYTMxNWZmMzg5YzNjZWY3NTJiZTM5NDkzMzgxNjRiYWM4NGE5NmUifX19"
    
    # 页面信息
    page_info:
      slot: 49
      item:
        id: PAPER
        name: "<gray>第 <yellow>%current_page% <gray>页，共 <yellow>%total_pages% <gray>页"
        lore:
          - "<gray>总共 <yellow>%total_items% <gray>个附魔"
```

### 显示模板配置

```yaml
# 显示配置
display:
  # 分类图标
  category_item:
    id: ENCHANTED_BOOK
    name: '<light_purple>附魔图鉴'
    lore:
      - '<gray>记录所有发现的附魔'
      - '<gray>发现进度: <yellow>%progress_bar%'
      - '<gray>已解锁: %unlocked%'
  
  # 已发现附魔的显示
  discovery_unlocked:
    id: ENCHANTED_BOOK
    name: "%name%"
    lore:
      - "%description%"
      - ""
      - "<color:#7289da>最大等级: <white>%max_level%"
      - "<color:#7289da>稀有度: <white>%rarity%"
      - ""
      - "<color:#7289da>适用物品:"
      - "%applicable_items%"
      - ""
      - "<color:#7289da>冲突附魔:"
      - "%conflicts%"
      - ""
      - "<color:#7289da>首次发现: <white>%date%"
  
  # 未发现附魔的显示
  discovery_blocked:
    id: BARRIER
    name: "<color:#8c8c8c>未知附魔"
    lore:
      - "<color:#8c8c8c>附魔物品或从书架中发现附魔"
```

## 高级配置

### 自定义附魔信息

您可以为特定附魔设置自定义名称和描述：

```yaml
# 自定义附魔配置
enchantments:
  "minecraft:sharpness":
    name: "<red>锋利"
    description:
      - "<gray>增加近战武器的伤害"
      - "<gray>每级增加0.5点额外伤害"
  
  "minecraft:protection":
    name: "<blue>保护"
    description:
      - "<gray>减少大部分类型的伤害"
      - "<gray>提供全面的防护效果"
  
  "aiyatsbus:telekinesis":
    name: "<purple>心灵手巧"
    description:
      - "<gray>破坏方块时直接收集到背包"
      - "<gray>Aiyatsbus插件提供"
```

### 显示模板定制

```yaml
# 描述模板配置
template:
  # 默认描述格式
  default_description:
    - "<gray>%description%"
    - ""
    - "<color:#7289da>最大等级: <white>%max_level%"
    - "<color:#7289da>稀有度: <white>%rarity%"
    - ""
    - "<color:#7289da>适用物品:"
    - "%applicable_items%"
    - ""
    - "<color:#7289da>冲突附魔:"
    - "%conflicts%"
  
  # 适用物品显示
  applicable_items:
    per_line: 3  # 每行显示的物品数量
    format: "<dark_gray>  ❱ <white>%item%"
    none: "<dark_gray>  ❱ <gray>无"
    too_many: "<dark_gray>  ❱ <gray>过多，无法全部显示"
    max_items: 15  # 最多显示的物品数量
  
  # 冲突附魔显示
  conflicts:
    per_line: 2  # 每行显示的冲突附魔数量
    format: "<dark_gray>  ❱ <white>%enchant%"
    none: "<dark_gray>  ❱ <gray>无"
    too_many: "<dark_gray>  ❱ <gray>过多，无法全部显示"
    max_conflicts: 10  # 最多显示的冲突附魔数量
  
  # 稀有度显示文本
  rarity:
    COMMON: "<white>普通"
    UNCOMMON: "<green>不常见"
    RARE: "<aqua>稀有"
    VERY_RARE: "<light_purple>非常稀有"
    MYTHIC: "<gold>神话"
    UNKNOWN: "<gray>未知"
    default: "<gray>未知"
```

## 奖励系统

### 发现奖励配置

```yaml
# 奖励配置
rewards:
  # 每次发现新附魔的奖励
  per_discovery:
    - "centered_message: <color:#a36bfe><strikethrough>                                                 "
    - "centered_message: "
    - "centered_message: <color:#eeeeee><bold>CODEX UPDATED"
    - "centered_message: <gray>附魔图鉴: %name%"
    - "centered_message: "
    - "centered_message: <color:#a36bfe><strikethrough>                                                 "
    - "playsound: BLOCK_ENCHANTMENT_TABLE_USE;1;1"
    - "console_command: experience add %player% 50 points"
  
  # 发现全部附魔的奖励
  all_discoveries:
    - "centered_message: <color:#a36bfe><strikethrough>                                                 "
    - "centered_message: "
    - "centered_message: <color:#eeeeee><bold>附魔大师!"
    - "centered_message: <gray>你已经发现了所有附魔!"
    - "centered_message: "
    - "centered_message: <color:#a36bfe><strikethrough>                                                 "
    - "playsound: UI_TOAST_CHALLENGE_COMPLETE;1;1"
    - "console_command: experience add %player% 500 points"
    - "console_command: give %player% enchanted_book 1"
```

### 可用变量

在附魔图鉴的奖励和显示中，您可以使用以下变量：

| 变量 | 说明 | 示例 |
|------|------|------|
| `%name%` | 附魔显示名称 | `锋利` |
| `%id%` | 附魔ID | `minecraft:sharpness` |
| `%description%` | 附魔描述 | `增加近战武器的伤害` |
| `%max_level%` | 最大等级 | `V` |
| `%rarity%` | 稀有度 | `普通` |
| `%date%` | 发现日期 | `2024-11-04` |
| `%time%` | 发现时间 | `14:30:25` |
| `%applicable_items%` | 适用物品列表 | `剑, 斧头, 三叉戟` |
| `%conflicts%` | 冲突附魔列表 | `尖刺, 节肢杀手` |

## 第三方插件集成

### Aiyatsbus 集成

Codex 会自动检测 Aiyatsbus 插件并集成其附魔：

```yaml
settings:
  # 启用 Aiyatsbus 信息获取
  fetch_aiyatsbus_info: true
  
  filter:
    # 包含 Aiyatsbus 附魔
    include_aiyatsbus: true
```

**Aiyatsbus 附魔示例**：
```yaml
enchantments:
  "aiyatsbus:telekinesis":
    name: "<purple>心灵手巧"
    description:
      - "<gray>破坏方块时物品直接进入背包"
  
  "aiyatsbus:auto_smelt":
    name: "<gold>自动熔炼"
    description:
      - "<gray>破坏方块时自动熔炼掉落物"
```

### EcoEnchants 集成

同样支持 EcoEnchants 插件的附魔：

```yaml
settings:
  fetch_ecoenchants_info: true
  
  filter:
    include_ecoenchants: true
```

### 检测机制

附魔检测支持多种方式：

1. **物品栏检测**：玩家打开背包时检测物品附魔
2. **装备检测**：玩家装备物品时检测
3. **容器检测**：打开箱子等容器时检测内部物品
4. **附魔台检测**：使用附魔台时检测结果
5. **铁砧检测**：使用铁砧合成时检测

```yaml
settings:
  # 检测冷却时间（避免频繁检测）
  check_cooldown: 5
  
  # 是否从容器检测附魔
  detect_from_containers: true
  
  # 支持的容器类型
  container_types:
    - CHEST
    - BARREL
    - SHULKER_BOX
    - ENDER_CHEST
    - HOPPER
    - DROPPER
    - DISPENSER
```

## 调试与优化

### 调试模式

启用调试模式查看附魔检测过程：

```yaml
# 在主配置文件中
settings:
  debug: true
```

调试信息包括：
- 检测到的附魔信息
- 过滤结果
- 性能统计
- 错误信息

### 性能优化

```yaml
settings:
  # 调整检测冷却时间
  check_cooldown: 10  # 增加冷却时间减少检测频率
  
  # 限制容器检测
  detect_from_containers: false  # 禁用容器检测提升性能
  
  # 减少支持的容器类型
  container_types:
    - CHEST  # 只检测箱子
```

### 内存管理

附魔图鉴会缓存附魔信息以提升性能：

```yaml
# 在主配置文件中
memory_management:
  # 缓存清理间隔（秒）
  cleanup_interval: 1800
  
  # 最大缓存条目数
  max_cache_size: 1000
```

## 常见问题

### Q: 某些附魔没有被检测到
**A**: 检查以下设置：
1. 确认 `auto_generate` 已启用
2. 检查附魔是否在 `excluded` 列表中
3. 验证第三方插件是否正确安装
4. 查看控制台是否有错误信息

### Q: 界面显示异常
**A**: 检查配置：
1. 确认 `gui.size` 是9的倍数
2. 检查物品ID是否正确
3. 验证颜色代码格式
4. 查看是否有YAML语法错误

### Q: 性能影响过大
**A**: 优化配置：
1. 增加 `check_cooldown` 值
2. 禁用 `detect_from_containers`
3. 减少支持的容器类型
4. 启用内存清理

## 使用示例

### 基础使用流程

1. **安装配置**：
   - 安装 Codex 插件
   - 启用附魔图鉴功能
   - 重载配置

2. **体验功能**：
   - 获取附魔书或附魔物品
   - 打开 `/codex` 界面
   - 点击附魔图鉴分类
   - 查看已发现的附魔

3. **自定义配置**：
   - 调整界面外观
   - 设置自定义附魔信息
   - 配置奖励系统

### 高级应用

**RPG服务器配置**：
```yaml
# 强调稀有附魔的重要性
rewards:
  per_discovery:
    - "broadcast: &e玩家 %player% 发现了稀有附魔：%name%!"
    - "console_command: give %player% diamond 5"
```

**教育服务器配置**：
```yaml
# 提供详细的附魔教学信息
template:
  default_description:
    - "%description%"
    - ""
    - "<gray>游戏机制："
    - "<gray>- 附魔等级越高效果越强"
    - "<gray>- 某些附魔之间会产生冲突"
    - "<gray>- 可通过附魔台或铁砧获得"
```

## 下一步

了解了附魔图鉴后，您可以继续学习：

1. **[钓鱼图鉴](fishing-codex.md)** - CustomFishing 集成
2. **[作物图鉴](crops-codex.md)** - CustomCrops 集成
3. **[第三方插件集成](../integrations/enchantment-plugins.md)** - 深度集成配置
4. **[界面配置](../configuration/gui-config.md)** - 自定义GUI外观
