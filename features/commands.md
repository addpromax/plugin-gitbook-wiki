# 命令与权限

本章详细介绍 Codex 插件的所有可用命令、权限节点和使用方法。

## 基础命令

### 主命令

#### `/codex`
打开 Codex 主界面GUI。

**语法**：`/codex`
**权限**：`codex.command.main`
**别名**：无

**示例**：
```
/codex
```

### 管理员命令

#### `/codex reload`
重新加载插件的所有配置文件。

**语法**：`/codex reload`
**权限**：`codex.admin`

**示例**：
```
/codex reload
```

**说明**：
- 重载所有 YAML 配置文件
- 刷新内存中的数据缓存
- 重新检查插件挂钩状态
- 不会重载玩家数据

#### `/codex debug [on|off]`
切换调试模式或查看调试信息。

**语法**：`/codex debug [on|off]`
**权限**：`codex.admin`

**示例**：
```
/codex debug on          # 开启调试模式
/codex debug off         # 关闭调试模式
/codex debug             # 显示当前调试状态和系统信息
```

**调试信息包含**：
- 📊 内存使用情况
- 📈 数据库连接状态
- 🔗 第三方插件挂钩状态
- ⏱️ 平均响应时间
- 📁 配置文件状态

#### `/codex help`
显示命令帮助信息。

**语法**：`/codex help [命令]`
**权限**：`codex.command.main`

**示例**：
```
/codex help              # 显示所有命令
/codex help reload       # 显示reload命令帮助
```

## 数据库管理命令

#### `/codex database info`
显示数据库连接信息和统计数据。

**语法**：`/codex database info`
**权限**：`codex.admin`

**显示信息**：
- 数据库类型（YAML/H2/MySQL）
- 连接状态
- 数据表统计
- 性能指标

#### `/codex database migrate <from> <to>`
数据库迁移命令。

**语法**：`/codex database migrate <from> <to>`
**权限**：`codex.admin`

**支持的迁移类型**：
- `yaml` → `h2`
- `yaml` → `mysql`
- `h2` → `mysql`
- `mysql` → `h2`

**示例**：
```
/codex database migrate yaml h2      # 从YAML文件迁移到H2数据库
/codex database migrate h2 mysql     # 从H2迁移到MySQL
```

**注意事项**：
- ⚠️ 迁移前请备份数据
- 🕐 迁移过程可能需要时间
- 🔒 迁移期间建议暂停服务器

#### `/codex database backup [文件名]`
创建数据库备份。

**语法**：`/codex database backup [文件名]`
**权限**：`codex.admin`

**示例**：
```
/codex database backup               # 自动生成文件名
/codex database backup daily_backup  # 指定文件名
```

## 玩家管理命令

#### `/codex player <玩家名> [操作]`
查看或管理玩家数据。

**语法**：`/codex player <玩家名> [info|reset|force]`
**权限**：`codex.admin`

**操作类型**：

**查看信息** (`info`)：
```
/codex player Steve info
```
显示玩家的发现统计：
- 总发现数量
- 各分类完成度
- 最近发现记录
- 账户创建时间

**重置数据** (`reset`)：
```
/codex player Steve reset           # 重置所有数据
/codex player Steve reset items     # 只重置物品分类
```

**强制发现** (`force`)：
```
/codex player Steve force first_diamond    # 强制解锁指定发现
```

#### `/codex stats [玩家名]`
显示发现统计信息。

**语法**：`/codex stats [玩家名]`
**权限**：
- `codex.stats.self` - 查看自己的统计
- `codex.stats.others` - 查看他人的统计

**示例**：
```
/codex stats              # 查看自己的统计
/codex stats Steve        # 查看Steve的统计
```

## 测试与调试命令

#### `/codex test <类型> [参数]`
测试功能命令（仅调试模式可用）。

**语法**：`/codex test <discovery|trigger|reward> [参数]`
**权限**：`codex.debug`

**测试类型**：

**发现测试**：
```
/codex test discovery first_diamond    # 测试钻石发现
```

**触发器测试**：
```
/codex test trigger ITEM_OBTAIN DIAMOND    # 测试物品获取触发器
```

**奖励测试**：
```
/codex test reward items per_discovery     # 测试物品分类的单个发现奖励
```

#### `/codex force <玩家> <发现ID>`
强制触发指定玩家的发现。

**语法**：`/codex force <玩家> <发现ID>`
**权限**：`codex.admin`

**示例**：
```
/codex force Steve first_diamond       # 强制Steve解锁钻石发现
/codex force Alex zombie_killer        # 强制Alex解锁僵尸击杀发现
```

## 权限系统

### 权限节点列表

#### 基础权限

