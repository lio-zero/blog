# shell 大括号扩展 — {}

`{}` 将对大括号中的文件名做扩展。

使用示例：

```bash
$ echo {a..z}
# a b c d e f g h i j k l m n o p q r s t u v w x y z

$ mv /path/{file,renamed}.txt
# 等同于
$ mv /path/file.txt /path/renamed.txt
```

## 使用 shell 大括号创建递归目录

- `mkdir` 的 `-p` 标志和 `{}`。
- `-p` 指示 `mkdir` 确保目录名称存在，不存在的就建一个。

```bash
$ mkdir -p new-dir/{foo,baz}/whatever-{1,2}/{a,b}

# new-dir
# ├── baz
# │  ├── whatever-1
# │  │  ├── a
# │  │  └── b
# │  └── whatever-2
# │     ├── a
# │     └── b
# └── foo
#    ├── whatever-1
#    │  ├── a
#    │  └── b
#    └── whatever-2
#       ├── a
#       └── b
```

## 创建包含索引的文件

```bash
$ touch {1..3}.txt
# 等同于
$ touch 1.txt 2.txt 3.txt
```

## 更多资料

[shell 中各种括号的作用()、(())、[]、[[]]、{}](https://www.runoob.com/w3cnote/linux-shell-brackets-features.html)
