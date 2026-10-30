# 工训赛 STM32 双板控制固件与 PCB

包含 STM32F103C8T6 主从板固件、构建/测试脚本，以及嘉立创EDA专业版 PCB 源工程和连接参考资料。PCB文件版本说明与旧Gerber的限制见 [Hardware/PCB/README.md](Hardware/PCB/README.md)。
STM32F103C8T6 双 MCU 固件工程：U1 为 MASTER，U3 为 IO。从板负责输入采集、舵机 PWM 与状态回报；主板负责调试/视觉串口、SPI 通信、OLED 状态和 X42S 步进驱动协议。

这是面向指定自制硬件的实验工程。源码可编译、主机测试通过不等于整机已完成安全验收。手动串口命令会向 X42S 发运动报文；首次试机请先卸载夹具/负载、让运动部件处于安全范围、准备独立断电方式，并逐轴低速确认方向、限位极性和行程。不要把软件限位当作硬件急停。

## 当前实现

- 主板调试串口：115200、8N1；接受 `ZERO`、`HOME`、`XY x y`、`XYZS x y z s`、`R3 turns`、`R4 turns`、`STOP3`、`STOP4`、`STOPALL` 和 `Pdddd`。
- 电机地址约定为 1=X、2=Y、3=Z、4=S。坐标单位为 mm。当前软件范围为 X 0–222 mm、Y 0–208 mm、Z/S 0–500 mm。
- X/Y/Z/S 运动规划使用每转 12800 个命令计数、每转 40 mm、最高 60 RPM；这些参数需与驱动器和传动匹配。
- `ZERO` 建立软件坐标原点；`HOME` 返回已保存坐标，不是自动寻找所有轴的机械开关。Z 轴 CN3/PA1 限位触发会停止并设为软件 Z=0。
- `P0500`–`P1100` 为舵机脉宽命令。`SERVO FORWARD QUEUED` 只表示已排队/转发，不表示舵机已到位。
- 视觉输入 USART1；调试 USART2；主从板 SPI1；X42S 串口 USART3。默认 115200、8N1。

PCB源工程、参考网表和旧版Gerber随包提供，版本关系及制造限制见 [Hardware/PCB/README.md](Hardware/PCB/README.md)。主板 `ZERO` 当前默认采用软件零点模式。此实验工程未经整机安全验收；软件限位不能替代硬件急停。

## 构建与测试

需要 CMake 3.20+、Ninja、GNU Arm Embedded 工具链和 Windows PowerShell。

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File .\scripts\build.ps1
powershell -NoProfile -ExecutionPolicy Bypass -File .\scripts\test.ps1
```

## 烧录

安装 OpenOCD 并将其加入 `PATH`，或设置 `OPENOCD_EXE`。SWD：GND、PA13/SWDIO、PA14/SWCLK，BOOT0 置低。主板 U1 和从板 U3 分别烧录；烧录会覆盖目标MCU应用区。

## 许可证

项目自有代码、文档及自有 PCB 设计文件以 MIT License 发布，见 [LICENSE](LICENSE)。嵌入的第三方符号、封装和库文件可能另有来源许可；STMicroelectronics CMSIS/HAL 文件按上游许可证分发，详见 [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)。
