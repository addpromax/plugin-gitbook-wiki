# 其他插件支持

除了物品和附魔插件外，Codex 还支持与多种功能性插件的集成，包括区域保护、怪物生成、权限管理等。这些集成大大扩展了发现系统的触发方式和应用场景。

## 区域保护插件

### WorldGuard 集成

WorldGuard 是最流行的区域保护插件，Codex 提供完整的区域触发支持。

#### 启用检测

```
[Codex] WorldGuard Hook: 成功
[Codex] - 检测到 WorldGuard 版本: 7.0.9
[Codex] - 已加载区域数量: 156 个
[Codex] - 支持的世界: 3 个
```

#### 基础区域触发

```yaml
discoveries:
  # 进入特定区域时触发
  secret_chamber:
    name: "&d神秘密室"
    description:
      - "&7你发现了隐藏在城堡中的密室！"
      - "&7里面散发着古老的魔力气息..."
    discovered_on:
      type: WORLDGUARD_REGION
      value:
        region_name: "secret_room"
        world_name: "world"  # 可选，不指定则任意世界
  
  # 进入城镇区域
  town_discovery:
    name: "&a新手村"
    description:
      - "&7欢迎来到新手村！"
      - "&7这里是所有冒险的开始。"
    discovered_on:
      type: WORLDGUARD_REGION
      value:
        region_name: "spawn_town"
```

#### 高级区域配置

```yaml
discoveries:
  # 多区域触发
  dungeon_explorer:
    name: "&c地牢探险家"
    description:
      - "&7你探索了多个危险的地牢！"
    discovered_on:
      type: WORLDGUARD_REGION
      value:
        # 进入任意一个地牢区域即可触发
        region_name: "dungeon_1;dungeon_2;dungeon_3;boss_room"
        world_name: "dungeon_world"
  
  # 区域层级触发
  castle_lord:
    name: "&6城堡领主"
    description:
      - "&7你征服了整座城堡！"
    discovered_on:
      type: WORLDGUARD_REGION
      value:
        # 需要进入城堡的核心区域
        region_name: "castle_throne_room"
        # 可以添加额外条件
        required_discoveries: ["castle_entrance", "castle_courtyard"]
```

#### 权限区域

```yaml
discoveries:
  # 需要特定权限才能进入的区域
  admin_vault:
    name: "&4管理员密库"
    description:
      - "&7只有管理员才能进入的神秘区域！"
    discovered_on:
      type: WORLDGUARD_REGION
      value:
        region_name: "admin_only_vault"
        required_permission: "worldguard.region.admin"
```

### Residence 集成

Residence 是另一个流行的区域保护插件。

#### 启用检测

```
[Codex] Residence Hook: 成功
[Codex] - 检测到 Residence 版本: 6.0.1.1
[Codex] - 已加载领地数量: 89 个
```

#### Residence 区域触发

```yaml
discoveries:
  # 进入玩家领地
  homeowner:
    name: "&e房主"
    description:
      - "&7你拥有了自己的领地！"
    discovered_on:
      type: RESIDENCE_REGION
      value:
        # 进入自己的领地时触发
        owner_only: true
  
  # 访问其他玩家领地
  friendly_visitor:
    name: "&b友善访客"
    description:
      - "&7你被邀请进入了其他玩家的领地！"
    discovered_on:
      type: RESIDENCE_REGION
      value:
        owner_only: false
        trusted_only: true  # 只有被信任的玩家
```

## 怪物生成插件

### MythicMobs 集成

MythicMobs 允许创建自定义怪物，Codex 支持基于 MythicMobs 怪物的发现触发。

#### 启用检测

```
[Codex] MythicMobs Hook: 成功
[Codex] - 检测到 MythicMobs 版本: 5.7.2
[Codex] - 已加载自定义怪物: 67 个
[Codex] - 已加载技能: 156 个
```

#### MythicMobs 怪物击杀

```yaml
discoveries:
  # 击杀自定义Boss
  dragon_slayer:
    name: "&4屠龙勇士"
    description:
      - "&7你击败了传说中的古龙！"
      - "&7你的名字将被载入史册！"
    discovered_on:
      type: MOB_KILL
      value:
        # 使用 MythicMobs 怪物类型
        mythic_mob_type: "AncientDragon"
  
  # 击杀特定等级的怪物
  elite_hunter:
    name: "&6精英猎手"
    description:
      - "&7你专门猎杀高等级的精英怪物！"
    discovered_on:
      type: MOB_KILL
      value:
        mythic_mob_type: "EliteGuard"
        min_level: 50  # 需要50级以上的精英守卫
```

#### 怪物技能触发

