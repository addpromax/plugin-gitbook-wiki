# 基础配置

本章详细介绍 Codex 插件的基础配置选项，帮助您根据服务器需求进行个性化设置。

## 主配置文件

### config.yml 结构

主配置文件 `plugins/Codex/config.yml` 包含插件的核心设置：

```yaml
# ==========================================
# Codex 主配置文件
# ==========================================

# 基础设置
settings:
  # 插件语言
  language: "zh-cn"  # 支持: en, zh-cn, zh-tw, ja, ko
  
  # 调试模式
  debug: false
  
  # 检查更新
  check_updates: true
  
  # 使用统计（匿名数据收集）
  metrics: true
  
  # 插件前缀
  prefix: "&8[&6Codex&8]"

# 数据存储配置
storage:
  # 存储类型: file, h2, mysql
  type: "h2"
  
  # 自动保存
  auto_save:
    enabled: true
    interval: 900  # 15分钟（秒）
    on_shutdown: true
    on_player_quit: true

# 性能设置
performance:
  # 异步处理
  async_operations: true
  
  # 线程池大小
  thread_pool_size: 4
  
  # 批处理设置
  batch_processing:
    enabled: true
    batch_size: 100
    timeout: 5000  # 毫秒

# 发现系统设置
discovery:
  # 全局冷却时间（毫秒）
  global_cooldown: 1000
  
  # 每个玩家的发现冷却时间（毫秒）
  player_cooldown: 500
  
  # 最大并发发现检查数
  max_concurrent_checks: 10
  
  # 发现缓存
  cache:
    enabled: true
    max_size: 10000
    expire_time: 3600  # 1小时
```

## 语言配置

### 多语言支持

Codex 支持多种语言，可以通过修改语言设置来切换：

```yaml
settings:
  # 可选语言
  language: "zh-cn"  # 简体中文
  # language: "zh-tw"  # 繁体中文
  # language: "en"     # 英语
  # language: "ja"     # 日语
  # language: "ko"     # 韩语
```

### 自定义语言文件

您可以创建自定义语言文件 `plugins/Codex/languages/custom.yml`：

```yaml
# 自定义语言文件
language:
  name: "自定义中文"
  version: "1.0.0"

# 基础消息
messages:
  # 命令消息
  no_permission: "&c你没有权限执行此命令！"
  reload_success: "&a配置文件重载成功！"
  player_not_found: "&c找不到玩家：%player%"
  
  # 发现消息
  discovery_unlocked: "&e新发现解锁：&f%name%"
  discovery_duplicate: "&7你已经解锁过这个发现了"
  category_complete: "&a恭喜！你完成了 &e%category% &a分类！"
  
  # GUI消息
  gui_title: "&8[&6Codex&8] &7%category%"
  gui_previous_page: "&7上一页"
  gui_next_page: "&7下一页"
  gui_close: "&c关闭"
  gui_back: "&7返回"
  
  # 进度消息
  progress_format: "&7进度：&e%unlocked%&7/&e%total% &7(&e%percentage%%&7)"
  progress_bar_completed: "&a█"
  progress_bar_remaining: "&7█"
  
  # 错误消息
  error_database: "&c数据库错误：%error%"
  error_config: "&c配置文件错误：%error%"
  error_unknown: "&c发生未知错误，请联系管理员"

# 时间格式
time:
  formats:
    date: "yyyy-MM-dd"
    time: "HH:mm:ss"
    datetime: "yyyy-MM-dd HH:mm:ss"
  
  units:
    year: "年"
    month: "月"
    day: "天"
    hour: "小时"
    minute: "分钟"
    second: "秒"

# 数字格式
numbers:
  decimal_separator: "."
  thousand_separator: ","
  currency_symbol: "¥"
```

## GUI 配置

### 界面基础设置

编辑 `plugins/Codex/inventory.yml`：

