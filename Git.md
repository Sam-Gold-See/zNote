# Git
[Git Learning Branch](https://learngitbranching.js.org/)

## Git Commit

Git 仓库中的提交记录保存的是目录下所有文件的快照，会将当前版本与仓库中的**上一个版本进行对比**，并把**所有的差异**打包到一起作为一个提交记录。

Git 还保存了**提交的历史记录**，大多数提交记录的上面都有 parent 节点的原因。

使用 `git commit` 指令，将当前文件版本，以上一个文件版本为 parent 节点进行保存

## Git Branch

生成 Git Branch 分支，基于这个提交以及它所有的 parent 节点提交进行新的工作。

使用 `git branch <branch-name>` 来创建一个新的分支，但是此时进行 `git commit` 操作仍然是对于原分支进行操作

使用 `git checkout <branch-name>` 来切换到自己想要的新分支上，再使用 `git commit` 时，此时的操作对象为新分支

在 Git 2.23 版本之后也可以使用 `git switch <branch-name>` 来切换

使用 `git checkout -b <branch-name>` 可以直接创建新分支后切换到该分支上面进行操作

## 分支与合并

为了将两个分支合并到一起，可以使用 `git merge <branch-name>` ，该操作会产生一个特殊的提交记录，有两个 parent 节点的一个提交记录

第二个合并分支的方法是 `git rebase` ，对当前分支取出一系列的提交记录后进行“复制”，最后再逐一放下去，形成一条线性结构

## 在提交树上移动

**HEAD** 默认总是指向当前分支上最近一次提交记录

使用 `git chekcout <option>` 使得 **HEAD** 指向一次具体的操作记录

## 相对引用

使用 `<option>^` 向上移动1个提交记录，或者使用 `~<num>` 移动多个提交记录；或者使用 **HEAD** 作为参照物，反复使用 `HEAD^` 重复向上移动

## `~` 操作符

使用 `~` 操作符，后跟一个数字（也可以不跟，此时等价于 `^` ），指定向上移动多少次

使用 `-f` 选项让分支指向另一个提交， `git branch -f <branch-name> HEAD~n` 使得 `<branch-name>` 分支强制指向 **HEAD** 的第n级 parent 提交

## 撤销变更

主要用两种方法来撤销变更，一是 `git reset` ，还有就是 `git revert`

在本地情况下，使用 `git reset HEAD~n` 将分支记录回退n个提交记录来实现撤销，本地代码库将无法找到回退之后的提交记录

在远程环境下，前者就失效了，需要使用 `git revert HEAD~n` ，此时并非删除n个提交记录，而是生成一个新提交记录，该提交记录等价于n个提交记录之前的提交记录