```yaml
discoveries:
  # 被特定技能击中时触发
  spell_survivor:
    name: "&5法术幸存者"
    description:
      - "&7你在强大的魔法攻击下幸存了下来！"
    discovered_on:
      type: MYTHIC_SKILL_HIT
      value:
        skill_name: "DragonFireball"
        mob_type: "FireDragon"
```

### EliteMobs 集成

EliteMobs 专注于创建精英怪物和地牢系统。

#### 启用检测

```
[Codex] EliteMobs Hook: 成功
[Codex] - 检测到 EliteMobs 版本: 9.2.3
[Codx] - 已加载精英怪物: 45 个
[Codex] - 已加载地牢: 12 个
```

#### EliteMobs 击杀发现

```yaml
discoveries:
  # 击杀精英怪物
  elite_slayer:
    name: "&c精英杀手"
    description:
      - "&7你击败了危险的精英怪物！"
    discovered_on:
      type: MOB_KILL
      value:
        # EliteMobs 精英怪物
        elitemobs_type: "elite"
        min_level: 10
  
  # 地牢征服者
  dungeon_conqueror:
    name: "&6地牢征服者"
    description:
      - "&7你征服了EliteMobs地牢！"
    discovered_on:
      type: ELITEMOBS_DUNGEON_COMPLETE
      value:
        dungeon_name: "AncientCaves"
```

## 经济插件

### Vault 集成

Vault 是经济系统的统一API，支持多种经济插件。

#### 经济触发器

```yaml
discoveries:
  # 达到特定财富水平
  millionaire:
    name: "&6百万富翁"
    description:
      - "&7你的财富已经超过了一百万！"
    discovered_on:
      type: ECONOMY_MILESTONE
      value:
        min_balance: 1000000
        currency: "default"  # 默认货币
  
  # 单次大额交易
  big_spender:
    name: "&e大手笔消费"
    description:
      - "&7你进行了一次大额消费！"
    discovered_on:
      type: ECONOMY_TRANSACTION
      value:
        transaction_type: "spend"
        min_amount: 50000
```

## 职业插件

### Jobs 集成

Jobs 是流行的职业系统插件。

#### 职业等级触发

```yaml
discoveries:
  # 达到职业等级
  master_miner:
    name: "&7挖矿大师"
    description:
      - "&7你已经成为了挖矿领域的大师！"
    discovered_on:
      type: JOBS_LEVEL
      value:
        job_name: "Miner"
        min_level: 50
  
  # 多职业精通
  jack_of_all_trades:
    name: "&e全能工匠"
    description:
      - "&7你在多个职业领域都有所成就！"
    discovered_on:
      type: JOBS_LEVEL
      value:
        # 需要多个职业都达到一定等级
        job_requirements:
          "Miner": 30
          "Builder": 30
          "Farmer": 30
```

### mcMMO 集成

mcMMO 提供技能等级系统。

#### 技能等级触发

```yaml
discoveries:
  # 技能等级里程碑
  swordsman_master:
    name: "&c剑术宗师"
    description:
      - "&7你的剑术技能已达到宗师级别！"
    discovered_on:
      type: MCMMO_SKILL
      value:
        skill_name: "Swords"
        min_level: 1000
  
  # 技能组合成就
  combat_expert:
    name: "&4战斗专家"
    description:
      - "&7你在所有战斗技能上都有极高造诣！"
    discovered_on:
      type: MCMMO_SKILL
      value:
        skill_requirements:
          "Swords": 500
          "Axes": 500
          "Archery": 500
          "Unarmed": 500
```

## 社交插件

### Marriage 集成

支持结婚系统的发现触发。

```yaml
discoveries:
  # 结婚发现
  newlywed:
    name: "&d新婚夫妇"
    description:
      - "&7恭喜你找到了生命中的另一半！"
    discovered_on:
      type: MARRIAGE_EVENT
      value:
        event_type: "marry"
  
  # 结婚纪念日
  anniversary:
    name: "&6金婚纪念"
    description:
      - "&7你们已经结婚50天了！"
    discovered_on:
      type: MARRIAGE_ANNIVERSARY
      value:
        days_married: 50
```

### Guilds 集成

支持公会系统的发现。

```yaml
discoveries:
  # 加入公会
  guild_member:
    name: "&b公会成员"
    description:
      - "&7你加入了一个公会！"
    discovered_on:
      type: GUILD_JOIN
      value:
        guild_type: "any"
  
  # 公会领袖
  guild_master:
    name: "&6公会会长"
    description:
      - "&7你创建或成为了公会的领导者！"
    discovered_on:
      type: GUILD_RANK
      value:
        min_rank: "master"
```

## 其他实用插件

### PlaceholderAPI 集成

PlaceholderAPI 为各种插件提供变量支持。

#### 变量条件触发