```yaml
# ==========================================
# GUI 界面配置
# ==========================================

# 主界面设置
main_gui:
  # 界面标题
  title: "&8[&6Codex&8] &7图鉴系统"
  
  # 界面大小（必须是9的倍数）
  size: 54
  
  # 背景填充
  background:
    enabled: true
    item:
      id: GRAY_STAINED_GLASS_PANE
      name: " "
  
  # 边框设置
  border:
    enabled: true
    item:
      id: BLACK_STAINED_GLASS_PANE
      name: " "

# 分类界面设置
category_gui:
  # 默认界面大小
  default_size: 54
  
  # 分页设置
  pagination:
    enabled: true
    items_per_page: 28
    
    # 分页按钮位置
    previous_slot: 45
    next_slot: 53
    info_slot: 49
  
  # 导航按钮
  navigation:
    back_button:
      enabled: true
      slot: 48
      item:
        id: ARROW
        name: "&7返回主菜单"

# 通用GUI元素
common_elements:
  # 上一页按钮
  previous_page:
    id: PLAYER_HEAD
    name: "&7上一页"
    skull_texture: "eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvMzdhZWU5YTc1YmYwZGY3ODk3MTgzMDE1Y2NhMGIyYTdkNzU1YzYzMzg4ZmYwMTc1MmQ1ZjQ0MTlmYzY0NSJ9fX0="
    lore:
      - "&7点击查看上一页"
  
  # 下一页按钮
  next_page:
    id: PLAYER_HEAD
    name: "&7下一页"
    skull_texture: "eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvNjgyYWQxYjljYjRkZDIxMjU5YzBkNzVhYTMxNWZmMzg5YzNjZWY3NTJiZTM5NDkzMzgxNjRiYWM4NGE5NmUifX19"
    lore:
      - "&7点击查看下一页"
  
  # 页面信息
  page_info:
    id: PAPER
    name: "&7页面信息"
    lore:
      - "&7当前页：&e%current_page%"
      - "&7总页数：&e%total_pages%"
      - "&7总条目：&e%total_items%"

# 颜色主题
color_themes:
  default:
    primary: "&6"      # 主要颜色（金色）
    secondary: "&7"    # 次要颜色（灰色）
    accent: "&e"       # 强调颜色（黄色）
    success: "&a"      # 成功颜色（绿色）
    warning: "&c"      # 警告颜色（红色）
    info: "&b"         # 信息颜色（青色）
```

### 自定义主题

您可以创建自定义GUI主题：

```yaml
# 自定义主题：紫色魔法风格
color_themes:
  magic_purple:
    primary: "&d"      # 紫色
    secondary: "&8"    # 深灰
    accent: "&5"       # 深紫
    success: "&a"      # 绿色
    warning: "&c"      # 红色
    info: "&b"         # 青色
    
    # 特殊效果
    gradient_start: "&d"
    gradient_end: "&5"
    
    # 背景配置
    background_item: "PURPLE_STAINED_GLASS_PANE"
    border_item: "MAGENTA_STAINED_GLASS_PANE"

# 应用主题
gui_theme: "magic_purple"
```

## 权限配置

### 基础权限节点

```yaml
# 权限配置
permissions:
  # 默认权限
  default_permissions:
    - "codex.command.main"
    - "codex.gui.open"
    - "codex.stats.self"
  
  # VIP权限
  vip_permissions:
    - "codex.bypass.cooldown"
    - "codex.stats.others"
    - "codex.gui.themes"
  
  # 管理员权限
  admin_permissions:
    - "codex.*"
    - "codex.admin"
    - "codex.command.*"
    - "codex.database.*"

# 权限组配置
permission_groups:
  # 新手组
  newcomer:
    discovery_limit: 50      # 最多解锁50个发现
    cooldown_multiplier: 2.0 # 冷却时间翻倍
    
  # 普通玩家组  
  player:
    discovery_limit: -1      # 无限制
    cooldown_multiplier: 1.0 # 正常冷却时间
    
  # VIP组
  vip:
    discovery_limit: -1      # 无限制
    cooldown_multiplier: 0.5 # 冷却时间减半
    bonus_rewards: true      # 额外奖励
    
  # 管理员组
  admin:
    discovery_limit: -1      # 无限制
    cooldown_multiplier: 0.0 # 无冷却时间
    bypass_all: true         # 绕过所有限制
```

## 奖励系统配置

### 全局奖励设置

```yaml
# 全局奖励配置
rewards:
  # 奖励系统开关
  enabled: true
  
  # 奖励执行模式
  execution_mode: "async"  # sync, async
  
  # 奖励延迟（毫秒）
  delay: 100
  
  # 失败重试
  retry:
    enabled: true
    max_attempts: 3
    delay: 1000  # 重试间隔
  
  # 奖励条件
  conditions:
    # 世界限制
    allowed_worlds:
      - "world"
      - "world_nether"
      - "world_the_end"
    
    # 游戏模式限制
    allowed_gamemodes:
      - "SURVIVAL"
      - "ADVENTURE"
    
    # 最小在线时间（分钟）
    min_online_time: 10

# 默认奖励模板
default_rewards:
  # 新手奖励
  newcomer:
    - "playsound: ENTITY_PLAYER_LEVELUP;0.5;1"
    - "console_command: experience add %player% 10 points"
    - "message: &e恭喜解锁新发现！"
  
  # 普通玩家奖励
  player:
    - "playsound: ENTITY_PLAYER_LEVELUP;1;1"
    - "console_command: experience add %player% 50 points"
    - "title: 10;40;10;&e发现解锁!;&7%name%"
  
  # VIP奖励
  vip:
    - "playsound: ENTITY_PLAYER_LEVELUP;1;1.2"
    - "console_command: experience add %player% 100 points"
    - "console_command: give %player% diamond 1"
    - "title: 10;40;10;&6VIP发现!;&e%name%"
    - "broadcast: &e%player% &7解锁了VIP专属发现：&e%name%"

# 里程碑奖励
milestone_rewards:
  # 发现数量里程碑
  discovery_milestones:
    10:
      - "title: 20;60;20;&e探索者!;&7已解锁10个发现"
      - "console_command: give %player% emerald 5"
    
    50:
      - "title: 20;60;20;&6冒险家!;&7已解锁50个发现"
      - "console_command: give %player% diamond 10"
    
    100:
      - "title: 20;60;20;&c传奇探险家!;&7已解锁100个发现"
      - "console_command: give %player% nether_star 1"
      - "broadcast: &c%player% &e成为了传奇探险家！"
  
  # 分类完成里程碑
  category_completion:
    first_category:
      - "title: 20;60;20;&a专家!;&7完成了第一个分类"
      - "console_command: experience add %player% 500 points"
    
    all_categories:
      - "title: 20;60;20;&6图鉴大师!;&7完成了所有分类"
      - "console_command: give %player% enchanted_book 5"
      - "broadcast: &6%player% &e成为了图鉴大师！"
```

