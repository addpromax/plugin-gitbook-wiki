# 数据存储系统

Codex 插件提供了灵活的数据存储选项，支持从简单的文件存储到高性能的数据库系统，满足不同规模服务器的需求。本章详细介绍各种存储方式的配置和优化方法。

## 存储方式概览

### 支持的存储类型

| 存储类型 | 适用场景 | 性能 | 跨服支持 | 配置难度 |
|----------|----------|------|----------|----------|
| **YAML文件** | 小型服务器 | 一般 | ❌ | 简单 |
| **H2数据库** | 中型服务器 | 优秀 | ❌ | 简单 |
| **MySQL数据库** | 大型服务器/网络 | 优秀 | ✅ | 中等 |

### 选择建议

- **玩家数 < 50**：使用 YAML 文件存储
- **玩家数 50-500**：推荐 H2 数据库
- **玩家数 > 500 或需要跨服**：使用 MySQL 数据库

## 基础配置

### 主配置文件设置

编辑 `plugins/Codex/config.yml`：

```yaml
# ==========================================
# 数据存储配置
# ==========================================

# 存储类型选择
# 可选值: file, h2, mysql
storage_type: h2

# 自动保存设置
auto_save:
  # 启用自动保存
  enabled: true
  
  # 保存间隔（分钟）
  interval: 15
  
  # 显示保存通知（仅管理员可见）
  show_notifications: false
  
  # 保存时机
  save_on:
    player_quit: true      # 玩家离开时保存
    server_shutdown: true  # 服务器关闭时保存
    discovery_unlock: true # 解锁发现时立即保存

# 内存管理
memory_management:
  # 内存清理间隔（秒）
  cleanup_interval: 1800  # 30分钟
  
  # 清理离线玩家数据
  clean_offline_players: true
  
  # 最大内存缓存条目数
  max_cache_size: 10000
  
  # 缓存过期时间（秒）
  cache_expire_time: 3600
```

## YAML 文件存储

### 配置说明

YAML 文件存储是最简单的存储方式，适合小型服务器：

```yaml
# 存储类型设置
storage_type: file

# 文件存储特定配置
file_storage:
  # 数据文件路径
  data_path: "plugins/Codex/players/"
  
  # 文件格式
  format: "yml"  # 支持 yml, json
  
  # 备份设置
  backup:
    enabled: true
    max_backups: 10
    backup_interval: 86400  # 24小时
  
  # 文件压缩（节省空间）
  compression:
    enabled: false
    format: "gzip"
```

### 优点和缺点

**优点**：
- ✅ 配置简单，无需外部依赖
- ✅ 数据可直接编辑
- ✅ 易于备份和迁移
- ✅ 适合调试和开发

**缺点**：
- ❌ 大量玩家时性能较差
- ❌ 不支持复杂查询
- ❌ 不支持跨服数据共享
- ❌ 并发访问可能导致数据丢失

### 数据结构示例

```yaml
# plugins/Codex/players/player_uuid.yml
player_data:
  uuid: "550e8400-e29b-41d4-a716-446655440000"
  name: "Steve"
  first_join: 1699123456789
  last_seen: 1699209856789
  
discoveries:
  items:
    first_diamond:
      unlocked: true
      unlock_time: 1699123500000
      unlock_date: "2024-11-04"
    first_iron:
      unlocked: true
      unlock_time: 1699123600000
      unlock_date: "2024-11-04"
  
  monsters:
    zombie_killer:
      unlocked: false
  
statistics:
  total_discoveries: 2
  categories_completed: 0
  last_discovery: "first_iron"
```

## H2 数据库存储

### 配置说明

H2 是推荐的存储方式，提供优秀的性能且无需额外配置：

```yaml
# 存储类型设置
storage_type: h2

# H2数据库配置
h2_database:
  # 数据库文件路径
  file_path: "plugins/Codex/data/codex.db"
  
  # 连接池配置
  pool:
    # 连接超时时间（毫秒）
    connectionTimeout: 5000
    
    # 最大连接数
    maximumPoolSize: 10
    
    # 最小空闲连接数
    minimumIdle: 2
    
    # 连接保活时间（毫秒）
    keepaliveTime: 30000
    
    # 空闲超时时间（毫秒）
    idleTimeout: 600000
    
    # 连接最大生存时间（毫秒）
    maxLifetime: 1800000
  
  # 数据库优化
  optimization:
    # 启用WAL模式（提高并发性能）
    wal_mode: true
    
    # 缓存大小（KB）
    cache_size: 10000
    
    # 同步模式
    synchronous: "NORMAL"  # OFF, NORMAL, FULL
    
    # 日志大小限制（MB）
    journal_size_limit: 100
```

### 优点和缺点

**优点**：
- ✅ 高性能，支持SQL查询
- ✅ 无需外部数据库服务器
- ✅ 支持事务和并发访问
- ✅ 自动优化和压缩
- ✅ 内置备份和恢复功能

