# API 文档概述

Codex 插件提供了丰富的 API 接口，允许其他插件开发者集成和扩展 Codex 的功能。本章介绍 API 的基本使用方法和核心接口。

## API 简介

### 设计理念

Codex API 设计遵循以下原则：

- **易于使用**：简洁直观的接口设计
- **高度灵活**：支持各种自定义需求
- **向后兼容**：API 版本更新时保持兼容性
- **性能优先**：异步操作和优化的数据结构
- **事件驱动**：基于事件系统的扩展机制

### API 版本

当前 API 版本：`3.0.7`

```java
// 获取 API 版本
String apiVersion = CodexAPI.getVersion();
System.out.println("Codex API Version: " + apiVersion);
```

## 快速开始

### 依赖配置

#### Maven 配置

```xml
<dependencies>
    <dependency>
        <groupId>cx.ajneb97</groupId>
        <artifactId>Codex</artifactId>
        <version>3.0.7</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```

#### Gradle 配置

```gradle
dependencies {
    compileOnly 'cx.ajneb97:Codex:3.0.7'
}
```

### 基础集成

#### 获取 API 实例

```java
import cx.ajneb97.api.CodexAPI;

public class MyPlugin extends JavaPlugin {
    
    private CodexAPI codexAPI;
    
    @Override
    public void onEnable() {
        // 检查 Codex 插件是否存在
        if (getServer().getPluginManager().getPlugin("Codex") == null) {
            getLogger().warning("Codex plugin not found!");
            getServer().getPluginManager().disablePlugin(this);
            return;
        }
        
        // 获取 API 实例
        codexAPI = CodexAPI.getInstance();
        
        if (codexAPI != null) {
            getLogger().info("Successfully hooked into Codex!");
        }
    }
}
```

## 核心 API 类

### CodexAPI 主类

#### 基础方法

```java
// 获取 API 实例
CodexAPI api = CodexAPI.getInstance();

// 检查 API 是否可用
boolean isEnabled = api.isEnabled();

// 获取插件版本
String version = api.getVersion();

// 获取数据库管理器
DatabaseManager dbManager = api.getDatabaseManager();

// 获取发现管理器
DiscoveryManager discoveryManager = api.getDiscoveryManager();
```

### 玩家数据 API

#### PlayerData 类

```java
import cx.ajneb97.api.PlayerData;

// 获取玩家数据
PlayerData playerData = codexAPI.getPlayerData(player);

if (playerData != null) {
    // 获取玩家总发现数
    int totalDiscoveries = playerData.getTotalDiscoveries();
    
    // 获取特定分类的发现数
    int categoryCount = playerData.getCategoryDiscoveries("items");
    
    // 检查是否已解锁特定发现
    boolean hasDiscovery = playerData.hasDiscovery("first_diamond");
    
    // 获取解锁时间
    long unlockTime = playerData.getDiscoveryUnlockTime("first_diamond");
    
    // 获取所有已解锁的发现
    Set<String> unlockedDiscoveries = playerData.getUnlockedDiscoveries();
}
```

#### 异步数据操作

```java
// 异步加载玩家数据
codexAPI.getPlayerDataAsync(player, new PlayerCallback<PlayerData>() {
    @Override
    public void onSuccess(PlayerData data) {
        // 数据加载成功
        int discoveries = data.getTotalDiscoveries();
        player.sendMessage("你有 " + discoveries + " 个发现！");
    }
    
    @Override
    public void onFailure(Throwable throwable) {
        // 数据加载失败
        plugin.getLogger().warning("Failed to load player data: " + throwable.getMessage());
    }
});
```

## 发现系统 API

### 创建自定义发现

```java
import cx.ajneb97.api.Discovery;
import cx.ajneb97.api.DiscoveryBuilder;

// 使用构建器模式创建发现
Discovery customDiscovery = new DiscoveryBuilder()
    .id("my_plugin_custom_discovery")
    .name("&6自定义发现")
    .description(Arrays.asList(
        "&7这是一个由第三方插件创建的发现",
        "&7展示了 API 的强大功能"
    ))
    .category("custom")
    .triggerType(TriggerType.ITEM_OBTAIN)
    .triggerValue("item_type", "DIAMOND")
    .reward("console_command", "give %player% emerald 5")
    .reward("message", "&a恭喜获得自定义发现奖励！")
    .build();

// 注册发现
codexAPI.registerDiscovery(customDiscovery);
```

### 手动触发发现