```yaml
discoveries:
  # 基于PlaceholderAPI变量的触发
  server_veteran:
    name: "&e服务器老兵"
    description:
      - "&7你已经在服务器上游玩了很长时间！"
    discovered_on:
      type: PLACEHOLDER_VALUE
      value:
        placeholder: "%statistic_play_one_minute%"
        operator: ">="
        value: 72000  # 1200小时（分钟）
  
  # 多变量条件
  elite_player:
    name: "&6精英玩家"
    description:
      - "&7你在各个方面都表现出色！"
    discovered_on:
      type: PLACEHOLDER_VALUE
      value:
        conditions:
          "%vault_eco_balance%": ">=100000"
          "%player_level%": ">=50"
          "%mcmmo_power_level%": ">=5000"
```

### LuckPerms 集成

权限管理系统集成。

```yaml
discoveries:
  # 权限组变更
  vip_member:
    name: "&eVIP会员"
    description:
      - "&7恭喜成为VIP会员！"
    discovered_on:
      type: PERMISSION_GROUP
      value:
        group_name: "vip"
        action: "add"  # add, remove, promote
  
  # 特殊权限获得
  admin_promotion:
    name: "&cManagement Team"
    description:
      - "&7你被提升为管理团队成员！"
    discovered_on:
      type: PERMISSION_NODE
      value:
        permission: "codex.admin"
        value: true
```

## 综合应用示例

### 大型RPG服务器集成

```yaml
# 综合多个插件的复杂发现系统
discoveries:
  # 传奇冒险家
  legendary_adventurer:
    name: "&6传奇冒险家"
    description:
      - "&7你已经成为了真正的传奇！"
      - "&7在各个领域都有卓越表现！"
    discovered_on:
      type: MULTIPLE_CONDITIONS
      value:
        conditions:
          # MythicMobs条件
          - type: MOB_KILL
            mythic_mob_type: "LegendaryBoss"
          
          # WorldGuard条件  
          - type: WORLDGUARD_REGION
            region_name: "legendary_shrine"
          
          # 经济条件
          - type: ECONOMY_MILESTONE
            min_balance: 5000000
          
          # Jobs条件
          - type: JOBS_LEVEL
            job_name: "Adventurer"
            min_level: 100
          
          # mcMMO条件
          - type: MCMMO_SKILL
            skill_name: "Combat"
            min_level: 1000
        
        # 所有条件都需要满足
        require_all: true
```

### 生存服务器集成

```yaml
# 生存服务器的集成应用
discoveries:
  # 建造大师
  master_builder:
    name: "&a建造大师"
    description:
      - "&7你展现了卓越的建造技艺！"
    discovered_on:
      type: MULTIPLE_CONDITIONS
      value:
        conditions:
          # Jobs建造师等级
          - type: JOBS_LEVEL
            job_name: "Builder"
            min_level: 75
          
          # WorldGuard领地大小
          - type: WORLDGUARD_REGION_SIZE
            min_blocks: 10000
          
          # ItemsAdder建造工具
          - type: ITEM_OBTAIN
            itemsadder_item_id: "tools:master_hammer"
        
        require_all: true
```

## 性能优化

### 插件集成性能配置

```yaml
# 第三方插件集成的性能优化
integration_performance:
  # 检测频率控制
  check_intervals:
    worldguard_regions: 1000    # 1秒检查一次区域
    mythicmobs_spawns: 500      # 0.5秒检查怪物
    economy_balance: 5000       # 5秒检查经济状态
    jobs_levels: 10000          # 10秒检查职业等级
  
  # 缓存配置
  cache_settings:
    region_cache_size: 5000
    mob_cache_size: 2000
    economy_cache_size: 1000
    cache_expire_time: 300000   # 5分钟
  
  # 异步处理
  async_processing:
    enabled: true
    thread_pool_size: 3
    max_queue_size: 1000
```

## 故障排除

### 常见问题

#### 问题：第三方插件挂钩失败
**解决方案**：
```yaml
# 手动启用插件挂钩
manual_hooks:
  worldguard: true
  mythicmobs: true
  elitemobs: true
  jobs: true
  
# 延迟加载（等待其他插件完全启动）
delayed_loading:
  enabled: true
  delay_seconds: 5
```

#### 问题：触发条件不生效
**解决方案**：
```yaml
# 调试特定插件集成
debug_integrations:
  - "worldguard"
  - "mythicmobs" 
  - "vault"
  
# 详细日志
verbose_integration_logging: true
```

## 下一步

了解了其他插件支持后，您可以继续学习：

1. **[配置指南](../configuration/basic-config.md)** - 深入了解配置系统
2. **[使用指南](../usage/player-interface.md)** - 玩家使用说明
3. **[故障排除](../troubleshooting/configuration-issues.md)** - 解决集成问题
4. **[性能优化](../admin/performance.md)** - 系统性能调优
