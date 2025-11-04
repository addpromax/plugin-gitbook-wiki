# 作物图鉴

作物图鉴是 Codex 插件与 CustomCrops 插件的深度集成功能，能够自动追踪玩家的农业活动，记录作物种植、收获数据、季节信息等，为农业爱好者提供完整的种植体验。

## 功能概述

### 主要特性

- 🌾 **自动记录**：无需手动配置，自动记录所有种植和收获的作物
- 📊 **农业统计**：记录每种作物的种植次数、收获次数、产量等
- 🌸 **季节系统**：显示作物的适宜种植季节和生长条件
- 🎁 **收获物追踪**：记录每种作物的所有收获物品
- 🏆 **成就奖励**：发现新作物或完成收集时获得奖励

### 前置要求

- ✅ 安装 [CustomCrops](https://www.spigotmc.org/resources/customcrops.103964/) 插件
- ✅ 确保 CustomCrops 正常运行
- ✅ 启用 Codex 作物图鉴功能

## 基础配置

### 启用作物图鉴

编辑 `plugins/Codex/crops.yml`：

```yaml
# 基础设置
settings:
  # 启用作物图鉴功能
  enabled: true
  
  # 自动从 CustomCrops 读取作物数据
  auto_generate: true
  
  # 分类名称（在主菜单中显示）
  category_name: "crops"
  
  # 在主菜单中的位置
  main_menu_slot: 23
  
  # 排除的作物ID列表（不记录这些作物）
  excluded_crops: []
```

### 智能过滤系统

作物图鉴支持多种过滤方式，避免记录不必要的物品：

```yaml
settings:
  # 排除包含指定关键词的物品（如种子）
  excluded_item_keywords:
    - "seed"         # 种子
    - "_seeds"       # 复数种子
    - "seedling"     # 幼苗
    - "sapling"      # 树苗
    - "bulb"         # 球茎
    - "sprout"       # 新芽
  
  # 排除特定作物ID
  excluded_crops:
    - "test_crop"
    - "debug_plant"
  
  # 只记录真实作物（排除装饰性植物）
  only_harvestable: true
  
  # 最小收获物数量（少于此数量的不记录）
  min_harvest_items: 1
```

## 界面配置

### GUI 外观设置

```yaml
# 显示配置
display:
  # 分类图标
  category_item:
    id: WHEAT_SEEDS
    name: '&a作物图鉴'
    lore:
      - '&7记录所有可种植的作物'
      - '&7发现进度: &e%progress_bar%'
      - '&7已解锁: %unlocked%/%total%'
      - '&7完成度: &e%percentage%%'
  
  # 已发现作物的显示
  discovery_unlocked:
    id: WHEAT  # 默认图标，会被作物本身的图标覆盖
    name: "%name%"
    lore:
      - "%description%"
      - ""
      - "#7289da种植季节: #ffffff%seasons%"
      - "#7289da生长时间: #ffffff%growth_time%"
      - ""
      - "#7289da收获物品:"
      - "%harvest_items%"
      - ""
      - "#7289da首次种植: #ffffff%date%"
      - "#7289da种植次数: #ffffff%plant_count%次"
      - "#7289da收获次数: #ffffff%harvest_count%次"
      - "#7289da总产量: #ffffff%total_yield%个"
  
  # 未发现作物的显示
  discovery_blocked:
    id: BARRIER
    name: "#8c8c8c未知作物"
    lore:
      - "#8c8c8c种植或收获以发现这种作物"
      - ""
      - "#7289da种植季节: #ffffff%seasons%"
      - "#7289da生长条件: #ffffff%conditions%"
```

### 收获物显示配置

```yaml
display:
  # 收获物品显示格式
  harvest_items:
    format: "#ffffff  ❱ %item_name% x%amount%"
    max_items: 8  # 最多显示的收获物种类
    too_many_text: "#ffffff  ❱ ...以及其他 %count% 种物品"
  
  # 季节显示配置
  seasons:
    format: "#ffffff%season%"
    separator: "#7289da, "
    all_seasons_text: "全季节"
    
    # 季节名称映射
    season_names:
      SPRING: "春季"
      SUMMER: "夏季"
      AUTUMN: "秋季"
      FALL: "秋季"
      WINTER: "冬季"
  
  # 生长时间显示
  growth_time:
    format: "#ffffff%time%"
    units:
      days: "天"
      hours: "小时"
      minutes: "分钟"
      seconds: "秒"
```

## 数据追踪功能

### 记录的数据类型

作物图鉴会为每种作物记录以下详细信息：

1. **基础信息**：
   - 作物名称和描述
   - CustomCrops 中的原始ID
   - 作物图标和外观

2. **种植数据**：
   - 首次种植日期和时间
   - 总种植次数
   - 最近种植时间
   - 种植成功率

3. **收获数据**：
   - 总收获次数
   - 总收获产量
   - 平均产量
   - 最大单次产量

4. **生长信息**：
   - 适宜种植季节
   - 生长所需时间
   - 生长条件要求
   - 所有收获物品列表

5. **环境信息**：
   - 适宜的生物群系
   - 土壤要求
   - 气候条件
   - 特殊要求

### 统计变量

在配置中可以使用以下变量：

| 变量 | 说明 | 示例 |
|------|------|------|
| `%name%` | 作物名称 | `小麦` |
| `%description%` | 作物描述 | `基础农作物...` |
| `%date%` | 首次种植日期 | `2024-11-04` |
| `%time%` | 首次种植时间 | `14:30:25` |
| `%plant_count%` | 种植次数 | `25` |
| `%harvest_count%` | 收获次数 | `23` |
| `%total_yield%` | 总产量 | `147` |
| `%avg_yield%` | 平均产量 | `6.4` |
| `%seasons%` | 适宜季节 | `春季, 夏季` |
| `%growth_time%` | 生长时间 | `3天 12小时` |
| `%harvest_items%` | 收获物列表 | `小麦 x3, 种子 x1` |
| `%conditions%` | 生长条件 | `需要水源, 阳光充足` |

## 奖励系统

### 发现奖励配置

```yaml
# 发现奖励
rewards:
  # 每次发现新作物的奖励
  per_discovery:
    - "centered_message: #32CD32&m                                                 "
    - "centered_message: "
    - "centered_message: #eeeeee&lCODEX UPDATED"
    - "centered_message: &7作物图鉴: %name%"
    - "centered_message: &7种植季节: %seasons%"
    - "centered_message: "
    - "centered_message: #32CD32&m                                                 "
    - "playsound: ENTITY_VILLAGER_YES;1;1.2"
    - "console_command: experience add %player% 30 points"
    - "title: 20;40;20;&a新作物发现!;&7%name%"
  
  # 发现全部作物的奖励
  all_discoveries:
    - "centered_message: #32CD32&m                                                 "
    - "centered_message: "
    - "centered_message: #eeeeee&l农业大师!"
    - "centered_message: &7你已经发现了所有作物!"
    - "centered_message: &7真正的农业专家!"
    - "centered_message: "
    - "centered_message: #32CD32&m                                                 "
    - "playsound: UI_TOAST_CHALLENGE_COMPLETE;1;1"
    - "console_command: experience add %player% 800 points"
    - "console_command: give %player% golden_hoe{display:{Name:'{\"text\":\"大师级锄头\",\"color\":\"gold\"}'},Enchantments:[{id:\"minecraft:efficiency\",lvl:5}]} 1"
```

### 按季节分类奖励

```yaml
# 高级奖励配置 - 根据季节给不同奖励
rewards:
  per_discovery:
    # 春季作物
    - "condition: %seasons% contains 春季"
    - "console_command: experience add %player% 50 points"
    - "message: &a春意盎然！发现了春季作物 %name%"
    
    # 全季节作物（更珍贵）
    - "condition: %seasons% == 全季节"
    - "console_command: experience add %player% 100 points"
    - "console_command: give %player% emerald 2"
    - "broadcast: &e玩家 %player% 发现了珍贵的全季节作物：%name%!"
```

### 产量里程碑奖励

```yaml
# 产量里程碑奖励
milestone_rewards:
  # 收获里程碑
  harvest_milestones:
    10:
      - "message: &e收获达人！你已经收获了 10 次 %name%"
      - "console_command: experience add %player% 100 points"
    
    50:
      - "message: &6专业农夫！你已经收获了 50 次 %name%"
      - "console_command: experience add %player% 500 points"
      - "console_command: give %player% diamond 1"
    
    100:
      - "title: 20;60;20;&6农业专家!;&e%name% 收获大师"
      - "broadcast: &6%player% 成为了 %name% 的收获大师！"
      - "console_command: give %player% nether_star 1"
```

## CustomCrops 集成

### 自动数据读取

Codex 会自动从 CustomCrops 读取以下数据：

1. **作物配置**：
   - 从作物配置文件读取基本信息
   - 包括名称、描述、图标、生长时间等

2. **季节信息**：
   - 读取作物的适宜种植季节
   - 解析季节系统配置

3. **收获配置**：
   - 解析收获物品配置
   - 获取收获概率和数量范围

4. **生长条件**：
   - 读取土壤、气候、水源等要求
   - 解析特殊生长条件

### 兼容性检查

插件会在启动时检查 CustomCrops 兼容性：

```
[Codex] CustomCrops Hook: 成功
[Codex] - 检测到 CustomCrops 版本: 3.6.44
[Codex] - 已加载作物数量: 32
[Codex] - 已加载季节数量: 4
[Codex] - 支持的生物群系: 18
[Codex] - 启用季节系统: true
```

### 事件监听

作物图鉴监听以下 CustomCrops 事件：

- **种植事件**：`CropPlantEvent`
- **收获事件**：`CropHarvestEvent`
- **生长事件**：`CropGrowEvent`
- **破坏事件**：`CropBreakEvent`

## 高级配置

### 季节系统配置

```yaml
# 季节系统配置
seasons:
  # 启用季节追踪
  enabled: true
  
  # 季节检测模式
  detection_mode: "auto"  # auto, manual, plugin
  
  # 手动季节映射（当mode为manual时）
  manual_seasons:
    "1-3": "SPRING"    # 1-3月为春季
    "4-6": "SUMMER"    # 4-6月为夏季
    "7-9": "AUTUMN"    # 7-9月为秋季
    "10-12": "WINTER"  # 10-12月为冬季
  
  # 季节显示格式
  display:
    show_current_season: true
    season_indicator: " ⭐"  # 当前季节的标记
```

### 生长条件配置

```yaml
# 生长条件配置
growth_conditions:
  # 显示生长条件
  show_conditions: true
  
  # 条件类型映射
  condition_names:
    "water_nearby": "需要水源"
    "sunlight": "需要阳光"
    "specific_biome": "特定生物群系"
    "fertile_soil": "肥沃土壤"
    "temperature": "适宜温度"
  
  # 条件显示格式
  format: "#7289da  • #ffffff%condition%"
  no_conditions: "#7289da  • #ffffff无特殊要求"
```

### 产量统计配置

```yaml
# 产量统计配置
yield_tracking:
  # 启用详细产量统计
  enabled: true
  
  # 记录单次收获详情
  track_individual_harvests: true
  
  # 统计保留时间（天）
  retention_days: 90
  
  # 产量排行榜
  leaderboard:
    enabled: true
    max_entries: 10
    update_interval: 3600  # 1小时更新一次
```

## 调试与故障排除

### 调试模式

启用调试模式查看详细信息：

```yaml
# 在主配置文件中
settings:
  debug: true
```

调试信息包括：
- CustomCrops 挂钩状态
- 作物数据加载过程
- 种植和收获事件处理
- 季节系统状态

### 常见问题

#### 问题：作物图鉴不显示任何作物
**排查步骤**：
1. 确认 CustomCrops 插件已正确安装
2. 检查控制台是否显示挂钩成功
3. 验证 `auto_generate` 是否启用
4. 查看是否有作物被过滤掉

#### 问题：种植事件没有被记录
**排查步骤**：
1. 检查是否使用了 CustomCrops 的作物
2. 确认种植事件是否被正确监听
3. 查看作物是否在排除列表中
4. 检查权限和区域保护

#### 问题：季节信息显示错误
**排查步骤**：
1. 检查 CustomCrops 季节系统是否启用
2. 验证季节配置是否正确
3. 确认时间同步设置
4. 查看季节检测模式配置

### 性能优化

```yaml
# 性能优化配置
performance:
  # 事件处理延迟（毫秒）
  event_delay: 50
  
  # 批量保存间隔（秒）
  batch_save_interval: 300
  
  # 最大缓存作物数量
  max_cached_crops: 500
  
  # 是否异步处理事件
  async_processing: true
  
  # 数据清理间隔（小时）
  cleanup_interval: 24
```

## 使用示例

### 基础使用流程

1. **配置启用**：
   ```yaml
   settings:
     enabled: true
     auto_generate: true
   ```

2. **开始农业**：
   - 使用 CustomCrops 提供的作物种子
   - 在适宜的季节和环境种植
   - 收获成熟的作物

3. **查看图鉴**：
   - 输入 `/codex` 打开主界面
   - 点击作物图鉴分类
   - 浏览已发现的作物信息

### 高级应用示例

**农业服务器**：
```yaml
rewards:
  per_discovery:
    - "broadcast: &a%player% 发现了新作物 %name%! (季节: %seasons%)"
    - "console_command: eco give %player% 500"
    - "console_command: jobs addexp %player% Farmer 100"
```

**RPG 服务器**：
```yaml
# 根据作物稀有度给予不同奖励
rewards:
  per_discovery:
    - "condition: %seasons% == 全季节"
    - "console_command: mcmmo addxp %player% herbalism 200"
    - "console_command: give %player% emerald 3"
```

## 下一步

了解了作物图鉴后，您可以继续学习：

1. **[世界历史系统](world-history.md)** - RPG 背景故事收集
2. **[数据存储系统](data-storage.md)** - 配置数据库和存储
3. **[第三方插件集成](../integrations/other-plugins.md)** - 更多插件集成
4. **[界面配置](../configuration/gui-config.md)** - 自定义GUI外观
