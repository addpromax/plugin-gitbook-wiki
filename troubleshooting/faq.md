# 常见问题解答

本章收集了 Codex 插件使用过程中的常见问题和解决方案。如果您遇到的问题不在此列表中，请查看其他故障排除章节或联系技术支持。

## 安装与配置问题

### Q1: 插件无法正常启动

**问题描述**：插件在服务器启动时出现错误或无法加载

**可能原因**：
- Java 版本不兼容
- 服务器版本不支持
- 依赖插件缺失
- 配置文件格式错误

**解决方案**：

1. **检查版本兼容性**：
   ```bash
   # 查看Java版本
   java -version
   
   # 查看服务器版本
   /version
   ```
   确保：
   - Java 8 或更高版本
   - Minecraft 1.13 或更高版本
   - Paper/Spigot 服务器核心

2. **检查控制台错误信息**：
   ```
   [ERROR] [Codex] Failed to load plugin
   [ERROR] Caused by: java.lang.ClassNotFoundException
   ```
   根据错误信息进行相应处理

3. **验证依赖插件**：
   ```bash
   /plugins
   ```
   确保 PlaceholderAPI（可选）等依赖插件已正确安装

4. **重置配置文件**：
   ```bash
   # 备份现有配置
   mv plugins/Codex plugins/Codex_backup
   
   # 重启服务器让插件重新生成默认配置
   ```

### Q2: 配置文件重载后没有变化

**问题描述**：使用 `/codex reload` 后配置修改没有生效

**可能原因**：
- YAML 语法错误
- 缓存未清理
- 某些配置需要重启生效

**解决方案**：

1. **检查YAML语法**：
   ```yaml
   # 错误示例：缩进不正确
   discoveries:
   first_diamond:  # 缺少空格缩进
     name: "钻石发现"
   
   # 正确示例：
   discoveries:
     first_diamond:  # 正确的2空格缩进
       name: "钻石发现"
   ```

2. **查看重载结果**：
   ```bash
   /codex reload
   ```
   注意控制台输出的成功/错误信息

3. **清理缓存**：
   ```bash
   /codex debug
   /codex cache clear
   ```

4. **完全重启**：
   某些配置（如数据库设置）需要完全重启服务器

### Q3: 权限设置无效

**问题描述**：设置了权限但玩家仍然无法使用功能

**解决方案**：

1. **检查权限插件配置**：
   ```bash
   # LuckPerms 示例
   /lp user <玩家> permission set codex.command.main true
   
   # 检查权限
   /lp user <玩家> permission check codex.command.main
   ```

2. **验证权限节点**：
   ```yaml
   # 确保权限节点拼写正确
   permissions:
     - "codex.command.main"     # 正确
     - "codex.commands.main"    # 错误
   ```

3. **检查权限继承**：
   ```bash
   # 检查组权限
   /lp group default permission set codex.command.main true
   ```

## 功能使用问题

### Q4: 发现没有被触发

**问题描述**：满足条件但发现没有解锁

**可能原因**：
- 触发条件配置错误
- 物品识别失败
- 冷却时间未结束
- 权限或世界限制

**解决方案**：

1. **启用调试模式**：
   ```yaml
   # config.yml
   settings:
     debug: true
   ```

2. **检查触发条件**：
   ```yaml
   discoveries:
     first_diamond:
       discovered_on:
         type: ITEM_OBTAIN
         value:
           item_type: DIAMOND  # 确保物品类型正确
   ```

3. **验证物品信息**：
   ```bash
   # 手持物品使用命令查看信息
   /codex debug item
   ```

4. **检查冷却时间**：
   ```bash
   /codex debug cooldown <玩家>
   ```

5. **检查世界限制**：
   ```yaml
   # 确保当前世界允许触发
   rewards:
     conditions:
       allowed_worlds:
         - "world"
   ```

### Q5: GUI界面显示异常

**问题描述**：图鉴界面无法打开或显示错乱

**解决方案**：

1. **检查GUI配置**：
   ```yaml
   # inventory.yml
   main_gui:
     size: 54  # 必须是9的倍数
     title: "&8[&6Codex&8] &7图鉴系统"
   ```

2. **验证物品ID**：
   ```yaml
   category:
     id: DIAMOND  # 确保是有效的物品ID
     # 无效示例：INVALID_ITEM
   ```

3. **检查颜色代码**：
   ```yaml
   name: "&6金色文本"      # 正确
   name: "&z无效颜色"      # 错误
   name: "<color:#FFD700>HEX颜色"  # MiniMessage格式
   ```

4. **重置GUI配置**：
   ```bash
   # 备份并重置
   mv plugins/Codex/inventory.yml plugins/Codex/inventory.yml.bak
   /codex reload
   ```

### Q6: 第三方插件集成失败

**问题描述**：ItemsAdder、MMOItems等插件的物品无法被识别

**解决方案**：

1. **检查插件挂钩状态**：
   ```
   # 查看控制台启动信息
   [Codex] ItemsAdder Hook: 成功
   [Codex] MMOItems Hook: 失败
   ```

2. **验证插件版本兼容性**：
   | 插件 | 最低版本 | 推荐版本 |
   |------|----------|----------|
   | ItemsAdder | 3.5.0 | 3.6.1+ |
   | MMOItems | 6.8.0 | 6.9.4+ |

3. **检查物品ID格式**：
   ```yaml
   # ItemsAdder
   itemsadder_item_id: "myitems:magic_sword"  # 正确
   itemsadder_item_id: "magic_sword"          # 错误，缺少命名空间
   
   # MMOItems
   mmoitems_type: "SWORD"
   mmoitems_id: "LEGENDARY_BLADE"
   ```

