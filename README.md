# 自动售货机 PLC 控制系统

## 🎓 毕业设计项目

基于 **西门子 S7-1200 PLC** + **KTP700 Basic HMI** 的自动售货机控制系统，使用 **TIA Portal V18** 开发。

---

## 📋 系统功能

### 核心控制模式
- **投币与金额累计**：支持 1元、2元、5元、10元 四种面值
- **商品选择与购买**：6种商品（可乐3元、果汁4元、奶茶5元、矿泉水2元、咖啡6元、茶3元）
- **找零与退款**：自动找零（优先大面额）、一键退款
- **缺货预警**：库存≤2件时HMI闪烁提示
- **套餐优惠**：可乐+薯片套餐原价8元→优惠价7元

### 库存与数据管理
- **库存动态管理**：初始库存5件，售出自动递减，为零显示"售罄"
- **销售统计**：自动记录销售数量、总销售额，管理员可查询
- **价格可调**：管理员可在HMI修改售价

### 状态指示
- 欢迎光临 / 请投币 / 出货中 / 余额不足 / 请取商品 / 找零中
- 故障报警：缺货、找零钱箱空、通信故障

---

## 🔧 主要技术参数

| 参数 | 规格 |
|------|------|
| 最大投币金额 | 20元（可设置） |
| 单次交易处理时间 | ≤ 2秒 |
| 找零响应时间 | ≤ 1秒 |
| 商品种类 | 4-6种 |
| 单种商品最大库存 | 99件 |
| 销售记录容量 | ≥ 1000笔 |
| 存储容量 | < 10KB |
| 工作电源 | AC 220V (±10%), 50Hz |
| 控制电压 | DC 24V (±5%), ≥ 2A |
| 待机功耗 | < 15W |
| 峰值功耗 | < 50W |
| 出货驱动信号 | 脉冲2秒, 24V/1A |

---

## 📁 项目结构

```
vending-machine-plc/
├── README.md                    # 项目说明
├── PLC_Program/                 # PLC 程序源文件（TIA Portal V18 可导入）
│   ├── OB1_Main.scl             # 主程序循环 OB1
│   ├── FB_Payment.scl           # 投币与金额管理 FB
│   ├── FB_ProductSelect.scl     # 商品选择与出货 FB
│   ├── FB_Change.scl            # 找零与退款 FB
│   ├── FB_Inventory.scl         # 库存管理 FB
│   ├── FB_Alarm.scl             # 报警管理 FB
│   ├── FC_Display.scl           # 显示控制 FC
│   ├── DB_Global.scl            # 全局数据块
│   ├── DB_Inventory.scl         # 库存数据块
│   └── DB_Sales.scl             # 销售记录数据块
├── HMI/                         # HMI 配置
│   ├── Tags/
│   │   └── HMI_Tags.sdf         # HMI 变量表（可导入）
│   └── Screens/
│       └── HMI_ScreenGuide.md   # 画面设计指南
├── Docs/
│   ├── IO_Table.md              # I/O 地址分配表
│   └── Wiring_Diagram.md       # 接线说明
└── import_guide.md              # TIA Portal 导入指南
```

---

## 🚀 快速开始

### 环境要求
- **TIA Portal V18** (含 PLCSIM)
- **CPU**: S7-1200 (推荐 CPU 1214C DC/DC/DC)
- **HMI**: KTP700 Basic 或使用 WinCC 仿真

### 导入步骤
1. 在 TIA Portal V18 中创建新项目
2. 添加 PLC 设备（CPU 1214C）
3. 右键 PLC 项目树 → "External source file" → "Add external source file"
4. 依次导入 `PLC_Program/` 目录下所有 `.scl` 文件
5. 右键源文件 → "Compile" 编译
6. 添加 HMI 设备 → 导入 `HMI/Tags/HMI_Tags.sdf`
7. 根据 `HMI/Screens/HMI_ScreenGuide.md` 创建画面
8. 下载到设备或启动仿真运行

详细导入说明请参考 [import_guide.md](import_guide.md)

---

## 📊 I/O 分配概要

### 数字量输入 (I)
| 地址 | 信号名称 | 说明 |
|------|----------|------|
| I0.0 | BTN_1YUAN | 1元投币按钮 |
| I0.1 | BTN_2YUAN | 2元投币按钮 |
| I0.2 | BTN_5YUAN | 5元投币按钮 |
| I0.3 | BTN_10YUAN | 10元投币按钮 |
| I0.4 | BTN_COLA | 可乐选择按钮 |
| I0.5 | BTN_JUICE | 果汁选择按钮 |
| I0.6 | BTN_TEA | 奶茶选择按钮 |
| I0.7 | BTN_WATER | 矿泉水选择按钮 |
| I1.0 | BTN_COFFEE | 咖啡选择按钮 |
| I1.1 | BTN_GTEA | 茶选择按钮 |
| I1.2 | BTN_CANCEL | 取消/退款按钮 |
| I1.3 | BTN_COMBO | 套餐购买按钮 |
| I1.4 | BTN_CONFIRM | 确认出货按钮 |

### 数字量输出 (Q)
| 圀址 | 信号名称 | 说明 |
|------|----------|------|
| Q0.0 | MOTOR_COLA | 可乐出货电机 |
| Q0.1 | MOTOR_JUICE | 果汁出货电机 |
| Q0.2 | MOTOR_TEA | 奶茶出货电机 |
| Q0.3 | MOTOR_WATER | 矿泉水出货电机 |
| Q0.4 | MOTOR_COFFEE | 咖啡出货电机 |
| Q0.5 | MOTOR_GTEA | 茶出货电机 |
| Q0.6 | MOTOR_CHANGE | 找零电机 |
| Q0.7 | LAMP_READY | 就绪指示灯 |

---

## 📝 作者
毕业设计项目

## 📄 License
MIT