```java
// 强制触发玩家的发现
boolean success = codexAPI.triggerDiscovery(player, "custom_discovery_id");

if (success) {
    plugin.getLogger().info("Successfully triggered discovery for " + player.getName());
} else {
    plugin.getLogger().warning("Failed to trigger discovery");
}

// 批量触发发现
List<String> discoveryIds = Arrays.asList("discovery1", "discovery2", "discovery3");
codexAPI.triggerDiscoveriesAsync(player, discoveryIds, new PlayerCallback<Boolean>() {
    @Override
    public void onSuccess(Boolean result) {
        if (result) {
            player.sendMessage("&a所有发现已触发！");
        }
    }
    
    @Override
    public void onFailure(Throwable throwable) {
        plugin.getLogger().severe("Failed to trigger discoveries: " + throwable.getMessage());
    }
});
```

## 事件系统

### 监听 Codex 事件

```java
import cx.ajneb97.api.events.*;
import org.bukkit.event.EventHandler;
import org.bukkit.event.Listener;

public class CodexEventListener implements Listener {
    
    @EventHandler
    public void onDiscoveryUnlock(DiscoveryUnlockEvent event) {
        Player player = event.getPlayer();
        Discovery discovery = event.getDiscovery();
        
        // 发现被解锁时的处理
        String discoveryName = discovery.getName();
        String category = discovery.getCategory();
        
        // 发送自定义消息
        player.sendMessage("&e恭喜解锁发现：&f" + discoveryName);
        
        // 记录到日志
        plugin.getLogger().info(player.getName() + " unlocked discovery: " + discovery.getId());
        
        // 可以取消事件阻止发现解锁
        if (someCondition) {
            event.setCancelled(true);
        }
    }
    
    @EventHandler
    public void onCategoryComplete(CategoryCompleteEvent event) {
        Player player = event.getPlayer();
        String category = event.getCategory();
        
        // 分类完成时的处理
        Bukkit.broadcastMessage("&6" + player.getName() + " &e完成了 &f" + category + " &e分类！");
        
        // 给予额外奖励
        if ("items".equals(category)) {
            player.getInventory().addItem(new ItemStack(Material.DIAMOND, 10));
        }
    }
    
    @EventHandler
    public void onDataLoad(PlayerDataLoadEvent event) {
        Player player = event.getPlayer();
        PlayerData data = event.getPlayerData();
        
        // 玩家数据加载完成
        int totalDiscoveries = data.getTotalDiscoveries();
        
        if (totalDiscoveries >= 100) {
            player.sendMessage("&6欢迎回来，发现大师！");
        }
    }
}
```

### 自定义事件

```java
import cx.ajneb97.api.events.CustomDiscoveryEvent;

// 创建自定义事件
CustomDiscoveryEvent customEvent = new CustomDiscoveryEvent(player, "my_custom_trigger", data);

// 触发事件
Bukkit.getPluginManager().callEvent(customEvent);

// 检查事件是否被取消
if (!customEvent.isCancelled()) {
    // 执行后续逻辑
}
```

## 数据库 API

### 数据库操作

```java
import cx.ajneb97.api.DatabaseManager;

DatabaseManager dbManager = codexAPI.getDatabaseManager();

// 异步保存玩家数据
dbManager.savePlayerDataAsync(playerData, new PlayerCallback<Boolean>() {
    @Override
    public void onSuccess(Boolean success) {
        if (success) {
            plugin.getLogger().info("Player data saved successfully");
        }
    }
    
    @Override
    public void onFailure(Throwable throwable) {
        plugin.getLogger().severe("Failed to save player data: " + throwable.getMessage());
    }
});

// 批量操作
List<PlayerData> dataList = Arrays.asList(data1, data2, data3);
dbManager.batchSaveAsync(dataList, callback);
```

### 自定义查询

```java
// 执行自定义查询
String sql = "SELECT COUNT(*) FROM codex_discoveries WHERE category = ? AND unlocked = true";
Object[] params = {"items"};

dbManager.executeQueryAsync(sql, params, new PlayerCallback<ResultSet>() {
    @Override
    public void onSuccess(ResultSet resultSet) {
        try {
            if (resultSet.next()) {
                int count = resultSet.getInt(1);
                plugin.getLogger().info("Total unlocked items: " + count);
            }
        } catch (SQLException e) {
            plugin.getLogger().severe("Error processing query result: " + e.getMessage());
        }
    }
    
    @Override
    public void onFailure(Throwable throwable) {
        plugin.getLogger().severe("Query failed: " + throwable.getMessage());
    }
});
```

## 集成示例

### 与经济插件集成

