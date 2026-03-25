# 🥇 第一题 WP（Bandori Shell）

# 🥇 Bandori Shell - Writeup

## 📌 题目考点

- Linux 基础命令
- 目录切换
- 输入限制处理

---

## 🧠 解题思路

本题是一个模拟 Shell，需要通过输入命令寻找正确路径获取 FLAG。

支持的命令包括：

- `ls`
- `cd xxx`
- `cd ..`

---

## ⚠️ 特殊机制

### 1. 错误命令反馈

输入非法命令时会返回随机提示：

- 错误的指令desuwa
- 你这家伙在说什么呢？
- 不存在的指令，zako~

---

### 2. 输入长度限制

如果输入过长：

```text
你不乘哦
```

并断开连接。

---

## 🪜 解题步骤

不断尝试目录结构：

```
ls
cd xxx
ls
cd ..
```

通过反复探索找到正确路径。

---

## 🎯 获取 FLAG

进入目标目录后即可获得：

```
flag{millsage_intruder_xxxxxxxx}
```

---

## 💡 总结

本题核心是：

- 模拟终端环境
- 限制输入行为
- 防止暴力脚本

---

# 🥈 第二题 WP（MyGO 追逐战）

# 🥈 MyGO 追逐战 - Writeup

## 📌 题目考点

- 栈结构基础
- 偏移量计算

---

## 🎮 玩法说明

每一轮包含两部分：

1. QTE（按键）
2. 输入偏移量

---

## ⚙️ 栈结构示例

题目会给出类似：

```text
buf: 0x40
rbp: 0x8
```

---

## 🧠 解题核心

计算偏移量：

```
偏移 = buf + rbp
```

例如：

```
0x40 + 0x8 = 0x48
```

---

## 🪜 解题步骤

### Step 1：QTE

输入：

```
F
```

最后一轮：

```
E
```

### Step 2：输入偏移

```
0x48
```

---

## ❌ 失败情况

以下任一情况都会失败：

- QTE未完成
- 偏移错误
- 超时

---

## 🏁 成功条件

连续4轮全部正确：

```
flag{uuid-xxxx-xxxx}
```

---

## 💡 总结

本题本质是：

```
偏移 = buf + rbp
```

属于 PWN 入门题。

---

# 🥉 第三题 WP（第六片花瓣 - ROPgadget）

# 🥉 第六片花瓣 - Writeup

## 📌 题目考点

- ROPgadget 使用
- 函数地址查找
- x64 调用约定基础

---

## 📚 ROPgadget 简介

ROPgadget 用于查找二进制中的 gadget：

```bash
ROPgadget --binary chall
```

---

## 📚 常用命令

### 查找函数地址

```bash
nm -n chall | grep win
```

### 查找 pop rdi

```bash
ROPgadget --binary chall | grep "pop rdi"
```

---

## 🎯 本题核心

本题不需要构造 payload，只需要：

> 找到正确地址并输入

---

## 🪜 解题步骤

### 🌸 第一阶段

```bash
nm -n chall | grep win
```

输出示例：

```
0x4011d6
```

### 🌸 第二阶段

```bash
ROPgadget --binary chall | grep "pop rdi ; ret"
```

选择最干净的一条：

```
0x40120b
```

（避免带 `cli` / `endbr64`）

### 🌸 第三阶段

```bash
ROPgadget --binary chall | grep "ret"
```

### 🌸 第四阶段

按照题目提示完成组合逻辑。

---

## ❌ 错误处理

如果输入错误，仍然会给出讲解：

```
ROP链：
pop rdi → 参数
win → 调用函数

原因：
x64 参数通过 rdi 传递
```

---

## 🎬 剧情结局

### 🟢 结局1：未尽之语

- **条件**：中途出错
- **结果**：无法看到遗言

### 🔵 结局2：你从未离去

- **条件**：全部正确
- **结果**：解锁完整剧情 + 邮件

---

## 🎁 最终 FLAG

```
flag{WHITE_LILLY_xxxxxxxx}
```

---

## 🎁 彩蛋

45秒后触发：

```
嗯？
你还没有走吗？
```
