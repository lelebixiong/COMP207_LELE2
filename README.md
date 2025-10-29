# COMP207_LELE2 📚  
**数据库事务 / 并发控制 / 日志恢复 / 锁机制 复习笔记**

<p align="center">
  <img src="https://img.shields.io/badge/type-study%20notes-lightgrey.svg" />
  <img src="https://img.shields.io/badge/topic-database--transactions-blue.svg" />
  <img src="https://img.shields.io/badge/license-GPL--3.0-orange.svg" />
</p>

---

## 🧠 这个仓库是什么 / What is this?

这是我整理的一份数据库系统期末 / 考试复习笔记（主要是事务管理、锁、2PL、隔离级别、崩溃恢复等主题）。  
内容是“按我能听懂的方式”去解释课堂上最容易扣分的点，并配了一些例子、口语化比喻和练习题。

> 用法：当你复习到一半，脑子糊成一锅粥的时候，就翻这里当速查表。

**关键词**：ACID、Shared / Exclusive Lock、Update Lock、Intention Lock、Two-Phase Locking (2PL)、死锁、Undo / Redo Logging、Crash Recovery、Isolation Levels。

---

## 🌳 目录 / Table of Contents
- [这个仓库是什么 / What is this?](#-这个仓库是什么--what-is-this)
- [特性 / Features](#-特性--features)
- [文件结构 / Repo Structure](#-文件结构--repo-structure)
- [快速开始 / Quick Start](#-快速开始--quick-start)
- [学习建议 / Study Tips](#-学习建议--study-tips)
- [术语速查 / Cheat Sheet](#-术语速查--cheat-sheet)
- [许可证 / License](#-许可证--license)
- [免责声明 / Disclaimer](#-免责声明--disclaimer)

---

## ✨ 特性 / Features

- **中英双语+白话解释**  
  概念都尽量用“人话”解释，比如更新锁（Update Lock）会用社交媒体的类比来讲为什么它能防止死锁。

- **从概念到考试思路**  
  不只是定义，还有“考场怎么写才能拿分”的角度，比如：怎么解释 2PL 为什么能保证冲突可串行化；怎么给一个 schedule 标上锁并判断是不是死锁。

- **图示 / 小故事 / 场景化**  
  不是一堆教科书式句子，会穿插场景描述（例如“两个事务互相卡住谁也动不了”去解释死锁）。

- **练习题 / 常见坑**  
  有些文件里是直接给“老师常见提问 → 标准思路”的形式，适合自测。

---

## 🗂 文件结构 / Repo Structure

```text
COMP207_LELE2/
├─ 五：LECTURE8.md
│    ├─ ACID 事务特性回顾
│    ├─ Basic Locks / Shared Lock / Exclusive Lock
│    ├─ Update Lock（更新锁）和为什么它能防止饥饿
│    ├─ 多粒度锁（表锁 vs 行锁）和意向锁 (Intention Locks)
│    └─ Two-Phase Locking (2PL) 以及为什么它让调度变成“冲突可串行化”
│
├─ 六：锁（Locks）、日志与恢复（Logging & Recovery）、以及调度与回滚（Schedules & Rollback）.md
│    ├─ 给事务加锁的练习（S-lock / X-lock / unlock）
│    ├─ 如何制造死锁 & 如何避免死锁（统一加锁顺序）
│    ├─ Undo / Redo Logging 的直观流程
│    └─ 系统崩溃之后如何恢复（谁需要撤销 / 谁需要重做）
│
├─ 七：.md
│    ├─ 四种隔离级别：Read Uncommitted / Read Committed /
│    │                 Repeatable Read / Serializable
│    ├─ 哪些现象被允许，哪些是禁止的（脏读 / 不可重复读 / 幻读）
│    └─ 锁类型速查（S / X / Update / Intention）
│
├─ LICENSE
│    └─ GPL-3.0 License
│
└─ README.md
     └─ 你正在看的这个文件
```


##🔖  术语速查 / Cheat Sheet

- **ACID**
原子性 / 一致性 / 隔离性 / 持久性；描述“事务有多靠谱”

- **Shared Lock (S)**
共享锁	只能读，很多事务可以一起拿

- **Exclusive Lock (X)**
排他锁	读+写都行，但同一时间只能有一个事务拿

- **Update Lock**
更新锁	“我先读，但我可能要写”，用来避免两个事务互相卡死

- **Intention Lock** 意向锁
告诉系统“我要在更细粒度地方（比如某一行）上锁”，方便表级/行级同时管理

- **2PL (Two-Phase Locking)**
事务先疯狂加锁→到某个点之后只能解锁不能再加锁；保证调度冲突可串行化

- **死锁 Deadlock**
T1 等 T2 的锁，T2 也在等 T1 的锁，互相卡住谁也走不了

- **Isolation Level** 隔离级别
  数据库允许的并发“干扰”程度，比如能不能看到别人未提交的数据

- **Undo / Redo Logging**
崩溃后要不要回滚没提交的事务（Undo）/ 重新应用已提交的更新（Redo）