```java
import net.milkbowl.vault.economy.Economy;

public class EconomyIntegration {
    
    private Economy economy;
    private CodexAPI codexAPI;
    
    public void setupIntegration() {
        // 设置经济系统
        RegisteredServiceProvider<Economy> rsp = getServer().getServicesManager()
            .getRegistration(Economy.class);
        economy = rsp.getProvider();
        
        // 获取 Codex API
        codexAPI = CodexAPI.getInstance();
        
        // 注册事件监听器
        getServer().getPluginManager().registerEvents(new EconomyListener(), this);
    }
    
    public class EconomyListener implements Listener {
        
        @EventHandler
        public void onDiscoveryUnlock(DiscoveryUnlockEvent event) {
            Player player = event.getPlayer();
            Discovery discovery = event.getDiscovery();
            
            // 根据发现稀有度给予不同的金钱奖励
            double reward = calculateReward(discovery);
            
            if (reward > 0) {
                economy.depositPlayer(player, reward);
                player.sendMessage("&a获得 $" + reward + " 作为发现奖励！");
            }
        }
        
        private double calculateReward(Discovery discovery) {
            // 根据发现属性计算奖励金额
            String category = discovery.getCategory();
            
            switch (category) {
                case "items": return 100.0;
                case "monsters": return 200.0;
                case "fishing": return 150.0;
                case "enchantments": return 300.0;
                default: return 50.0;
            }
        }
    }
}
```

### 与权限插件集成

```java
import net.luckperms.api.LuckPerms;
import net.luckperms.api.LuckPermsProvider;

public class PermissionIntegration {
    
    private LuckPerms luckPerms;
    
    public void setupIntegration() {
        luckPerms = LuckPermsProvider.get();
        
        // 监听分类完成事件
        getServer().getPluginManager().registerEvents(new PermissionListener(), this);
    }
    
    public class PermissionListener implements Listener {
        
        @EventHandler
        public void onCategoryComplete(CategoryCompleteEvent event) {
            Player player = event.getPlayer();
            String category = event.getCategory();
            
            // 根据完成的分类给予特殊权限
            String permission = "codex.master." + category;
            
            luckPerms.getUserManager().modifyUser(player.getUniqueId(), user -> {
                user.data().add(Node.builder(permission).build());
            });
            
            player.sendMessage("&a获得新权限: " + permission);
        }
    }
}
```

## API 最佳实践

### 1. 异步操作

```java
// 好的做法：使用异步操作
codexAPI.getPlayerDataAsync(player, new PlayerCallback<PlayerData>() {
    @Override
    public void onSuccess(PlayerData data) {
        // 在主线程中更新 UI
        Bukkit.getScheduler().runTask(plugin, () -> {
            updatePlayerGUI(player, data);
        });
    }
    
    @Override
    public void onFailure(Throwable throwable) {
        plugin.getLogger().warning("Failed to load data: " + throwable.getMessage());
    }
});

// 避免的做法：阻塞主线程
// PlayerData data = codexAPI.getPlayerData(player); // 可能造成卡顿
```

### 2. 错误处理

```java
// 完善的错误处理
try {
    boolean result = codexAPI.triggerDiscovery(player, discoveryId);
    if (!result) {
        plugin.getLogger().warning("Discovery trigger returned false for: " + discoveryId);
    }
} catch (IllegalArgumentException e) {
    plugin.getLogger().severe("Invalid discovery ID: " + discoveryId);
} catch (Exception e) {
    plugin.getLogger().severe("Unexpected error triggering discovery: " + e.getMessage());
    e.printStackTrace();
}
```

### 3. 资源管理

```java
// 正确的资源清理
@Override
public void onDisable() {
    // 取消所有异步任务
    codexAPI.cancelAllTasks(this);
    
    // 清理监听器
    HandlerList.unregisterAll(this);
    
    // 保存重要数据
    codexAPI.saveAllData();
}
```

## API 参考

### 接口列表

| 接口名称 | 说明 |
|----------|------|
| `CodexAPI` | 主要 API 接口 |
| `PlayerData` | 玩家数据操作 |
| `Discovery` | 发现数据结构 |
| `DiscoveryManager` | 发现管理器 |
| `DatabaseManager` | 数据库管理器 |
| `CategoryManager` | 分类管理器 |

### 事件列表

| 事件名称 | 说明 |
|----------|------|
| `DiscoveryUnlockEvent` | 发现解锁事件 |
| `CategoryCompleteEvent` | 分类完成事件 |
| `PlayerDataLoadEvent` | 玩家数据加载事件 |
| `DatabaseSaveEvent` | 数据库保存事件 |

## 下一步

现在您已经了解了 API 的基础使用，可以继续学习：

1. **[事件系统](events.md)** - 深入了解事件处理
2. **[数据结构](data-structures.md)** - 详细的数据模型
3. **[扩展开发](extensions.md)** - 开发自定义扩展

如需更多帮助，请访问：
- **GitHub Wiki**：详细的 API 文档
- **JavaDoc**：在线 API 参考
- **示例项目**：完整的集成示例