## 数据库配置详解

### H2 数据库配置

```yaml
# H2数据库详细配置
h2_database:
  # 数据库文件路径
  file_path: "plugins/Codex/data/codex.h2.db"
  
  # 连接池配置
  connection_pool:
    # 最大连接数
    maximum_pool_size: 10
    
    # 最小空闲连接数
    minimum_idle: 2
    
    # 连接超时（毫秒）
    connection_timeout: 30000
    
    # 空闲超时（毫秒）
    idle_timeout: 600000
    
    # 连接最大生存时间（毫秒）
    max_lifetime: 1800000
    
    # 连接保活时间（毫秒）
    keepalive_time: 30000
  
  # H2特定设置
  h2_settings:
    # 缓存大小（KB）
    cache_size: 16384
    
    # WAL模式（提高并发性能）
    wal_mode: true
    
    # 自动压缩
    auto_compact: true
    
    # 同步模式
    synchronous: "NORMAL"  # OFF, NORMAL, FULL
```

### MySQL 数据库配置

```yaml
# MySQL数据库详细配置
mysql_database:
  # 基础连接信息
  host: "localhost"
  port: 3306
  database: "codex"
  username: "codex_user"
  password: "secure_password"
  
  # 连接参数
  connection_properties:
    useSSL: false
    allowPublicKeyRetrieval: true
    useUnicode: true
    characterEncoding: "utf8mb4"
    autoReconnect: true
    failOverReadOnly: false
    maxReconnects: 3
    initialTimeout: 2
    
  # 连接池配置
  connection_pool:
    maximum_pool_size: 20
    minimum_idle: 5
    connection_timeout: 30000
    idle_timeout: 600000
    max_lifetime: 1800000
    leak_detection_threshold: 60000
  
  # 表配置
  table_settings:
    # 表前缀
    prefix: "codex_"
    
    # 字符集
    charset: "utf8mb4"
    collation: "utf8mb4_unicode_ci"
    
    # 存储引擎
    engine: "InnoDB"
```

## 调试与日志配置

### 调试设置

```yaml
# 调试配置
debug:
  # 全局调试开关
  enabled: false
  
  # 调试级别
  level: "INFO"  # TRACE, DEBUG, INFO, WARN, ERROR
  
  # 调试类别
  categories:
    - "discovery"      # 发现系统调试
    - "database"       # 数据库调试
    - "gui"            # GUI调试
    - "rewards"        # 奖励系统调试
    - "integrations"   # 第三方插件集成调试
  
  # 详细日志
  verbose: false
  
  # 日志文件
  log_file:
    enabled: true
    path: "plugins/Codex/logs/debug.log"
    max_size: "10MB"
    max_files: 5

# 性能监控
performance_monitoring:
  # 启用性能监控
  enabled: true
  
  # 监控间隔（秒）
  interval: 60
  
  # 监控项目
  metrics:
    - "memory_usage"
    - "database_queries"
    - "discovery_checks"
    - "gui_operations"
  
  # 性能告警
  alerts:
    # 内存使用告警阈值（MB）
    memory_threshold: 512
    
    # 慢查询告警阈值（毫秒）
    slow_query_threshold: 1000
    
    # GUI操作延迟告警（毫秒）
    gui_delay_threshold: 500
```

## 配置验证与测试

### 配置验证

Codex 提供配置验证功能，帮助您检查配置文件的正确性：

```bash
# 验证所有配置文件
/codex config validate

# 验证特定配置文件
/codex config validate config.yml
/codex config validate inventory.yml

# 显示配置信息
/codex config info

# 重载配置
/codex reload
```

### 配置模板

您可以生成配置模板：

```bash
# 生成默认配置模板
/codex config template default

# 生成特定功能的配置模板
/codex config template enchantments
/codex config template fishing
/codex config template crops
```

## 下一步

了解了基础配置后，您可以继续学习：

1. **[发现配置](discovery-config.md)** - 深入了解发现系统配置
2. **[界面配置](gui-config.md)** - 自定义GUI外观和行为
3. **[奖励配置](rewards-config.md)** - 配置复杂的奖励系统
4. **[数据库配置](database-config.md)** - 数据库高级配置和优化