| 权限节点 | 描述 | 默认值 |
|----------|------|--------|
| `codex.command.main` | 使用主命令 | `true` |
| `codex.gui.open` | 打开GUI界面 | `true` |
| `codex.stats.self` | 查看自己统计 | `true` |

#### 管理员权限

| 权限节点 | 描述 | 默认值 |
|----------|------|--------|
| `codex.admin` | 所有管理权限 | `op` |
| `codex.command.reload` | 重载配置 | `op` |
| `codex.command.debug` | 调试功能 | `op` |
| `codex.database.*` | 数据库管理 | `op` |
| `codex.player.*` | 玩家管理 | `op` |
| `codex.stats.others` | 查看他人统计 | `op` |

#### 特殊权限

| 权限节点 | 描述 | 默认值 |
|----------|------|--------|
| `codex.bypass.cooldown` | 跳过发现冷却 | `false` |
| `codex.bypass.world` | 跳过世界限制 | `false` |
| `codex.notify.admin` | 接收管理通知 | `op` |

### 权限组配置示例

#### 使用 LuckPerms
```bash
# 普通玩家权限
lp group default permission set codex.command.main true
lp group default permission set codex.gui.open true
lp group default permission set codex.stats.self true

# VIP玩家权限
lp group vip permission set codex.bypass.cooldown true
lp group vip permission set codex.stats.others true

# 管理员权限
lp group admin permission set codex.admin true
```

#### 使用 PermissionsEx (PEX)
```yaml
groups:
  default:
    permissions:
      - codex.command.main
      - codex.gui.open
      - codex.stats.self
  
  vip:
    inheritance:
      - default
    permissions:
      - codex.bypass.cooldown
      - codex.stats.others
  
  admin:
    permissions:
      - codex.admin
```

#### 使用 GroupManager
```yaml
groups:
  Default:
    permissions:
      - codex.command.main
      - codex.gui.open
      - codex.stats.self
  
  VIP:
    inherits:
      - Default
    permissions:
      - codex.bypass.cooldown
  
  Admin:
    permissions:
      - codex.*
```

## 命令别名配置

您可以在 `commands.yml` 中添加自定义别名：

```yaml
command-block-overrides: []
ignore-vanilla-permissions: false
aliases:
  # Codex 命令别名
  图鉴:
    - "codex"
  codebook:
    - "codex"
  discoveries:
    - "codex"
  
  # 管理员命令别名
  codex-reload:
    - "codex reload"
  codex-debug:
    - "codex debug"
```

## 控制台命令

所有命令都支持从控制台执行，但有以下注意事项：

### 控制台专用语法

#### 查看在线玩家统计
```bash
codex stats *              # 显示所有在线玩家统计
```

#### 批量操作
```bash
codex force * first_diamond    # 给所有在线玩家解锁钻石发现
```

#### 服务器状态检查
```bash
codex debug                # 显示详细系统信息
codex database info        # 显示数据库状态
```

## 自动化与脚本

### 定时任务示例

使用 Cron 或其他调度工具：

```bash
#!/bin/bash
# 每日备份脚本
screen -S minecraft -p 0 -X stuff "codex database backup daily_$(date +%Y%m%d)$(printf \\r)"

# 每周统计报告
screen -S minecraft -p 0 -X stuff "codex stats *$(printf \\r)"
```

### 启动脚本集成

```bash
#!/bin/bash
# 服务器启动后的初始化
sleep 30  # 等待服务器完全启动

# 检查插件状态
screen -S minecraft -p 0 -X stuff "codex debug$(printf \\r)"

# 检查数据库连接
screen -S minecraft -p 0 -X stuff "codex database info$(printf \\r)"
```

## 故障排除

### 常见命令问题

#### 权限不足
**现象**：执行命令时提示"你没有权限"
**解决**：检查权限配置，确保用户具有相应权限节点

#### 命令不响应
**现象**：输入命令后没有任何反应
**解决**：
1. 检查插件是否正确加载
2. 查看控制台错误信息
3. 尝试重载插件配置

#### 数据库命令失败
**现象**：数据库相关命令执行失败
**解决**：
1. 检查数据库连接状态
2. 确认数据库配置正确
3. 查看数据库日志

### 调试建议

1. **启用调试模式**：
   ```
   /codex debug on
   ```

2. **查看详细信息**：
   ```
   /codex debug
   /codex database info
   ```

3. **测试基础功能**：
   ```
   /codex test discovery first_diamond
   ```

## 下一步

了解了命令系统后，您可以继续学习：

1. **[数据存储系统](data-storage.md)** - 配置数据库和存储选项
2. **[自动图鉴系统](enchantment-codex.md)** - 配置自动图鉴功能
3. **[配置文件详解](../configuration/basic-config.md)** - 深入了解配置选项
4. **[故障排除](../troubleshooting/faq.md)** - 解决常见问题