**缺点**：
- ❌ 不支持跨服数据共享
- ❌ 需要更多内存
- ❌ 数据库文件损坏风险

### 性能优化

```yaml
h2_database:
  # 高性能配置
  pool:
    maximumPoolSize: 20
    connectionTimeout: 3000
    
  optimization:
    # 关闭同步以提高写入速度（但增加数据丢失风险）
    synchronous: "OFF"
    
    # 增大缓存
    cache_size: 50000
    
    # 批量提交
    batch_size: 100
    
    # 预编译语句缓存
    prepared_statement_cache: 100
```

## MySQL 数据库存储

### 配置说明

MySQL 数据库适合大型服务器和网络服务器：

```yaml
# 存储类型设置
storage_type: mysql

# MySQL数据库配置
mysql_database:
  # 数据库连接信息
  host: "localhost"
  port: 3306
  database: "codex"
  username: "codex_user"
  password: "your_secure_password"
  
  # SSL 连接
  ssl:
    enabled: false
    trust_certificate: true
    verify_hostname: false
  
  # 连接池配置
  pool:
    # 连接超时时间（毫秒）
    connectionTimeout: 5000
    
    # 最大连接数
    maximumPoolSize: 15
    
    # 最小空闲连接数
    minimumIdle: 3
    
    # 连接保活时间（毫秒）
    keepaliveTime: 30000
    
    # 空闲超时时间（毫秒）
    idleTimeout: 600000
    
    # 连接最大生存时间（毫秒）
    maxLifetime: 1800000
  
  # 字符集配置
  charset: "utf8mb4"
  collation: "utf8mb4_unicode_ci"
  
  # 表前缀
  table_prefix: "codex_"
```

### 数据库准备

**创建数据库**：
```sql
CREATE DATABASE codex 
CHARACTER SET utf8mb4 
COLLATE utf8mb4_unicode_ci;
```

**创建用户**：
```sql
CREATE USER 'codex_user'@'localhost' IDENTIFIED BY 'your_secure_password';
GRANT ALL PRIVILEGES ON codex.* TO 'codex_user'@'localhost';
FLUSH PRIVILEGES;
```

**跨服配置**：
```sql
-- 允许远程连接
CREATE USER 'codex_user'@'%' IDENTIFIED BY 'your_secure_password';
GRANT ALL PRIVILEGES ON codex.* TO 'codex_user'@'%';
FLUSH PRIVILEGES;
```

### 优点和缺点

**优点**：
- ✅ 支持跨服数据共享
- ✅ 高性能和可扩展性
- ✅ 完整的SQL功能
- ✅ 专业的备份和恢复工具
- ✅ 支持集群和高可用

**缺点**：
- ❌ 需要额外的数据库服务器
- ❌ 配置相对复杂
- ❌ 网络延迟影响性能
- ❌ 需要数据库管理知识

### 高级配置

```yaml
mysql_database:
  # 连接字符串参数
  connection_properties:
    useSSL: false
    useUnicode: true
    characterEncoding: "utf8mb4"
    autoReconnect: true
    failOverReadOnly: false
    maxReconnects: 3
    
  # 查询优化
  optimization:
    # 批量插入大小
    batch_insert_size: 1000
    
    # 查询缓存
    query_cache_size: 1000
    
    # 预编译语句
    use_prepared_statements: true
    
    # 事务超时（秒）
    transaction_timeout: 30
```

## 数据库表结构

### 自动生成表结构

Codex 会在首次启动时自动创建所需的数据库表：

```sql
-- 玩家基础信息表
CREATE TABLE codex_players (
    id INT AUTO_INCREMENT PRIMARY KEY,
    uuid VARCHAR(36) UNIQUE NOT NULL,
    name VARCHAR(16) NOT NULL,
    first_join BIGINT NOT NULL,
    last_seen BIGINT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- 发现记录表
CREATE TABLE codex_discoveries (
    id INT AUTO_INCREMENT PRIMARY KEY,
    player_uuid VARCHAR(36) NOT NULL,
    category VARCHAR(50) NOT NULL,
    discovery_id VARCHAR(100) NOT NULL,
    unlocked BOOLEAN DEFAULT FALSE,
    unlock_time BIGINT NULL,
    unlock_date DATE NULL,
    data TEXT NULL,
    UNIQUE KEY unique_discovery (player_uuid, category, discovery_id),
    FOREIGN KEY (player_uuid) REFERENCES codex_players(uuid) ON DELETE CASCADE
);

-- 统计数据表
CREATE TABLE codex_statistics (
    id INT AUTO_INCREMENT PRIMARY KEY,
    player_uuid VARCHAR(36) NOT NULL,
    stat_type VARCHAR(50) NOT NULL,
    stat_value BIGINT DEFAULT 0,
    last_updated BIGINT NOT NULL,
    UNIQUE KEY unique_stat (player_uuid, stat_type),
    FOREIGN KEY (player_uuid) REFERENCES codex_players(uuid) ON DELETE CASCADE
);

-- 缓存数据表（可选）
CREATE TABLE codex_cache (
    cache_key VARCHAR(255) PRIMARY KEY,
    cache_value LONGTEXT NOT NULL,
    expire_time BIGINT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

## 数据迁移

### 迁移工具

Codex 提供内置的数据迁移工具：

```bash
# 从文件迁移到H2
/codex database migrate file h2

