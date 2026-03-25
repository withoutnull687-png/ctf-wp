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

输入非法命令时（希望你没有尝试一键速通哦）会返回随机提示：

- 错误的指令desuwa
- 你这家伙在说什么呢？
- 不存在的指令，zako~
然后题目会断开连接来惩罚你！

---

### 2. 输入长度限制

如果输入过长则会提示：

```text
你不乘哦
```

并断开连接。

---

## 🪜 解题步骤

请不断探索目录结构以确认乐队成员

```
最终可以通过以下步骤来到正确的目录下：
ls
cd Millsage
ls
cd SETSUNA
---

## 🎯 获取 FLAG

进入目标目录后输入 cat flag即可获得：

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

# 🥈 第二题 WP（小祥 追逐战）

# 🥈 小祥 追逐战 - Writeup

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

题目会给出类似例子：

```text
buf: 0x40
rbp: 0x8
```

## 🪜 解题步骤

### Step 1：QTE

前四轮：

```
15s内输入 F（f） 即可
```

最后一轮：

```
15s内输入 E（e） 即可
```

### Step 2：输入偏移

```
第一轮：
  buf       @ rbp-0x40
  hit       @ rbp-0x8
  saved rbp @ rbp+0x0
  ret       @ rbp+0x8
所以结果就是 0x40-0x8 = 56（十进制表示结果）
第二轮同理：
  buf       @ rbp-0x50
  hit       @ rbp-0x10
  saved rbp @ rbp+0x0
  ret       @ rbp+0x8
所以结果就是 0x50-0x10 = 64
第三轮：
  buf       @ rbp-0x38
  hit       @ rbp-0x4
  saved rbp @ rbp+0x0
  ret       @ rbp+0x8
结果为 0x38-0x4 = 52
第四轮
  buf       @ rbp-0x48
  hit       @ rbp-0xc
  saved rbp @ rbp+0x0
  ret       @ rbp+0x8
结果为 0x48-0xc = 60

```

---

## ❌ 失败情况

以下任一情况都会失败：

- QTE未完成（按错或者15s内未完成）
- 偏移值计算错误
- 每一轮作答超时（超过90s没有完成一轮作答）

---

## 🏁 成功条件

连续4轮全部正确且完成QTE即可获取flag：

```
flag{xxxxxxxxxxxxxxxxxxxx}
```

---

## 💡 总结

本题本质其实是栈偏移的计算（16进制）：

```
偏移量 = hit - buf
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

```
比如本题提供的附件，就可以对其使用ROPgadget
```

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

```
$ ROPgadget --binary chall | grep "pop rdi ; ret"
0x000000000040120a : cli ; pop rdi ; ret
0x0000000000401207 : endbr64 ; pop rdi ; ret
0x000000000040120b : pop rdi ; ret

```

```
要找的gadget即为0x401207
```

### 🌸 第二阶段

```
$ ROPgadget --binary chall | grep "pop rsi ; ret"
0x0000000000401213 : cli ; pop rsi ; ret
0x0000000000401210 : endbr64 ; pop rsi ; ret
0x000000000040120d : nop ; ud2 ; endbr64 ; pop rsi ; ret
0x0000000000401214 : pop rsi ; ret
0x000000000040120e : ud2 ; endbr64 ; pop rsi ; ret

要找的gadget即为0x401210

```

### 🌸 第三、四阶段

```
$ strings -tx chall | grep "white_lily"
   2008 white_lily


$ nm -n chall | grep "visit_ward"
0000000000401221 T visit_ward

要找的gadget为0x2008和0x401221

```

### 🌸 第五阶段

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

- **条件**：中途出错/作答时间过长
- **结果**：无法看到遗言

### 🔵 结局2：你从未离去

- **条件**：全部正确
- **结果**：解锁完整剧情 + 未发送出去的邮件

---

## 🎁 最终 FLAG

```
flag{WHITE_LILLY_xxxxxxxx}
```

---

## 🎁 彩蛋

获取flag45秒后未关闭终端将会触发出题人设置的彩蛋：

```
嗯？
你还没有走吗？
......
```