4. **手动启用挂钩**：
   ```yaml
   # config.yml
   force_plugin_hooks:
     itemsadder: true
     mmoitems: true
   ```

## 数据库问题

### Q7: 数据库连接失败

**问题描述**：H2或MySQL数据库无法连接

**解决方案**：

1. **H2数据库问题**：
   ```bash
   # 检查文件权限
   ls -la plugins/Codex/data/
   
   # 确保Codex有写入权限
   chmod 755 plugins/Codex/
   chmod 644 plugins/Codex/data/*
   ```

2. **MySQL连接问题**：
   ```yaml
   # 检查连接配置
   mysql_database:
     host: "localhost"      # 确保主机正确
     port: 3306            # 确保端口正确
     database: "codex"     # 确保数据库存在
     username: "codex_user" # 确保用户存在
     password: "password"   # 确保密码正确
   ```

3. **测试数据库连接**：
   ```bash
   # MySQL连接测试
   mysql -h localhost -P 3306 -u codex_user -p codex
   ```

4. **查看详细错误**：
   ```bash
   /codex database info
   /codex debug database
   ```

### Q8: 数据丢失或不同步

**问题描述**：玩家发现数据丢失或在不同服务器间不同步

**解决方案**：

1. **检查自动保存设置**：
   ```yaml
   auto_save:
     enabled: true
     interval: 900  # 15分钟
     on_shutdown: true
     on_player_quit: true
   ```

2. **手动保存数据**：
   ```bash
   /codex save all
   /codex save <玩家>
   ```

3. **数据库备份恢复**：
   ```bash
   # 创建备份
   /codex database backup emergency
   
   # 恢复备份
   /codex database restore <备份文件>
   ```

4. **跨服同步检查**：
   - 确保所有服务器使用相同的MySQL数据库
   - 检查网络连接和权限
   - 验证数据库表结构一致

## 性能问题

### Q9: 服务器性能下降

**问题描述**：安装Codex后服务器出现卡顿或性能问题

**解决方案**：

1. **检查性能统计**：
   ```bash
   /codex debug performance
   /timings report  # Paper服务器
   ```

2. **优化配置**：
   ```yaml
   # 减少检测频率
   discovery:
     global_cooldown: 2000    # 增加到2秒
     player_cooldown: 1000    # 增加到1秒
   
   # 启用异步处理
   performance:
     async_operations: true
     thread_pool_size: 2      # 减少线程数
   ```

3. **禁用不需要的功能**：
   ```yaml
   # 禁用自动图鉴
   enchantments:
     settings:
       enabled: false
   
   fishing:
     settings:
       enabled: false
   ```

4. **优化数据库**：
   ```yaml
   # 使用H2而非文件存储
   storage:
     type: "h2"
   
   # 调整连接池
   h2_database:
     pool:
       maximum_pool_size: 5  # 减少连接数
   ```

### Q10: 内存使用过高

**解决方案**：

1. **启用内存清理**：
   ```yaml
   memory_management:
     cleanup_interval: 900    # 15分钟清理一次
     clean_offline_players: true
     max_cache_size: 5000     # 减少缓存大小
   ```

2. **调整缓存设置**：
   ```yaml
   discovery:
     cache:
       max_size: 5000         # 减少缓存条目
       expire_time: 1800      # 30分钟过期
   ```

3. **监控内存使用**：
   ```bash
   /codex debug memory
   ```

## 错误代码参考

### 常见错误代码

| 错误代码 | 含义 | 解决方法 |
|----------|------|----------|
| `COD-001` | 配置文件语法错误 | 检查YAML格式 |
| `COD-002` | 数据库连接失败 | 检查数据库配置 |
| `COD-003` | 权限不足 | 检查权限设置 |
| `COD-004` | 插件挂钩失败 | 检查依赖插件 |
| `COD-005` | 发现触发失败 | 检查触发条件 |
| `COD-006` | GUI加载失败 | 检查界面配置 |
| `COD-007` | 奖励执行失败 | 检查奖励配置 |
| `COD-008` | 缓存溢出 | 调整缓存设置 |

## 获取帮助

### 诊断信息收集

在寻求技术支持时，请提供以下信息：

1. **服务器信息**：
   ```bash
   /version
   /plugins
   java -version
   ```

2. **Codex诊断信息**：
   ```bash
   /codex debug info
   /codex config validate
   ```

3. **错误日志**：
   - 控制台启动日志
   - `plugins/Codex/logs/` 下的日志文件
   - 完整的错误堆栈跟踪

4. **配置文件**：
   - `config.yml`
   - `inventory.yml`
   - 相关的分类配置文件

### 支持渠道

- **GitHub Issues**：[项目仓库](https://github.com/addpromax/plugin-gitbook-wiki/issues)
- **Spigot论坛**：[插件页面](https://www.spigotmc.org/resources/codex-rpg-discoveries.90371/)
- **QQ群**：Codex用户交流群
- **邮件支持**：技术问题邮件咨询

### 提问技巧

为了快速获得帮助，请：

1. **详细描述问题**：
   - 具体的问题现象
   - 期望的结果
   - 已尝试的解决方法

2. **提供完整信息**：
   - 服务器环境信息
   - 相关配置文件
   - 完整的错误日志

3. **使用代码块**：
   ```
   在论坛或GitHub中使用代码块格式粘贴日志和配置
   ```

4. **一次一个问题**：
   避免在一个帖子中询问多个不相关的问题

## 下一步

如果常见问题解答无法解决您的问题，请查看：

1. **[安装问题](installation-issues.md)** - 详细的安装故障排除
2. **[配置问题](configuration-issues.md)** - 配置相关问题解决
3. **[性能问题](performance-issues.md)** - 性能优化和问题排查
4. **[管理员工具](../admin/debugging.md)** - 高级调试和诊断工具
