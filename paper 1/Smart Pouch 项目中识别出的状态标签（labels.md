你这张图展示了 **Smart Pouch 项目中识别出的状态标签（labels）**，其中以 `_state` 结尾的是你训练模型用于分类的动作/状态类别。我们现在的目标是：

> ✅ **将这些 `_state` 标签与你论文中所用的 IATA MOP Milestones 对应起来**

---

## ✅ 对齐思路说明：

我们不是将每一个状态强行对应一个 MOP milestone，而是：

1. **将多个状态归为一类“物理事件”**（如：称重、搬运、装载、运输等）；
2. 然后再映射到 MOP 的一个或多个 milestone。

---

## 🧩 标签分类与对应的 MOP 事件关系表（初步映射）

| Smart Pouch Label (`_state`)                                                           | 动作描述        | 可映射的 MOP Milestone(s)                                   |
| -------------------------------------------------------------------------------------- | ----------- | ------------------------------------------------------- |
| `scale_state`                                                                          | 静止+称重状态     | #3 Received from Shipper                                |
| `driving_around_state`                                                                 | 在场内行驶/运输状态  | #4 Ready for Carriage / #5 Tendered                     |
| `put_dolly_state`, `attach_dollytrain_state`, `detach_dollytrain_state`                | 放上/取下拖车     | #6 Received from Forwarder / 物理交接动作                     |
| `uld_buildup_state`                                                                    | 正在打板        | #4 Ready for Carriage                                   |
| `uld_breakdown_state`                                                                  | 正在拆板        | #16 Received from Carrier                               |
| `remove_ground_storage_state`, `put_ground_storage_state`, `idle_ground_storage_state` | 仓库移出/放入/静置  | 仓储管理流程，可扩展用于 delay 检测，不直接映射 MOP                         |
| `remove_aircraft_storage_state`                                                        | 从飞机上卸货      | #16 Received from Carrier                               |
| `truck_load_state`, `truck_unload_state`, `inside-truck_flo`                           | 上/下卡车、卡车内运输 | #6 Received from Forwarder / #19 Delivered to Consignee |
| `highloader_state`                                                                     | 高装卸机操作中     | #9 Ready for Uplift / #16 Received from Carrier         |
| `in-dolly-train-veh`, `on-dolly_veh`, `pltK_veh`, `forkl_veh`, `man_veh`               | 在某类车上运输     | 通常属于 #5 Tendered 或 #14 Departed from Transit            |
| `idle_on_apron_state`, `asph_flo`, `conc_flo`, `none_flo`                              | 静止、停在不同区域   | 可用于识别异常延迟（non-MOP事件）                                    |
| `other1_state`, `other2_state`, `other3_state`                                         | 暂未归类        | 需手动审查是否为异常动作                                            |

---

## 📌 示例（用于论文或图示）

**状态识别 → 动作类别 → MOP Milestone**

```
[scale_state]      → "Cargo is static on scale"        → MOP #3: Received from Shipper  
[uld_buildup_state]→ "Cargo is being palletized"       → MOP #4: Ready for Carriage  
[truck_load_state] → "Loaded onto truck for handover"  → MOP #6: Received from Forwarder  
[highloader_state] → "ULD being loaded to aircraft"    → MOP #9: Ready for Uplift  
[remove_aircraft_storage_state] → "Unloading from aircraft" → MOP #16: Received from Carrier
```

---

## 📦 应用价值（写入论文）

通过以上映射，你可以在论文中这样写：

> The Smart Pouch device identifies physical cargo handling states (e.g., **`scale_state`, `uld_buildup_state`, `driving_around_state`**) and maps them to **operational milestones defined by the IATA MOP**. This enables a seamless integration of real-time physical tracking into digital milestone-based cargo management.

---

## 下一步建议：

1. ✅ 是否需要我将此表格整理成 **高质量 PDF/LaTeX 表格** 供论文插入？
2. ✅ 是否需要我用图画出这些状态与 MOP 之间的**箭头对齐图示**？

告诉我你想要哪一项，我可以直接生成。
