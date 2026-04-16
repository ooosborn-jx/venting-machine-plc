# I/O 地址分配表

## 自动售货机 PLC 控制系统 - TIA Portal V18

---

## 数字量输入 (Digital Input)

| 地址 | 变量名 | 信号名称 | 说明 | 类型 |
|------|--------|----------|------|------|
| I0.0 | BTN_1YUAN | 1元投币按钮 | 常开触点，按下=1 | Bool |
| I0.1 | BTN_2YUAN | 2元投币按钮 | 常开触点，按下=1 | Bool |
| I0.2 | BTN_5YUAN | 5元投币按钮 | 常开触点，按下=1 | Bool |
| I0.3 | BTN_10YUAN | 10元投币按钮 | 常开触点，按下=1 | Bool |
| I0.4 | BTN_COLA | 可乐选择按钮 | 常开触点，按下=1 | Bool |
| I0.5 | BTN_JUICE | 果汁选择按钮 | 常开触点，按下=1 | Bool |
| I0.6 | BTN_MILKTEA | 奶茶选择按钮 | 常开触点，按下=1 | Bool |
| I0.7 | BTN_WATER | 矿泉水选择按钮 | 常开触点，按下=1 | Bool |
| I1.0 | BTN_COFFEE | 咖啡选择按钮 | 常开触点，按下=1 | Bool |
| I1.1 | BTN_TEA | 茶选择按钮 | 常开触点，按下=1 | Bool |
| I1.2 | BTN_CANCEL | 取消/退款按钮 | 常开触点，按下=1 | Bool |
| I1.3 | BTN_COMBO | 套餐购买按钮 | 常开触点，按下=1 | Bool |
| I1.4 | BTN_CONFIRM | 确认出货按钮 | 常开触点，按下=1 | Bool |

---

## 数字量输出 (Digital Output)

| 地址 | 变量名 | 信号名称 | 说明 | 类型 |
|------|--------|----------|------|------|
| Q0.0 | MOTOR_COLA | 可乐出货电机 | 脉冲输出2秒, 24V/1A | Bool |
| Q0.1 | MOTOR_JUICE | 果汁出货电机 | 脉冲输出2秒, 24V/1A | Bool |
| Q0.2 | MOTOR_MILKTEA | 奶茶出货电机 | 脉冲输出2秒, 24V/1A | Bool |
| Q0.3 | MOTOR_WATER | 矿泉水出货电机 | 脉冲输出2秒, 24V/1A | Bool |
| Q0.4 | MOTOR_COFFEE | 咖啡出货电机 | 脉冲输出2秒, 24V/1A | Bool |
| Q0.5 | MOTOR_TEA | 茶出货电机 | 脉冲输出2秒, 24V/1A | Bool |
| Q0.6 | MOTOR_CHANGE | 找零电机 | 脉冲输出2秒, 24V/1A | Bool |
| Q0.7 | LAMP_READY | 就绪指示灯 | 常亮=待机, 闪烁=工作中 | Bool |

---

## 标志位 (Marker / M区)

| 地址 | 变量名 | 说明 |
|------|--------|------|
| M0.0 | AlarmActive | 报警激活标志 |
| M2.0 | AlarmText | 报警文字 (String) |
| M10.0 | HMI_Btn1Yuan | HMI 1元投币虚拟按钮 |
| M10.1 | HMI_Btn2Yuan | HMI 2元投币虚拟按钮 |
| M10.2 | HMI_Btn5Yuan | HMI 5元投币虚拟按钮 |
| M10.3 | HMI_Btn10Yuan | HMI 10元投币虚拟按钮 |
| M10.4 | HMI_BtnCancel | HMI 取消虚拟按钮 |
| M10.5 | HMI_BtnRestock | HMI 一键补货按钮 |

---

## 数据块 (Data Block)

| DB编号 | 名称 | 说明 | 大小(估算) |
|--------|------|------|-----------|
| DB1 | DB_Global | 全局状态与控制变量 | ~200 bytes |
| DB2 | DB_Inventory | 库存数据块 | ~500 bytes |
| DB3 | DB_Sales | 销售记录数据块 | ~8 KB |

---

## 定时器使用

| 用途 | 类型 | 设定值 | 说明 |
|------|------|--------|------|
| 出货定时器 | TON | 2秒 | 出货电机脉冲持续时间 |
| 取货等待 | TON | 3秒 | 出货后等待顾客取货 |
| 返回待机 | TON | 2秒 | 交易完成返回待机延时 |
| 闪烁定时器 | TON | 500ms | 报警/低库存闪烁 |
| 投币脉冲 | TON | 200ms | 投币信号脉冲宽度 |

---

## CPU 型号要求

| 参数 | 要求 |
|------|------|
| CPU 型号 | S7-1200 CPU 1214C DC/DC/DC (6ES7 214-1AG40-0XB0) |
| 数字量输入 | ≥ 14 点 (本项目使用 13 点) |
| 数字量输出 | ≥ 10 点 (本项目使用 8 点) |
| 程序存储器 | ≥ 75 KB |
| 工作存储器 | ≥ 4 MB |
| 通信接口 | PROFINET (连接 HMI) |
