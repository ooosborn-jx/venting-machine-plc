# TIA Portal V18 导入指南

## 自动售货机 PLC 控制系统 - 项目导入步骤

---

## 📋 前提条件

- ✅ 已安装 **TIA Portal V18**
- ✅ 已安装 **PLCSIM V18**（仿真需要）
- ✅ 下载本项目所有文件

---

## 步骤 1：创建新项目

1. 打开 TIA Portal V18
2. 点击 **"Create new project"**
3. 项目名称：`VendingMachine`
4. 选择保存路径
5. 点击 **"Create"**

---

## 步骤 2：添加 PLC 设备

1. 在项目树中点击 **"Add new device"**
2. 选择 **"Controller"** → **"S7-1200"** → **"CPU 1214C DC/DC/DC"**
   - 型号：`6ES7 214-1AG40-0XB0`
3. 设备名称：`PLC_1`
4. 点击 **"OK"**

---

## 步骤 3：导入 PLC 源程序

### 3.1 添加外部源文件

1. 在项目树中展开 PLC → 右键 **"External source files"**
2. 选择 **"Add external source file"**
3. 按以下**顺序**逐个导入 `.scl` 文件：

```
① DB_Global.scl        ← 全局数据块
② DB_Inventory.scl     ← 库存数据块
② DB_Sales.scl         ← 销售数据块
③ FB_Payment.scl       ← 投币功能块
④ FB_ProductSelect.scl ← 商品选择功能块
⑤ FB_Change.scl        ← 找零功能块
⑥ FB_Inventory.scl     ← 库存管理功能块
⑦ FB_Alarm.scl         ← 报警功能块
⑧ FC_Display.scl       ← 显示控制功能
⑨ OB1_Main.scl         ← 主程序（最后导入）
```

> ⚠️ **注意**：必须按顺序导入，先导入被引用的 DB 和 FB。

### 3.2 编译源文件

1. 右键每个源文件 → **"Compile"**
2. 等待编译完成，检查是否有错误
3. 如果有编译错误：
   - 双击错误信息定位到源文件
   - 根据提示修正
   - 重新编译

### 3.3 验证编译结果

编译成功后，在项目树中应该能看到：
- **Program blocks** → OB1, FB1-FB6, FC1, DB1-DB3
- 双击各块可以查看梯形图/结构化文本

---

## 步骤 4：添加 HMI 设备

1. 点击 **"Add new device"**
2. 选择 **"HMI"** → **"Basic Panels"** → **"KTP700 Basic"**
   - 型号：`6AV2 123-2GB03-0AX0`
   - （如果没有 KTP700，选择任何 Basic 面板均可）
3. 设备名称：`HMI_1`
4. 在弹出的向导中：
   - 选择 **"HMI device and PLC are connected"**
   - 选择 PLC 设备
5. 点击 **"OK"**

---

## 步骤 5：导入 HMI 变量

1. 在 HMI 项目树中展开 **"HMI tags"**
2. 右键 → **"Import"**
3. 选择 `HMI/Tags/HMI_Tags.sdf` 文件
4. 确认导入

> 如果导入失败，可以手动创建变量表，参考 `HMI_Tags.sdf` 中的变量名和地址。

---

## 步骤 6：创建 HMI 画面

参照 `HMI/Screens/HMI_ScreenGuide.md` 创建以下画面：

1. **MainScreen** — 主操作画面
2. **AdminLogin** — 管理员登录
3. **AdminScreen** — 管理后台（库存管理）
4. **SalesScreen** — 销售统计
5. **PriceScreen** — 价格设置
6. **AlarmScreen** — 报警画面

### 画面切换关系：
```
MainScreen ←→ AdminLogin → AdminScreen → SalesScreen
                              ↓
                         PriceScreen
                              
MainScreen → AlarmScreen (报警时自动弹出)
```

---

## 步骤 7：编译与下载

### 7.1 全部编译
1. 菜单 **"Build"** → **"Rebuild all"**
2. 等待 PLC 和 HMI 都编译完成

### 7.2 仿真运行（无硬件）
1. 点击工具栏 **"Start simulation"** 按钮（绿色三角）
2. 选择 PLCSIM 仿真器
3. PLC 程序将下载到仿真器
4. 切换到 HMI 项目 → 点击 **"Start simulation"** 启动 HMI 仿真
5. 在仿真界面中测试各功能

### 7.3 下载到实际硬件
1. 用网线连接 PLC 和电脑
2. 设置电脑 IP 与 PLC 同网段
3. 右键 PLC 设备 → **"Download to device"** → **"Hardware and software"**
4. 同样下载 HMI 程序

---

## 步骤 8：功能测试清单

在仿真器中逐一测试以下功能：

- [ ] 投币：1元、2元、5元、10元，金额正确累加
- [ ] 超过20元：提示"金额已满"，不再接受投币
- [ ] 选择商品：6种商品按钮响应正常
- [ ] 余额不足：选择价格高于余额的商品时提示
- [ ] 正常购买：确认后出货，2秒脉冲，余额扣款
- [ ] 找零：购买后找零金额正确（优先大面额）
- [ ] 取消退款：按取消按钮退回全部金额
- [ ] 售罄：库存为0时商品按钮无效，显示"售罄"
- [ ] 低库存：库存≤2时商品闪烁提示
- [ ] 套餐购买：可乐+矿泉水套餐价7元
- [ ] 状态显示：各状态文字正确显示
- [ ] 管理员：登录后查看销售统计和库存

---

## ❓ 常见问题

### Q: 编译报错 "Identifier not found"
**A**: 检查导入顺序，确保 DB 先于 FB 导入，FB 先于 OB1 导入。

### Q: HMI 画面显示 "???"
**A**: 检查变量连接是否正确，确认 PLC 和 HMI 已建立通信连接。

### Q: 仿真器启动失败
**A**: 确保安装了 PLCSIM V18，且 Windows 防火墙未阻止。

### Q: 按钮无响应
**A**: 检查边沿检测逻辑，确认按钮信号能正确到达 PLC 输入端。

---

## 📞 技术支持

如遇到问题，请检查：
1. TIA Portal 版本是否为 V18
2. CPU 型号是否匹配
3. 源文件编译是否有错误
4. 变量地址是否一致
