# 工训赛 STM32 双板控制固件与 PCB

此仓库发布固件、测试/构建脚本，以及嘉立创EDA专业版PCB工程和连接参考资料。

## 下载完整工程

[下载固件与PCB完整源码包（ZIP）](gongxunsai-firmware-source.zip)

包内包含可直接解压的完整源码目录。PCB目录含2026-10-30原生工程、2026-10-18参考网表和2026-10-11旧版Gerber。请阅读包内 `Hardware/PCB/README.md`：旧Gerber不是当前板子的制造文件，制作前应从最新原生工程重新导出并检查。

ZIP SHA-256：`231AE2922D27B20CD30C098A8E937D3F007A9B2964FCCDA3305FBFE5BC428C23`

## 构建与测试

Windows PowerShell：

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File .\scripts\build.ps1
powershell -NoProfile -ExecutionPolicy Bypass -File .\scripts\test.ps1
```

需要 CMake、Ninja、GNU Arm Embedded 工具链；编译不会验证实际接线、机械方向或硬件急停。

## 硬件安全

这是面向指定自制硬件的实验工程，未经整机安全验收。首次试机请卸载负载、准备独立断电方式，并逐轴低速确认方向和行程。软件限位不能替代硬件急停。

## 许可证

项目自有代码、文档及自有PCB设计文件以MIT License发布，详见压缩包中的 `LICENSE`。第三方CMSIS/HAL和嵌入的EDA库资产仍遵循各自上游许可；说明见 `THIRD_PARTY_NOTICES.md`。
