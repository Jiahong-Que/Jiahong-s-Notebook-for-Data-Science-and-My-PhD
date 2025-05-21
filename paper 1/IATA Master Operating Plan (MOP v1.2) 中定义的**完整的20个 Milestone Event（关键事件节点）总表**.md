当然可以，下面是 IATA Master Operating Plan (MOP v1.2) 中定义的**完整的20个 Milestone Event（关键事件节点）总表**，是贯穿整个航空货运生命周期的数据和流程基准点。你可以将它用于论文中方法对齐、传感器识别标签设计、KPI 分析等多个部分。

---

## 📌 **IATA MOP 20个标准 Milestone Events 总表**

| No. | Milestone Event 名称          | 中文解释     | 触发时机            | 备注/相关数据              |
| --- | --------------------------- | -------- | --------------- | -------------------- |
| 1   | **Planned**                 | 已规划运输需求  | 货主/货代提出运输计划     | 产品种类、服务等级、时间要求等      |
| 2   | **Booking Confirmed**       | 订舱已确认    | 航空公司确认运力并发回确认   | 与 Booking ID 绑定      |
| 3   | **Received from Shipper**   | 从发货人收货   | 地面代理从货主或货代处接收货物 | 可由称重、静止等状态触发         |
| 4   | **Ready for Carriage**      | 可运输状态确认  | 所有单证、安检、打板等流程完成 | 通常发生在货站中             |
| 5   | **Tendered**                | 提交给航空公司  | GHA 将货物正式提交航空公司 | 多用于 BUP 货物或特货        |
| 6   | **Received from Forwarder** | 从货代收货    | 航空公司从货代或集运人接收   | 与 #3 区分角色不同          |
| 7   | **Accepted**                | 接收完成     | 航空公司正式接收货物      | e-AWB 签字或系统确认时间点     |
| 8   | **Document Received**       | 收到运单和单证  | 收到 e-AWB 或纸质文件  | 与 Acceptance 紧密相关    |
| 9   | **Ready for Uplift**        | 准备装机     | 配载完成、ULD 准备好    | 可由智能设备检测装载信号触发       |
| 10  | **Manifested**              | 已列入舱单    | 在舱单中注册该货物       | 运往系统传输给海关和机场         |
| 11  | **Uplifted**                | 已起飞      | 货物随航班起飞         | 可结合 ADS-B 或传感器判断     |
| 12  | **Departed from Origin**    | 起飞离站     | 原机场出发           | 与航班信息绑定              |
| 13  | **Arrived at Transit**      | 到达中转站    | 多段运输中落地中转机场     | 适用于 HUB 枢纽中转         |
| 14  | **Departed from Transit**   | 从中转站起飞   | 继续飞往下一站         | 多段转运需要               |
| 15  | **Arrived at Destination**  | 到达目的站机场  | 最后一段航班落地        | 可对接 Smart Pouch 到达事件 |
| 16  | **Received from Carrier**   | 收到航空公司交付 | GHA 接收落地货物      | 多用于 Destination GHA  |
| 17  | **Customs Cleared**         | 清关完成     | 海关放行许可          | 与国家法规相关              |
| 18  | **Available for Delivery**  | 可提货状态    | 完成所有流程，可通知收货人   | 通知提货时间点              |
| 19  | **Delivered to Consignee**  | 交付收货人    | 货物交还最终收件人       | 最终责任交接节点             |
| 20  | **Proof of Delivery (POD)** | 提货签收证明   | 收货人签字或扫描确认      | 通常在 DCS 系统中产生记录      |

---

## 🧩 示例：如何与 Smart Pouch 状态对齐

| Smart Pouch 状态标签              | 对应 MOP 事件编号 | MOP 事件名称                                       |
| ----------------------------- | ----------- | ---------------------------------------------- |
| `scale_state`                 | #3          | Received from Shipper                          |
| `driving_around_state`        | #4, #5      | Ready for Carriage / Tendered                  |
| `remove_ground_storage_state` | #6          | Received from Forwarder                        |
| `loading_state`               | #9, #10     | Ready for Uplift / Manifested                  |
| `airborne_state`              | #11         | Uplifted                                       |
| `arrival_detected`            | #15, #16    | Arrived at Destination / Received from Carrier |
| `handover_state`              | #19         | Delivered to Consignee                         |

---

如果你希望，我还可以为你：

1. 制作一张 **甘特图/流程图** 展示这20个事件的时间顺序；
2. 将该表格转换为 LaTeX 表格形式用于论文；
3. 提供每个 milestone 在实际 XML/ONE Record 数据接口中的结构说明（用于系统开发或数据匹配）；
4. 给出实际货运流程中可能“跳过”或“合并”的节点组合分析。

你是否需要我进行其中某一项扩展？例如绘制整体流程图？