# 从H2迁移到MySQL
/codex database migrate h2 mysql

# 查看迁移进度
/codex database migrate status

# 取消正在进行的迁移
/codex database migrate cancel
```

### 手动迁移步骤

**1. 备份原数据**：
```bash
# 备份文件数据
cp -r plugins/Codex/players/ backup/

# 备份H2数据库
/codex database backup h2_backup
```

**2. 配置新存储方式**：
```yaml
# 修改config.yml
storage_type: mysql  # 新的存储类型
```

**3. 执行迁移**：
```bash
/codex database migrate h2 mysql
```

**4. 验证数据**：
```bash
# 检查数据完整性
/codex database verify

# 对比迁移前后的数据
/codex database compare backup_file current_mysql
```

## 性能优化

### 通用优化配置

```yaml
# 性能优化配置
performance:
  # 批量操作配置
  batch_operations:
    enabled: true
    batch_size: 100
    batch_timeout: 5000  # 毫秒
  
  # 异步操作
  async_operations:
    enabled: true
    thread_pool_size: 4
    queue_size: 1000
  
  # 缓存配置
  cache:
    enabled: true
    max_size: 10000
    expire_time: 3600  # 秒
    cleanup_interval: 300  # 秒
  
  # 连接池监控
  monitoring:
    enabled: true
    log_slow_queries: true
    slow_query_threshold: 1000  # 毫秒
```

### 服务器级优化

**Java虚拟机优化**：
```bash
# 启动参数优化
-Xms2G -Xmx4G
-XX:+UseG1GC
-XX:MaxGCPauseMillis=50
-XX:+ParallelRefProcEnabled
```

**系统级优化**：
```bash
# 增加文件描述符限制
ulimit -n 65536

# 优化网络参数
echo 'net.core.somaxconn = 65535' >> /etc/sysctl.conf
echo 'net.ipv4.tcp_max_syn_backlog = 65535' >> /etc/sysctl.conf
```

## 监控与维护

### 性能监控

```yaml
# 监控配置
monitoring:
  # 启用性能监控
  enabled: true
  
  # 监控间隔（秒）
  interval: 60
  
  # 监控指标
  metrics:
    - connection_pool_usage
    - query_execution_time
    - memory_usage
    - cache_hit_rate
    - disk_usage
  
  # 告警配置
  alerts:
    # 连接池使用率告警
    connection_pool_threshold: 80
    
    # 慢查询告警
    slow_query_threshold: 2000  # 毫秒
    
    # 内存使用告警
    memory_threshold: 85  # 百分比
```

### 定期维护

```yaml
# 维护配置
maintenance:
  # 自动清理
  auto_cleanup:
    enabled: true
    
    # 清理旧的缓存数据
    cache_retention_days: 7
    
    # 清理断开连接的玩家数据
    offline_player_retention_days: 90
    
    # 清理错误日志
    error_log_retention_days: 30
  
  # 数据库优化
  database_optimization:
    enabled: true
    
    # 优化间隔（小时）
    interval: 24
    
    # 重建索引
    rebuild_indexes: true
    
    # 更新统计信息
    update_statistics: true
```

### 备份策略

```yaml
# 备份配置
backup:
  # 自动备份
  auto_backup:
    enabled: true
    interval: "0 2 * * *"  # 每天凌晨2点
    
  # 备份保留策略
  retention:
    daily_backups: 7    # 保留7天的日备份
    weekly_backups: 4   # 保留4周的周备份
    monthly_backups: 12 # 保留12个月的月备份
  
  # 备份压缩
  compression:
    enabled: true
    format: "gzip"
    compression_level: 6
```

## 故障排除

### 常见问题

#### 问题：数据库连接失败
**排查步骤**：
1. 检查数据库服务是否运行
2. 验证连接参数是否正确
3. 检查防火墙和网络连接
4. 查看数据库用户权限

#### 问题：性能下降
**排查步骤**：
1. 检查连接池使用情况
2. 分析慢查询日志
3. 监控内存和CPU使用率
4. 检查磁盘空间和I/O

#### 问题：数据不一致
**排查步骤**：
1. 检查事务配置
2. 验证并发控制设置
3. 查看错误日志
4. 执行数据完整性检查

## 下一步

了解了数据存储系统后，您可以继续学习：

1. **[第三方插件集成](../integrations/item-plugins.md)** - 与其他插件的数据集成
2. **[配置优化](../configuration/basic-config.md)** - 深入了解配置选项
3. **[性能调优](../admin/performance.md)** - 服务器性能优化
4. **[故障排除](../troubleshooting/performance-issues.md)** - 解决性能问题
