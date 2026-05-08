# AutoUpdate

自动获取 Hyperion (米游社) 和 HoYoLab 的最新 APK 并提取 K2/LK2 DS Salt。

## 文件说明

- `hyperion_salt_extractor.py` — 核心提取脚本
- `bhyp64.py` — 自定义 Base64 编解码器
- `auto_update.ps1` — Windows 自动更新脚本
- `signature` — BHyp64 编码后的 Salt 签名
- `version` — 应用版本及 SDK 版本

## 基本用法

```bash
# 手动指定 APK
python hyperion_salt_extractor.py myapp.apk

# 自动模式（检查 Hyperion + HoYoLab 更新）
python hyperion_salt_extractor.py --auto

# 仅检查 Hyperion
python hyperion_salt_extractor.py --auto hyperion

# 静默模式（仅输出结果，配合自动更新使用）
python hyperion_salt_extractor.py --auto -j 2 -m 2048m --silent
python hyperion_salt_extractor.py myapp.apk --app hyperion --silent
```

## 静默模式输出格式

```
Hyperion:1
HoYoLab:0
```

- `1` — 无需更新或提取成功
- `0` — 提取失败

## 自动更新

### 单次执行

```powershell
.\auto_update.ps1
```

### 循环模式（每 2 小时）

```powershell
.\auto_update.ps1 -Loop
```

### 配合 Windows 任务计划程序

1. 打开「任务计划程序」
2. 创建基本任务，触发器设为「每天」，间隔设为「每 2 小时」
3. 操作：启动程序 `powershell.exe`
4. 参数：`-ExecutionPolicy Bypass -File "D:\Project\auto_bbs_info\auto_update.ps1"`

检测到更新后会自动 commit 并 push `signature` 和 `version` 到 `origin/main`。