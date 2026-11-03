# GyroFix 控制台

> 一加 Ace 5 Pro（PKR110）专用的陀螺仪提频 + 触控采样率锁定模块
> 带完整 WebUI 控制台，刷完即用

---

## ✨ 特性

### 🎯 陀螺仪提频
- 开机自动接管，无需打开 App
- 全局高频采样（不依赖游戏白名单）
- `SENSOR_DELAY_FASTEST` 拉满，实测约 **460Hz**
- 尝试 **SensorDirectChannel** 直连通道（`RATE_VERY_FAST` 标称 800Hz），失败自动回退
- 传感器心跳自检，假死自动重连（带重试上限）

### 👆 触控采样率锁定
- 突破系统限制：**123Hz → 348Hz**（实测）
- 写入触控 HAL 寄存器 `0/182`，360 档为硬件上限（实测 364~365Hz）
- 守护进程每 3 秒补写，对抗系统重置
- 游戏进程检测，进游戏自动切档
- **游戏包名可自行增删**

### 🎨 WebUI 控制台
- 深色青绿主题，玻璃拟态卡片
- **实时采样率仪表**，每 2 秒刷新
- 档位切换、游戏列表管理、一键重装
- 完整的更新日志

### 🕶️ 隐藏式设计
- 桌面不显示图标，从 KernelSU 管理器进入
- 通知栏显示当前陀螺仪 + 触控档位

---

## 📱 适配

| 项目 | 说明 |
|---|---|
| 机型 | 一加 Ace 5 Pro（PKR110） |
| 系统 | Android 16 / ColorOS |
| 平台 | 高通骁龙 8 Elite |
| 触控 IC | Synaptics TCM（`synaptics_tcm_hbp`） |
| Root | KernelSU / BakaSU（Magisk 理论可用，未测试） |

> ⚠️ **其他机型不保证可用**，触控部分依赖 `/odm/bin/touchHidlTest` 和特定寄存器地址。

---

## 🚀 安装

1. 下载最新 Release 的 `.zip`
2. 在 KernelSU 管理器里「从本地安装」
3. 重启手机
4. 等 15 秒自动接管，通知栏出现采样率
5. 打开入口：KernelSU → 模块 → GyroFix 控制台 → **打开**

---

## ⚙️ 配置

### WebUI 控制台
- 陀螺仪档位：100 / 200 / 250Hz
- 触控档位：125 / 180 / 240 / 360Hz
- 游戏包名列表（可增删）
- 通知刷新间隔

### 配置文件
```
/data/adb/modules/gyrofix_takeover/config.prop    # 陀螺仪配置
/data/adb/modules/gyrofix_takeover/touch/touch.conf  # 触控配置
```

---

## 📊 实测数据

| 项目 | 修改前 | 修改后 |
|---|---|---|
| 触控采样率 | 123 Hz | **348 Hz** |
| 陀螺仪采样率 | - | **~460 Hz** |

> 数据基于一加 Ace 5 Pro + Android 16 实测

---

## 🔧 日志与排查

**模块日志**
```bash
su -c "cat /data/adb/modules/gyrofix_takeover/logs/service.log"
su -c "cat /data/adb/modules/gyrofix_takeover/logs/touch.log"
```

**App 日志**
```bash
logcat -s GyroFixService GyroFixA11y GyroDirect
```

---

## ⚠️ 注意

- 触控写入属于内核级操作，**刷入前建议先备份**
- 如触控异常，**长按电源键 10 秒强制重启**即可恢复
- 模块追求极致性能，耗电会有所增加
- 与同类触控模块**可能冲突**，请勿同时安装

---

## 📜 致谢

- 触控寄存器地址参考：[reigadegr/pjz110_sampling_controller](https://github.com/reigadegr/pjz110_sampling_controller)
- 触控通道思路参考：LockTouchGuard / 宇川触控模块
- KernelSU / BakaSU 提供运行环境

---

## 📄 License

MIT
