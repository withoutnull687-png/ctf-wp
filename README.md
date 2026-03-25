# ctf-wp

> 这个仓库专门用来存放 CTF 比赛的 Writeup（WP）

## 目录结构

```
ctf-wp/
├── pwn/          # 二进制漏洞利用
├── web/          # Web 安全
├── crypto/       # 密码学
├── reverse/      # 逆向工程
├── misc/         # 杂项
└── forensics/    # 取证分析
```

每个分类下按比赛名称建立子目录，例如：

```
web/
└── 2024-SomeCTF/
    └── challenge-name/
        ├── README.md   # Writeup 正文
        └── ...         # 相关附件或脚本
```

## Writeup 模板

每道题目的 `README.md` 建议包含以下内容：

- **比赛名称 / 题目名称**
- **题目描述**
- **解题思路**
- **关键步骤 / Payload / 脚本**
- **Flag**
