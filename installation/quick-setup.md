# 快速配置

本指南将帮助您在 5 分钟内完成 Codex 插件的基础配置，让您快速体验插件的核心功能。

## 配置前准备

确保您已经：
- ✅ 成功安装了 Codex 插件
- ✅ 服务器已重启并插件正常加载
- ✅ 具有管理员权限

## 步骤 1：基础配置调整

### 编辑主配置文件

打开 `plugins/Codex/config.yml`，进行以下关键配置：

```yaml
# 基础设置
settings:
  # 启用调试模式（建议初期开启）
  debug: true
  
  # 数据存储方式（建议使用 h2）
  storage_type: h2
  
  # 自动保存间隔（分钟）
  auto_save_interval: 15

# 默认奖励设置
default_rewards:
  # 发现奖励
  discovery:
    - "playsound: ENTITY_PLAYER_LEVELUP;1;1"
    - "console_command: experience add %player% 50 points"
    - "message: &e恭喜！你解锁了新的发现！"

# GUI 设置
gui:
  # 主菜单标题
  main_title: "&8[&6Codex&8] &7图鉴系统"
  
  # 每页显示的项目数
  items_per_page: 28
```

### 配置消息文件

编辑 `plugins/Codex/messages.yml`：

```yaml
# 基础消息
messages:
  # 命令消息
  no_permission: "&c你没有权限执行此命令！"
  reload_success: "&a配置文件已重新加载！"
  
  # 发现消息
  discovery_unlocked: "&e你发现了：&f%name%"
  progress_update: "&7进度：&e%unlocked%&7/&e%total%"
  
  # GUI 消息
  gui_title: "&8[&6Codex&8] &7%category%"
  no_discoveries: "&7暂无发现内容"
```

## 步骤 2：创建第一个发现

### 创建简单的物品发现

在 `plugins/Codex/categories/` 目录下创建 `items.yml`：

```yaml
# 物品发现分类配置
config:
  inventory_items:
    # 分类图标
    category:
      id: DIAMOND
      name: "&b物品收集"
      lore:
        - "&7收集各种物品来解锁发现"
        - "&7进度：&e%progress_bar%"
    
    # 已解锁发现的显示
    discovery_unlocked:
      id: "%material%"
      name: "%name%"
      lore:
        - "%description%"
        - ""
        - "&7首次获得：&f%date%"
    
    # 未解锁发现的显示
    discovery_blocked:
      id: GRAY_DYE
      name: "&8未知物品"
      lore:
        - "&7继续探索以解锁此发现"
  
  # 奖励配置
  rewards:
    per_discovery:
      - "playsound: BLOCK_NOTE_BLOCK_PLING;1;2"
      - "console_command: experience add %player% 25 points"
      - "message: &e发现新物品：&f%name%"

# 发现项目定义
discoveries:
  # 钻石发现
  first_diamond:
    name: "&b第一颗钻石"
    description:
      - "&7你获得了第一颗珍贵的钻石！"
      - "&7这是踏向强者之路的第一步。"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        item_type: DIAMOND
  
  # 铁锭发现
  first_iron:
    name: "&7第一块铁锭"
    description:
      - "&7铁是制作工具和装备的重要材料。"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        item_type: IRON_INGOT
  
  # 金锭发现
  first_gold:
    name: "&6第一块金锭"
    description:
      - "&7金光闪闪的金锭，象征着财富。"
    discovered_on:
      type: ITEM_OBTAIN
      value:
        item_type: GOLD_INGOT
```

### 创建生物击杀发现

创建 `plugins/Codex/categories/monsters.yml`：

