# bash 中的历史命令快捷方式

```bash
$ echo "Hello world"
Hello world
```

- `!!` — 最后一个命令

```bash
!!
echo "Hello world"
```

- `!1` — 历史上的第一个条目
- `!-1` — 历史上的最后一个条目
- `!ssh` — 最后一个以 `ssh` 开头的命令

```bash
!1
!-1
!echo
echo "Hello world"
```

- `!:1` — 最后一个命令的第一个参数

```bash
!:1
"Hello world"
```

## 更多资料

[How To Use Bash History Commands and Expansions on a Linux VPS](https://www.digitalocean.com/community/tutorials/how-to-use-bash-history-commands-and-expansions-on-a-linux-vps)
