# EcoTag（主题版） - REAREye 锁屏组件

> 主题显示名：**能效标识** | 仓库名：EcoTag

复刻家电能效标签视觉风格的小米背屏 MAML 主题，根据电池电量自动显示 1～5 级能效等级。

## 预览
![预览图](./effect.png)

## 功能
- 🔋 **能效等级**：根据电池电量自动计算 1～5 级能效等级（>80% 为 1 级，≤20% 为 5 级）
- 💾 **存储空间**：显示剩余存储空间
- 🧠 **内存占用**：显示当前内存占用
- ⏱️ **开机时长**：显示系统开机时长
- 🚶 **今日步数**：显示今日步数（通过小米健康 ContentProvider）
- 💡 **电量胶囊**：胶囊电池 UI，支持充电流光动画（2 秒循环）
- 🌙 **AOD 常亮**：支持熄屏显示，自动切换省电模式
- 📱 **双机型适配**：自动适配 Pro / Pro Max 双机型（宽高比判断）
- ⚙️ **可自定义**：标题文字、副标题文字、隐藏电量胶囊开关

## 文件结构
```
├── manifest.xml        # MAML 主题布局文件（变量绑定、数据源、动画逻辑）
├── var_config.xml      # 用户可配置变量（标题、副标题、胶囊开关）
├── effect.png          # 预览图
├── widget_info.json    # REAREye 组件仓库配置
├── strings/            # 多语言资源（zh_CN）
├── AGENTS.md           # 技术架构说明
└── README.md
```

## 数据来源
本主题通过系统 ContentProvider 获取数据，**无需安装额外应用**：

| 数据项 | 来源 |
|--------|------|
| 存储空间 | `content://com.miui.securitycenter.widgetProvider/getCleanMasterData` |
| 内存占用 | 同存储空间，取 `memoryOccupied` 字段 |
| 今日步数 | `content://com.mi.health.provider.main/activity/steps/brief` |
| 电池电量 | 系统广播 `Battery` 变量更新器 |
| 时间 | `DateTime.Second` 变量更新器 |

## 安装
在 REAREye 组件仓库安装，或通过 REAREye 手动导入主题包。

## 变量配置
| 变量名 | 类型 | 说明 |
|--------|------|------|
| `hideCapsule` | 开关 | 隐藏电量胶囊 |
| `customTitle` | 文本（≤20字） | 自定义标题 |
| `customSubtitle` | 文本（≤30字） | 自定义副标题 |

## 版本历史
- **v2.1**：精简胶囊代码、修复 AOD 状态机与语法错误、熄屏省电优化

## 作者
**唯梦倾城**

## 许可
MIT License
