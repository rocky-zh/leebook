# 单一职责原则

---

单一职责原则又称单一功能原则（SRP：Single responsibility principle）

## 核心

解耦和增强内聚性（高内聚，低耦合）

## 描述

类被修改的几率很大，因此应该专注于单一的功能。如果你把多个功能放在同一个类中，功能之间就形成了关联，改变其中一个功能，有可能中止另一个功能，这时就需要新一轮的测试来避免可能出现的问题。