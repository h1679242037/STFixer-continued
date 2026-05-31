# STFixer - SteamTools 存档与离线初始化修复工具

STFixer 是一个 Windows 工具，用来修复 SteamTools 引起的常见问题：部分游戏无法创建/写入存档、SteamTools 后端不可用时无法完成首次初始化、SteamTools Desktop 覆盖补丁 DLL，以及 `xinput1_4.dll` / `dwmapi.dll` 缺失或版本不匹配。

English: [README.en.md](README.en.md)

> 本维护分支基于 @Selectively11 创建的 STFixer；原仓库已归档。授权与许可证说明仍在确认中。

> 对受影响的非拥有游戏，建议先关闭 Steam Cloud，并手动备份存档后再打补丁。

## 下载

从最新 Release 下载 `STFixer.exe`：

https://github.com/h1679242037/STFixer-continued/releases/latest

## 主要用途

- 修复部分 Capcom 游戏无法创建或写入存档的问题。
- 修复 SteamTools 后端不可用时的首次初始化问题。
- 防止 SteamTools Desktop 启动时覆盖已打好的补丁 DLL。
- 修复缺失、过旧或不匹配的 `xinput1_4.dll` 和 `dwmapi.dll`。

## 使用方法

1. 关闭 Steam 和 SteamTools。
2. 运行 `STFixer.exe`。
3. 确认自动检测到的 Steam 路径，或手动输入正确路径。
4. 选择需要的修复项。
5. 完成后重启 Steam 和 SteamTools。

如需撤销修改，重新运行 STFixer，选择 **Disable Everything**。

## 菜单说明

### 1. Setup SteamTools Offline

离线初始化修复。用于 SteamTools 后端服务器不可用、首次安装/修复后无法正常工作的情况。

### 2. Capcom Game Save Fix

存档修复。用于 SteamTools 的云存档行为导致部分游戏无法创建存档的情况。

如果打补丁后仍无法保存，尝试关闭该游戏的 Steam Cloud，清理对应 userdata 目录，重启 Steam 后再测试。

userdata 路径：

```text
<Steam install path>\userdata\<steamid>\<appid>
```

### 3. Patch SteamTools App

修补 `SteamTools.exe`，防止 SteamTools Desktop 每次启动时覆盖 STFixer 已修补的 DLL。

### 4. Repair SteamTools DLLs

重新下载并替换 SteamTools 核心 DLL。适合 DLL 缺失、版本不对或损坏时使用。

注意：如果已经成功执行选项 2，不要立刻再执行选项 4；选项 4 可能会覆盖刚打好的存档修复补丁。如需执行，请之后重新执行选项 2。

### 5. Disable Everything

从 STFixer 自动创建的备份中恢复原始文件，撤销已应用的修改。

## 注意事项

- STFixer 会在修改前自动备份文件。
- Steam 路径会从 Windows 注册表自动检测，也可以手动指定。
- SteamTools 或 Steam 更新后，可能需要重新运行 STFixer。
- 本分支是原仓库归档后的延续维护版。

## 构建

要求：

- .NET 9 SDK
- Windows x64 构建环境
- Visual Studio Build Tools 或 MinGW，用于构建 `Stella\stella_fallback.dll`

先构建 Stella fallback DLL，再发布主程序：

```powershell
cd Stella
.\build.bat
cd ..
dotnet publish .\CloudFix.csproj -c Release
```

如果使用 MinGW：

```powershell
cd Stella
x86_64-w64-mingw32-gcc -shared -O1 -Wall -Wextra -o stella_fallback.dll stella_fallback.c stella_fallback.def -lwinhttp '-Wl,--subsystem,windows' '-Wl,--out-implib,stella_fallback.lib'
cd ..
dotnet publish .\CloudFix.csproj -c Release
```

发布产物位置：

```text
bin\Release\net9.0\win-x64\publish\STFixer.exe
```