```yaml
# 怪物击杀分类配置
config:
  inventory_items:
    category:
      id: IRON_SWORD
      name: "&c怪物猎人"
      lore:
        - "&7击败各种怪物来解锁发现"
        - "&7进度：&e%progress_bar%"
    
    discovery_unlocked:
      id: SKELETON_SKULL
      name: "%name%"
      lore:
        - "%description%"
        - ""
        - "&7首次击杀：&f%date%"
        - "&7击杀次数：&e%kill_count%"
    
    discovery_blocked:
      id: GRAY_DYE
      name: "&8未知怪物"
      lore:
        - "&7击杀怪物以解锁此发现"
  
  rewards:
    per_discovery:
      - "playsound: ENTITY_ENDER_DRAGON_GROWL;0.5;1"
      - "console_command: experience add %player% 100 points"
      - "message: &c击败了强敌：&f%name%"

discoveries:
  # 僵尸击杀
  zombie_killer:
    name: "&2僵尸杀手"
    description:
      - "&7你击败了第一只僵尸！"
      - "&7亡灵生物对你来说不再可怕。"
    discovered_on:
      type: MOB_KILL
      value:
        entity_type: ZOMBIE
  
  # 骷髅击杀
  skeleton_hunter:
    name: "&f骷髅猎人"
    description:
      - "&7精准射手？不如精准剑客！"
    discovered_on:
      type: MOB_KILL
      value:
        entity_type: SKELETON
  
  # 爬行者击杀
  creeper_defuser:
    name: "&a拆弹专家"
    description:
      - "&7成功击败爬行者而没有被炸飞！"
    discovered_on:
      type: MOB_KILL
      value:
        entity_type: CREEPER
```

## 步骤 3：启用自动图鉴（可选）

如果您已安装相关插件，可以快速启用自动图鉴：

### 启用附魔图鉴

编辑 `plugins/Codex/enchantments.yml`：

```yaml
settings:
  enabled: true
  auto_generate: true
  category_name: "enchantments"
  
  filter:
    include_vanilla: true
    include_aiyatsbus: true
    excluded: []

rewards:
  per_discovery:
    - "playsound: BLOCK_ENCHANTMENT_TABLE_USE;1;1"
    - "console_command: experience add %player% 30 points"
    - "message: &d发现新附魔：&f%name%"
```

### 启用钓鱼图鉴（需要 CustomFishing）

编辑 `plugins/Codex/fishing.yml`：

```yaml
settings:
  enabled: true
  auto_generate: true
  category_name: "fishing"

rewards:
  per_discovery:
    - "playsound: ENTITY_FISHING_BOBBER_SPLASH;1;1"
    - "console_command: experience add %player% 40 points"
    - "message: &b发现新鱼类：&f%name%"
```

## 步骤 4：重载配置

完成配置后，在游戏中执行：

```
/codex reload
```

您应该看到成功消息：`&a配置文件已重新加载！`

## 步骤 5：测试功能

### 测试物品发现

1. 给自己一些测试物品：
   ```
   /give @s diamond 1
   /give @s iron_ingot 1
   /give @s gold_ingot 1
   ```

2. 打开图鉴界面：
   ```
   /codex
   ```

3. 查看是否成功解锁相应发现

### 测试怪物击杀

1. 生成测试怪物：
   ```
   /summon zombie ~ ~ ~
   /summon skeleton ~ ~ ~
   ```

2. 击杀怪物并检查图鉴更新

## 配置验证清单

完成快速配置后，请确认以下项目：

- ✅ 插件成功加载并无错误
- ✅ 主配置文件设置正确
- ✅ 至少创建了一个发现分类
- ✅ 测试了基本的发现功能
- ✅ GUI界面可以正常打开
- ✅ 奖励系统正常工作

## 常见问题

### Q: 配置重载后没有变化
**A**: 检查YAML文件格式是否正确，确保缩进使用空格而非制表符。

### Q: 发现没有被触发
**A**: 检查触发条件配置，确认物品类型或实体类型名称正确。

### Q: GUI显示异常
**A**: 检查inventory.yml配置，确认物品ID和显示名称格式正确。

## 下一步

配置完成后，您可以：

1. **[第一次使用](first-use.md)** - 体验完整功能
2. **[基础配置详解](../configuration/basic-config.md)** - 深入了解配置选项
3. **[添加更多发现](../configuration/discovery-config.md)** - 创建更复杂的发现条件

## 获取更多帮助

- 📖 [配置指南](../configuration/basic-config.md)
- 🔧 [故障排除](../troubleshooting/configuration-issues.md)
- 💬 [社区支持](https://github.com/addpromax/plugin-gitbook-wiki)
