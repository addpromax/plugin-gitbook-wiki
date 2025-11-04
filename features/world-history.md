# 世界历史系统

世界历史系统是 Codex 插件专为 RPG 服务器设计的独特功能，允许服务器管理员创建沉浸式的世界背景故事收集系统，让玩家通过探索和互动来逐步了解服务器的世界观和历史背景。

## 功能概述

### 主要特性

- 📚 **故事收集**：通过各种触发方式解锁世界历史片段
- 🎭 **沉浸体验**：丰富的文本描述和多媒体支持
- 🎯 **灵活触发**：支持所有 Codex 触发器类型
- 🏆 **进度追踪**：完整的收集进度和成就系统
- 🎨 **自定义外观**：完全可定制的界面和显示效果

### 适用场景

- **RPG 服务器**：构建完整的世界观和背景故事
- **冒险服务器**：创建探索驱动的故事线
- **教育服务器**：展示历史知识或学习材料
- **主题服务器**：增强特定主题的沉浸感

## 基础配置

### 配置文件结构

世界历史系统使用独立的配置文件 `plugins/Codex/categories/history.yml`：

```yaml
# ==========================================
# 世界历史配置 (支持 MiniMessage 格式)
# ==========================================

config:
  inventory_items:
    # 分类图标
    category:
      id: BOOK
      name: "<gray>Category: <color:#feb96b><bold>World History"
      lore:
        - "<color:#eeeeee>In your adventures you'll find some"
        - "<color:#eeeeee>interesting knowledge."
        - ""
        - "<gray>Unlocked: %unlocked% <dark_gray>[%progress_bar%<dark_gray>] <dark_gray>(<gray>%percentage%<dark_gray>)"
    
    # 已解锁的历史记录
    discovery_unlocked:
      id: PAPER
      name: "%name%"
      lore:
        - "%description%"
        - ""
        - "<dark_gray>Discovered on %date%"
    
    # 未解锁的历史记录
    discovery_blocked:
      id: GRAY_DYE
      name: "<red>??"
      lore:
        - "<gray>You haven't unlocked this discovery yet."
  
  # 发现奖励
  rewards:
    per_discovery:
      - "centered_message: <color:#feb96b><strikethrough>                                                 "
      - "centered_message: "
      - "centered_message: <color:#eeeeee><bold>CODEX UPDATED"
      - "centered_message: <gray>World History: %name%"
      - "centered_message: "
      - "centered_message: <gray>Check it now by using <color:#eeeeee>/codex"
      - "centered_message: "
      - "centered_message: <gray>Rewards: <green>+50XP"
      - "centered_message: "
      - "centered_message: <color:#feb96b><strikethrough>                                                 "
      - "title: 20;60;20;<color:#eeeeee><bold>CODEX UPDATED;<gray>World History: %name%"
      - "playsound: BLOCK_GILDED_BLACKSTONE_STEP;10;0.1"
      - "console_command: experience add %player% 50 points"
    
    # 完成所有历史记录的奖励
    all_discoveries:
      - "centered_message: <color:#feb96b><strikethrough>                                                 "
      - "centered_message: "
      - "centered_message: <color:#eeeeee><bold>LORE MASTER!"
      - "centered_message: <gray>You have discovered all world history!"
      - "centered_message: "
      - "centered_message: <color:#feb96b><strikethrough>                                                 "
      - "playsound: UI_TOAST_CHALLENGE_COMPLETE;1;1"
      - "console_command: experience add %player% 500 points"
      - "console_command: give %player% written_book 1"

# ==========================================
# 历史记录定义
# ==========================================
discoveries:
  # 在这里定义具体的历史记录
```

## 历史记录创建

### 基础历史记录

