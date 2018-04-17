# Git Commit Message Rules

## `GCM001` 首行作为「总结行」，简明扼要的总结提交内容

无论是 Kernel 领袖 Torvalds，还是 Vim 骨灰级玩家 [Tim Pope](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)，都提出了这一点要求。其中，Tim Pope 建议，该行信息长度应该控制在50个字符以内。

即使 Torvalds 并没有限制该行信息的长度，但是我们还是要对该长度进行控制。在`GCM005`中列举了若干需要用到总结行的命令和工具。在这些命令或工具中，如果「总结行」长度过长，那么显示效果和阅读体验将会大打折扣。

## `GCM002` 「总结行」行文使用「祈使句」

如果某次提交，主要是用于修复某个 Bug，那么使用英文写日志， 就应该表述为 「Fix Bug」，而不是 「Fixing Bug」或「Fixed Bug」。如果使用中文，则应写作「修复 Bug」，避免写作「修复了 Bug」或者 「已修复 Bug」等形式。

统一使用祈使句，一方面，可以统一形式；另外祈使句的 Git 日志，也能够与 `git merge` 和 `get revert` 自动生成的 Git 日志的形式相吻合。

在 GitLab 中，「总结行」中使用`#XXX`来引用 Issue。其中`XXX`就是 Issue ID。如果这个提交和这个 Issue 是位于同一个项目中的，那么 GitLab 就会自动为你创建一个指向 Issue 的链接。

在GitHub中，你甚至可以在「总结行」中，[使用关键词关闭一个Issue](https://help.github.com/articles/closing-issues-using-keywords/)。例如「总结行」中包含「Fix #123」，那么这个提交一旦被合并至主分支，Issue123 将会被 Close。

## `GCM003` 不在「总结行」结尾使用标点符号

「总结行」的作用，类似于标题。因此，不需要在总结行末尾添加标点符号。                                                                                          

## `GCM004` 使用「正文」来描述提交细节信息

提交的「正文」，应当回答以下几个问题：

1. `WHY` 这次提交是为了解决什么样的问题？

* 项目管理者，可能需要通过这个问题的回答，来判断究竟是否要将本次提交合并至 master 分支；
* 执行 Code Review 时，Reviewers 可以通过这个问题的回答，更加迅速地了解本次提交的内容，以及剔除不相关的提交内容；
* 当我们为了快速排查一次引入问题的提交时，这个信息也能够为我们提供很多有价值的参考。

2. `HOW` 这次提交如何解决刚才所提到的问题？

在更高的层面上，描述一下我们为了解决所提到的问题，采取了哪些策略和算法。例如，「通过在XXX过程中引入一个XXX对应于XXX关系的哈希表，提高对XXX的索引性能，进一步解决XXX方面执行效率的瓶颈。」就是一个很不错的描述。

当然，如果你所采取的做法非常明显，比如这次提交仅仅是为了修正不规范的代码风格，或者修正一个拼写错误，你大可省略这一段的描述。

当然，如果目前的做法有哪些地方不太妥当，或者只是临时的解决办法（Walk Across Solution），需要后续更正，这些内容也可以在本段中进行描述说明。

3. `OTHERS` 这次提交还包含其他哪些修改？

提交日志是否需要包含这段内容，取决于你的团队对单次提交所包含的修改内容多少的容忍程度。一般来讲，Git 鼓励我们只做「原子提交」，即每次提交仅仅只涉及一个内容，只解决一个问题。如果单次提交确实涉及到其他的修改项，可以在本段中列出。但是本着 「原子提交」的原则，其他的修改项不宜过多，尽量控制在 2 个以内。

此外，「正文」在行文中，可以使用列表的形式对所阐述的信息分条阐述。列表标记可以使用 `*` 或 `-` 符号。这一点和 `Markdown` 格式非常类似。

## `GCM005` 「总结行」与「正文」之间使用「空行分隔」

「空行分隔」用于区别「总结行」与「正文」部分，某些命令或工具，都会根据「空行分隔」对提交日志进行解析：

* `git format-patch`和`git send-email`命令中，「总结行」将作为邮件标题使用，「正文」将作为邮件正文使用；
* `git log --oneline`，`git shortlog`命令将使用「总结行」作为提交简要描述，如果没有「空行分隔」，提交简要描述中，将会包含「正文」部分；
* 使用`git rebase -i`后，自动启动的编辑器中，会使用「总结行」作为每个提交的简要描述；
* 如果 Git 设置了`merge.summary`选项，那么当执行`merge`操作时，Git 将会汇总来自所有被 merge 的提交的「总结行」，作为本次 merge 提交的「总结行」；
* `gitk`工具有专门的一栏用于显示「总结行」；
* `GitLab`和`GitHub`在他们的用户界面总，都专门为「总结行」做了显示设计；

## `GCM006` 「正文」中的不同段落使用空行分隔

该原则与 `Markdown` 的段落分隔方法相似。

## `GCM007` 「正文」每一行的长度控制在 72 个字符以内

在 Git 的设计思路中，提交日志的排版，换行等工作，需要日志的编写者来负责处理的。因为只有日志提交者，才知道究竟哪些地方需要换行，哪些地方不能换行。[Torvalds对此也有解释](https://github.com/torvalds/linux/pull/17#issuecomment-5660604)：
* `git log`命令在显示提交日志时，并不负责对日志进行分页换行处理，所以如何断行机比较美观，需要日志编写者来考虑处理；
* 某些信息，例如编译器的编译输出等，是不能包含换行的。

**为何日志的长度需要限制在 72 个字符呢？**

1. 默认情况下，`git log`命令会调用`less -S`对提交日志进行分页显示。因此，对于常用的宽度为80字符的终端来讲，如果你的日志中一行的长度超过80，那么长度超过80的的部分将会显示在终端之外，阅读起来将会很不方便。在宽度为80字符的终端中，为了更好的显示日志内容，80个字符减去左边可能会存在的4字符缩进，以及右边为了保证左右对称的4字符宽度，就得到了 72 字符的日志长度限制。
2. 使用`git format-patch --stdout`命令时，Git 会将提交转换成多个邮件序列。对于纯文本格式的邮件，为了保证邮件阅读体验，一般要保证在考虑了历史邮件嵌套的情况下，在80个字符的终端中，邮件仍然可以较美观的显示，因此，对于这个需求，72 字符的宽度限制，仍然是一个不错的选择。

## `GCM008` 确保你的提交是「原子提交」

「原子提交」指的是 每个提交**仅包含一个修改**，且**必须包含该修改所有的相关内容**。因此，你不可以：
* 提交中混合与提交目的不相关的提交内容；
* 提交不完整的内容，甚至导致代码不能正常编译的内容；
* 在一个较大的修改提交中「隐藏」一些很小的代码改动。

使用`git add -p`命令，可以帮助你较方便的将你的提交整理成一个个晓得的「原子提交」。

# References

* [Torvalds:Good git commit message](https://github.com/Subsurface-divelog/subsurface/commit/b6590150d68df528efd40c889ba6eea476b39873)
* [Tim Pope. Apr 19, 2008. A Note About Git Commit Messages](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)
* [`gitcommit.vim` By Tim Pope](https://github.com/tpope/vim-git/blob/master/syntax/gitcommit.vim)
* [https://commit.style/](https://commit.style/)
* [GitHub Help:Closing issues using keywords](https://help.github.com/articles/closing-issues-using-keywords/)
* [Caleb Thompson.June 12, 2015. Useful Tips For A Better Commit Message](https://robots.thoughtbot.com/5-useful-tips-for-a-better-commit-message)
