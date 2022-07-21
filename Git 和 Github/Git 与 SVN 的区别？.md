# Git 与 SVN 的区别？

Git 和 SVN 最大的区别在于 Git 是**分布式**的，而 SVN 是**集中式**的。因此我们不能再离线的情况下使用 SVN。如果服务器出现问题，我们就没有办法使用 SVN 来提交我们的代码。

SVN 中的分支是整个版本库的复制的一份完整目录，而 Git 的分支是指针指向某次提交，因此 Git 的分支创建更加开销更小 并且分支上的变化不会影响到其他人。SVN 的分支变化会影响到所有的人。

SVN 的概念指令相对于 Git 来说要简单一些，比 Git 更容易上手。

详细资料可以参考：

- [Comparing Workflows](https://www.atlassian.com/git/tutorials/comparing-workflows)
- [前端必须知道的 Git 和 SVN 的区别](https://juejin.cn/post/6960836262894764039)
