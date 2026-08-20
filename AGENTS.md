# EcoTag（主题版）技术架构

## 概述

本主题基于 MAML（Mi Assistant Markup Language）语法编写，目标平台为小米背屏（REAREye）。通过系统 ContentProvider 和广播变量更新器获取数据，无需额外应用。

## MAML 语法约定

### 变量绑定

- **useVariableUpdater**：`Battery,DateTime.Second`，用于获取电池电量与实时时间
- **变量前缀**：`var.` 表示用户可配置变量（来自 var_config.xml），`sys.` 表示系统变量

### 数据源

使用 `ContentProviderBinder` 元素绑定三个数据源：

1. **getStorageData**：`content://com.miui.securitycenter.widgetProvider/getCleanMasterData`
2. **getMemoryData**：同上 URI，取 `memoryOccupied` 字段
3. **MiSteps**：`content://com.mi.health.provider.main/activity/steps/brief`

### 能效等级计算

通过 `batteryLevel` 变量判断：
- 1 级：>80%
- 2 级：>60% 且 ≤80%
- 3 级：>40% 且 ≤60%
- 4 级：>20% 且 ≤40%
- 5 级：≤20%

### 动画系统

- **充电流光动画**：`VariableAnimation` 0→360 度，2 秒循环，仅在充电状态（`Battery.Charging`）时播放
- **AOD 模式**：通过 `enterAod` / `exitAod` trigger 控制 `aodState` 变量，切换省电布局

### 布局元素

- **Arc / Line**：绘制胶囊电池路径
- **Text**：显示能效等级、数据指标
- **Image**：背景图、EcoTag图标

## 广播数据格式

不依赖外部广播，所有数据通过系统 ContentProvider 获取。

## 双机型适配

通过 `@screenWidth / @screenHeight` 宽高比判断 Pro（约 1:1）与 Pro Max（约 1:1.3），自动切换布局参数。

## 注意事项

1. 依赖小米手机管家（`com.miui.securitycenter`）和小米健康（`com.mi.health`）提供 ContentProvider
2. 非小米设备或未安装对应应用时，部分数据可能显示为空
3. AOD 模式下自动降低刷新频率以节省电量
4. manifest.xml 必须为 UTF-8 编码，所有 XML 标签必须正确闭合
