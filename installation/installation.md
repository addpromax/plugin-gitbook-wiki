# 安装指南

本章将指导您完成 Codex 插件的完整安装过程。

## 安装前准备

### 1. 检查系统要求

在安装前，请确保您的服务器满足以下要求：

- ✅ Minecraft 1.13 或更高版本
- ✅ Paper 或 Spigot 服务器核心
- ✅ Java 8 或更高版本
- ✅ 至少 512MB 可用内存

### 2. 备份服务器

> ⚠️ **重要提示**：在安装任何插件之前，建议备份您的服务器数据！

```bash
# 停止服务器
stop

# 备份关键目录
cp -r plugins/ plugins_backup/
cp -r world/ world_backup/
```

## 安装步骤

### 步骤 1：下载插件

1. 访问 [Spigot 官方页面](https://www.spigotmc.org/resources/codex-rpg-discoveries.90371/)
2. 点击下载最新版本的 `Codex-x.x.x.jar` 文件
3. 确保下载的是与您的服务器版本兼容的版本

### 步骤 2：安装主插件

1. 将 `Codex-x.x.x.jar` 文件复制到服务器的 `plugins/` 目录
2. 确保文件权限正确（644 或 755）

```bash
# 复制插件文件
cp Codex-3.0.7.jar /path/to/server/plugins/

# 设置文件权限
chmod 644 /path/to/server/plugins/Codex-3.0.7.jar
```

### 步骤 3：安装依赖插件（可选）

根据您的需求安装以下可选依赖：

#### PlaceholderAPI（推荐）
- **作用**：提供变量支持，用于奖励消息和GUI显示
- **下载**：[PlaceholderAPI](https://www.spigotmc.org/resources/placeholderapi.6245/)

#### WorldGuard（区域功能）
- **作用**：支持区域进入触发器
- **下载**：[WorldGuard](https://dev.bukkit.org/projects/worldguard)

#### 物品插件支持
- **ItemsAdder**：[官方页面](https://www.spigotmc.org/resources/itemsadder.73355/)
- **MMOItems**：[官方页面](https://www.spigotmc.org/resources/mmoitems.39267/)

#### 附魔插件支持
- **Aiyatsbus**：[官方页面](https://www.spigotmc.org/resources/aiyatsbus.110939/)
- **EcoEnchants**：[官方页面](https://www.spigotmc.org/resources/ecoenchants.79573/)

#### 自动图鉴支持
- **CustomFishing**：[官方页面](https://www.spigotmc.org/resources/customfishing.63868/)
- **CustomCrops**：[官方页面](https://www.spigotmc.org/resources/customcrops.103964/)

### 步骤 4：首次启动

1. **启动服务器**
   ```bash
   java -jar server.jar
   ```

2. **验证安装**
   
   在服务器控制台查找以下信息：
   ```
   [INFO] [Codex] Loading Codex v3.0.7
   [INFO] [Codex] Codex enabled successfully!
   [INFO] [Codex] Hook状态检查：
   [INFO] [Codex] - PlaceholderAPI: 成功
   [INFO] [Codex] - WorldGuard: 成功
   [INFO] [Codex] - ItemsAdder: 未找到
   ```

3. **检查配置文件**
   
   启动后，插件会在 `plugins/Codex/` 目录下生成以下文件：
   ```
   plugins/Codex/
   ├── config.yml          # 主配置文件
   ├── messages.yml        # 消息配置
   ├── inventory.yml       # 界面配置
   ├── categories/         # 分类配置目录
   │   ├── history.yml
   │   ├── monsters.yml
   │   └── regions.yml
   ├── enchantments.yml    # 附魔图鉴配置
   ├── fishing.yml         # 钓鱼图鉴配置
   ├── crops.yml           # 作物图鉴配置
   └── players/           # 玩家数据目录
   ```

### 步骤 5：权限配置

为管理员设置必要的权限：

```yaml
# permissions.yml 或权限插件配置
permissions:
  codex.admin:
    description: "Codex管理员权限"
    children:
      codex.command.reload: true
      codex.command.database: true
      codex.command.debug: true
  
  codex.player:
    description: "Codex玩家权限"
    children:
      codex.command.main: true
      codex.gui.open: true
```

## 验证安装

### 测试基本功能

1. **检查插件状态**
   ```
   /plugins
   ```
   确认 Codex 显示为绿色

2. **测试基本命令**
   ```
   /codex
   ```
   应该打开主界面GUI

3. **检查配置重载**
   ```
   /codex reload
   ```
   应该显示重载成功消息

### 常见安装问题

#### 问题：插件无法加载
**可能原因**：
- Java版本过低
- 服务器版本不兼容
- 文件损坏

**解决方案**：
1. 检查控制台错误信息
2. 确认Java和服务器版本
3. 重新下载插件文件

#### 问题：配置文件未生成
**可能原因**：
- 文件权限不足
- 磁盘空间不足

**解决方案**：
1. 检查目录权限
2. 确保磁盘空间充足
3. 手动创建目录

#### 问题：依赖插件挂钩失败
**可能原因**：
- 依赖插件版本不兼容
- 依赖插件未正确安装

**解决方案**：
1. 检查依赖插件版本
2. 确认依赖插件已启用
3. 重启服务器

## 下一步

安装完成后，建议您：

1. **[快速配置](quick-setup.md)** - 配置基础功能
2. **[第一次使用](first-use.md)** - 体验插件特性
3. **[基础配置](../configuration/basic-config.md)** - 深入了解配置选项

## 获取帮助

如果在安装过程中遇到问题：

- 📖 查看 [故障排除指南](../troubleshooting/installation-issues.md)
- 💬 访问 [GitHub Issues](https://github.com/addpromax/plugin-gitbook-wiki/issues)
- 📧 联系技术支持