```yaml
discoveries:
  # 古代文明记录
  ancient_kingdom:
    name: "<color:#feb96b><bold>古老王国的兴衰"
    description:
      - "<color:#eeeeee>在遥远的过去，这片大陆上曾经存在着"
      - "<color:#eeeeee>一个强大的王国。他们掌握着古老的魔法，"
      - "<color:#eeeeee>建造了许多宏伟的建筑..."
      - ""
      - "<color:#eeeeee>然而，贪婪和权力的争斗最终导致了"
      - "<color:#eeeeee>这个伟大文明的衰落。"
    discovered_on:
      type: WORLDGUARD_REGION
      value:
        region_name: "ancient_ruins"
        world_name: "world"
  
  # 神秘事件记录  
  mysterious_incident:
    name: "<color:#feb96b><bold>神秘的红月事件"
    description:
      - "<color:#eeeeee>据古老的文献记载，每隔千年"
      - "<color:#eeeeee>天空会出现血红色的月亮。"
      - ""
      - "<color:#eeeeee>在红月之夜，死者会短暂复活，"
      - "<color:#eeeeee>而生者则会获得神秘的力量..."
    discovered_on:
      type: ITEM_OBTAIN
      value:
        item_type: "REDSTONE"
        custom_model_data: 100001
```

### 多触发条件历史记录

```yaml
discoveries:
  # 需要多个条件才能解锁的高级历史
  elder_scroll_knowledge:
    name: "<color:#feb96b><bold>上古卷轴的秘密"
    description:
      - "<color:#eeeeee>上古卷轴记载着世界创造的真相。"
      - "<color:#eeeeee>只有同时拥有三件神器的人，"
      - "<color:#eeeeee>才能解读其中的奥秘..."
    discovered_on:
      type: ITEM_OBTAIN
      value:
        # 需要同时拥有多个物品
        item_type: "WRITTEN_BOOK"
        components: "elder_scroll:ancient;elder_scroll:sealed"
        required_discoveries:
          - "ancient_kingdom"
          - "mysterious_incident"
```

### 按章节组织的历史

```yaml
discoveries:
  # 第一章：起源
  chapter_1_genesis:
    name: "<color:#feb96b><bold>第一章：世界的起源"
    description:
      - "<color:#eeeeee>在混沌的虚无中，第一道光芒诞生了。"
      - "<color:#eeeeee>光芒凝聚成大地，黑暗化作天空，"
      - "<color:#eeeeee>生命在两者之间萌芽..."
    discovered_on:
      type: COMMAND_RUN
      value:
        command: "lore start"
  
  # 第二章：文明
  chapter_2_civilization:
    name: "<color:#feb96b><bold>第二章：文明的曙光"  
    description:
      - "<color:#eeeeee>第一批智慧生物在大地上建立了聚落。"
      - "<color:#eeeeee>他们学会了使用工具，驯化动物，"
      - "<color:#eeeeee>并开始书写历史..."
    discovered_on:
      type: ITEM_OBTAIN
      value:
        item_type: "WRITTEN_BOOK"
        required_discoveries: ["chapter_1_genesis"]
```

## 高级功能

### 条件性历史记录

```yaml
discoveries:
  # 只在特定时间解锁
  midnight_legend:
    name: "<color:#feb96b><bold>午夜传说"
    description:
      - "<color:#eeeeee>传说在午夜时分，亡灵会在墓地中游荡..."
    discovered_on:
      type: MOB_KILL
      value:
        entity_type: "ZOMBIE"
        time_range: "18000-6000"  # 夜晚时间
        world: "world"
  
  # 需要特定权限的历史
  secret_archives:
    name: "<color:#feb96b><bold>秘密档案"
    description:
      - "<color:#eeeeee>只有高级权限的人才能访问的机密信息..."
    discovered_on:
      type: COMMAND_RUN
      value:
        command: "archives access"
        required_permission: "server.admin"
```

### 连续性故事线

```yaml
discoveries:
  # 故事线：失落的王子
  prince_story_1:
    name: "<color:#feb96b><bold>失落王子的踪迹（一）"
    description:
      - "<color:#eeeeee>年轻的王子在一次狩猎中神秘失踪。"
      - "<color:#eeeeee>他的马匹在森林边缘被发现，"
      - "<color:#eeeeee>但王子本人却杳无音信..."
    discovered_on:
      type: WORLDGUARD_REGION
      value:
        region_name: "lost_forest"
  
  prince_story_2:
    name: "<color:#feb96b><bold>失落王子的踪迹（二）"
    description:
      - "<color:#eeeeee>在森林深处发现了王子的佩剑。"
      - "<color:#eeeeee>剑身上的血迹已经干涸，"
      - "<color:#eeeeee>但剑柄上刻着神秘的符号..."
    discovered_on:
      type: ITEM_OBTAIN
      value:
        item_type: "DIAMOND_SWORD"
        components: "royal:prince_sword"
        required_discoveries: ["prince_story_1"]
  
  prince_story_final:
    name: "<color:#feb96b><bold>失落王子的真相"
    description:
      - "<color:#eeeeee>经过调查，你发现王子并非遇害，"
      - "<color:#eeeeee>而是为了寻找传说中的永生之泉"
      - "<color:#eeeeee>而主动踏上了危险的旅程..."
    discovered_on:
      type: ITEM_OBTAIN
      value:
        item_type: "WATER_BUCKET"
        components: "eternal:fountain_water"
        required_discoveries: ["prince_story_1", "prince_story_2"]
```

## 界面定制

### 自定义显示效果

```yaml
config:
  inventory_items:
    category:
      id: ENCHANTED_BOOK
      name: "<gradient:#ffd700:#ff6b35><bold>世界编年史"
      lore:
        - "<color:#eeeeee>记录着这个世界的传奇故事"
        - "<color:#eeeeee>每一个故事都蕴含着深刻的智慧"
        - ""
        - "<gray>收集进度: <color:#ffd700>%progress_bar%"
        - "<gray>已发现: <color:#ff6b35>%unlocked%<gray>/<color:#ffd700>%total%"
        - "<gray>知识等级: %knowledge_level%"
    
    discovery_unlocked:
      id: PAPER
      name: "%name%"
      lore:
        - "%description%"
        - ""
        - "<color:#7289da>记录类型: <white>%record_type%"
        - "<color:#7289da>发现日期: <white>%date%"
        - "<color:#7289da>历史时期: <white>%era%"
        - ""
        - "<italic><color:#888888>\"历史是最好的老师\""
```

### 分章节显示

```yaml
# 支持按章节组织历史记录
display:
  # 章节分组
  chapters:
    enabled: true
    chapter_indicator: "📖"
    
    # 章节名称映射
    chapter_names:
      "chapter_1": "第一章：创世纪"
      "chapter_2": "第二章：文明兴起"
      "chapter_3": "第三章：战争年代"
      "chapter_4": "第四章：和平时期"
      "chapter_5": "第五章：末日预言"
  
  # 进度指示器
  progress:
    chapter_format: "%chapter_name%: %unlocked%/%total%"
    overall_format: "总进度: %unlocked%/%total% (%percentage%%)"
```

## 多媒体支持

### 富文本格式

世界历史系统完全支持 MiniMessage 格式：

```yaml
discoveries:
  colorful_history:
    name: "<gradient:#ff0000:#00ff00:#0000ff><bold>彩虹之战"
    description:
      - "<rainbow>传说中的彩虹之战改变了世界的颜色</rainbow>"
      - ""
      - "<color:#ff0000>红色军团</color>代表着火焰与激情"
      - "<color:#00ff00>绿色联盟</color>象征着自然与生命"  
      - "<color:#0000ff>蓝色帝国</color>掌握着水与智慧"
      - ""
      - "<italic><color:#888888>直到今天，天空中还能看到"
      - "<italic><color:#888888>那场战争留下的彩虹痕迹..."
```

### 自定义变量

```yaml
# 可以在历史记录中使用自定义变量
custom_variables:
  # 世界设定变量
  world_name: "艾泽拉斯"
  current_year: "第三纪元 1247年"
  current_ruler: "阿尔萨斯国王"
  
  # 玩家相关变量
  player_title: "%player_title%"  # 需要其他插件支持
  player_guild: "%player_guild%"
  
discoveries:
  current_events:
    name: "<color:#feb96b><bold>当代史记录"
    description:
      - "<color:#eeeeee>在 %current_year%，%current_ruler% 统治着 %world_name%"
      - "<color:#eeeeee>而你，%player%，作为一名 %player_title%"
      - "<color:#eeeeee>正在书写属于自己的传奇..."
```

## 奖励系统

### 知识等级系统

```yaml
# 知识等级配置
knowledge_system:
  enabled: true
  
  # 等级定义
  levels:
    1:
      name: "学徒史学家"
      required_discoveries: 5
      rewards:
        - "title: 20;60;20;&e学徒史学家;&7开始你的知识之旅"
        - "console_command: experience add %player% 100 points"
    
    2:
      name: "见习学者"
      required_discoveries: 15
      rewards:
        - "console_command: give %player% book 5"
        - "message: &e恭喜！你已成为见习学者！"
    
    3:
      name: "博学之士"
      required_discoveries: 30
      rewards:
        - "console_command: give %player% enchanted_book 3"
        - "broadcast: &6%player% 成为了博学之士！"
```

### 主题完成奖励

```yaml
# 按主题给予奖励
theme_rewards:
  # 古代文明主题
  ancient_civilization:
    discoveries:
      - "ancient_kingdom"
      - "lost_empire"  
      - "forgotten_magic"
    rewards:
      - "title: 20;60;20;&6考古学家;&7古代文明专家"
      - "console_command: give %player% ancient_relic 1"
  
  # 英雄传说主题
  hero_legends:
    discoveries:
      - "prince_story_final"
      - "dragon_slayer_tale"
      - "wise_wizard_story"
    rewards:
      - "title: 20;60;20;&c传奇编年史家;&7英雄故事收藏家"
      - "console_command: give %player% hero_token 1"
```

## 管理工具

### 故事管理命令

```bash
# 查看玩家历史记录进度
/codex history <玩家> [主题]

# 强制解锁历史记录
/codex history unlock <玩家> <记录ID>

# 重置历史记录进度  
/codex history reset <玩家> [主题]

# 导出历史记录数据
/codex history export <玩家>
```

### 内容管理

```yaml
# 内容管理配置
content_management:
  # 支持热重载
  hot_reload: true
  
  # 内容验证
  validation:
    max_description_lines: 20
    max_line_length: 80
    required_fields: ["name", "description", "discovered_on"]
  
  # 版本控制
  versioning:
    enabled: true
    backup_on_change: true
    max_backups: 10
```

## 使用示例

### RPG 服务器应用

```yaml
# 魔幻主题服务器的世界历史
discoveries:
  creation_myth:
    name: "<gradient:#ffd700:#ff6b35><bold>创世神话"
    description:
      - "<color:#eeeeee>在时间的起始，七位创世神聚集在虚无中"
      - "<color:#eeeeee>光明之神 <color:#ffd700>卢米诺斯</color> 创造了太阳与星辰"
      - "<color:#eeeeee>大地之神 <color:#8b4513>特拉玛特</color> 塑造了山川河流"
      - "<color:#eeeeee>而生命之神 <color:#90ee90>维塔莉亚</color> 则赋予了万物生机"
    discovered_on:
      type: WORLDGUARD_REGION
      value:
        region_name: "creation_altar"
```

### 教育服务器应用

```yaml
# 历史教学服务器的内容
discoveries:
  industrial_revolution:
    name: "<color:#feb96b><bold>工业革命的影响"
    description:
      - "<color:#eeeeee>18世纪后期开始的工业革命"
      - "<color:#eeeeee>彻底改变了人类的生产方式"
      - ""
      - "<color:#eeeeee>主要特征："
      - "<color:#eeeeee>• 机器大生产取代手工劳动"
      - "<color:#eeeeee>• 城市化进程加速"
      - "<color:#eeeeee>• 社会结构发生重大变化"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        item_type: "IRON_INGOT"
        amount: 64
```

## 下一步

了解了世界历史系统后，您可以继续学习：

1. **[数据存储系统](data-storage.md)** - 配置数据库和存储选项
2. **[配置指南](../configuration/discovery-config.md)** - 深入了解发现配置
3. **[界面定制](../configuration/gui-config.md)** - 自定义GUI外观
4. **[奖励系统](../configuration/rewards-config.md)** - 配置复杂奖励机制
