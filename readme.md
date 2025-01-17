# XFLOPS 2024招新 第一轮考核文档

在正式做题之前，强烈建议先阅读一遍考核文档

## 第一轮考核规则

> 尽管下述规则已在报名指南中给出，但**建议再次阅读并严格遵守下述规则**

1. 不得作弊。考核请个人独立完成，任何多人合作完成题目的行为都将被认为作弊。我们将进行代码查重，希望大家诚信参与考核。
2. 不得使用恶意代码注入等方式对评测进行攻击。本考核不是 CTF，不考察任何形式的攻击。如果我们发现任何攻击行为，将取消本次考核成绩。
3. 不得打表。超算比赛要求保证优化后源码的正确性与普适性。如果我们发现你的代码中基于给出的示例算例打表，将取消本次考核成绩。同时我们在评测时会适当修改示例算例中的部分数据。
4. 请不要再登录节点上运行并行编译或并行计算等高负载程序。交我算平台设有相应检测，如因在登录节点上进行运行高负载程序导致封号后果自负。
5. 可以使用搜索引擎与AI辅助答题，但请确保自己已经充分理解了所提交的每一行代码的含义。
6. 请勿滥用交我算平台计算资源，一经发现，立即封号。


## 试题与成绩计算方法

本次考核一共有五道试题：
> 请注意下述排序仅为首字母排序，不代表难度排序。请自行评估难度并选择合理的解题策略

题目 | 权重 | 所需CPU资源 | 内存限制 |
-----|------|---------|------|
Bithack | 20% | 单机1核心 | 16G |
Clever Clang | 20% | 单机4核心 | 256G |
Cluster | 20% | 单机128核心 | 256G |
HPL | 20% | 单机128核心 | 256G |
N Body Problem | 20% | 单机128核心 | 256G |


每道试题都是100分制，每个试题的计分方式均在题目的`readme.md`中给出，在计算总分和排名的时候会根据每题的权重加权平均。

每道试题均遵循下述文件结构

```txt
<试题名称>
├─source_code # 存放题目源码的文件夹
│ ├ ...
├─evaluate.py # 自动评测脚本
├─readme.md # 题目描述文档
...
```

## 提交文件要求

最终的提交文件夹应该遵循以下文件结构
``` txt
xflops2024_submit
├─Bithack
│ ├ writeup.md
│ ... (此题要求提交的文件)
├─Clever_Clang
│ ├ writeup.md
│ ... (此题要求提交的文件)
├─Cluster
│ ├ writeup.md
│ ... (此题要求提交的文件)
├─HPL
│ ├ writeup.md
│ ... (此题要求提交的文件)
├─N_Body_Problem
│ ├ writeup.md
│ ... (此题要求提交的文件)
```

例如，你希望提交`Bithack`试题，则提交的文件结构应该如下
``` txt
xflops2024_submit
├─Bithack
│ ├ writeup.md
│ ... (此题要求提交的文件)
```

每题的均需要提交`writeup.md`简单的阐述自己的解题思路，即使题目未解出，也可提交解题思路便于我们了解你的思考内容。我们最终将综合**考核得分**、**解题思路**、**年级**与**个人简历**决定是否通过本轮考核。

为方便大家提交，我们在题目根目录下放置了脚本`submit.py`，在根目录下运行`python submit.py`即可自动的从指定的题目的目录下寻找对应的文件（其寻找路径和各个题目中`evaluate.py`评测时的目标文件路径一致，`writeup.md`将在每个题目的根目录下寻找）并复制到根目录下的`xflops2024_submit`文件夹下（如此文件夹不存在将新建），并压缩为`.tar`文件同时输出该文件的sha256哈希值。
> 请注意，即使没更改对应的提交文件，重复运行`python submit.py`也将导致生成文件的哈希值改变


## 自测脚本

每道题均为大家设置了自测脚本`evaluate.py`，用于帮助大家自测目前的做题与得分情况。
请注意：
* 运行脚本前请先运行`pip install pyyaml`安装脚本所需依赖
* 运行脚本时请在`evaluate.py`所在文件目录下运行
* 运行脚本的评分仅作参考，最终得分以XFLOPS超算队的测试结果为准

## 考核所用队列

考核设置`kp920race`和`kp920vscode`两个队列。

### kp920race

该队列用于运行所需核心数较多的(>8核心)的试题，队列最长运行时间为1h，允许申请的最大的核心数量为128核心，仅允许使用1个节点。

### kp920vscode

该队列用于使用VSCode连接计算节点开发已经交互式操作测试核心数较少的(<8核心)题目，队列最长运行时间为6h，允许申请的最大核心数量为8核心，仅允许使用1个节点。

推荐使用kp920vscode队列进行VSCode连接，用于交互式开发与代码测试。对于需要较多核心数运行的题目，编写slurm脚本并使用sbatch提交到kp920race队列运行。

## 注意事项

1. 所有的试题中给出的命令指导均假设当前命令行环境拥有足够的资源量，实际上你当前命令行环境可能并没有足够的资源量（例如在kp920vscode队列开发时你至多申请到8个核心），请自行编写slurm脚本并使用sbatch提交到kp920race队列以申请到足够多的资源量运行命令。
2. 如遇到`readme.md`中的关于试题评测方法的自然语言描述存在歧义的情况，请根据`evaluate.py`源码的实现逻辑为准，如你确信`evaluate.py`的实现逻辑存在问题，请向我们反馈。
3. 如发现题目中存在问题欢迎向我们反馈（请给出问题的复现方法），我们将向你公开表示感谢。
4. `Clever_Clang`试题将在22,23级提交通道关闭后在HPC入门指南上公布Hint, 在此之后提交此试题的得分将按原分数的60%计算。
5. 请注意行尾序列，本仓库所有文件夹均为LF行尾序列，在使用vscode进行编辑时可在右下角栏查看并确认。文本文件经过windows系统可能自动转为CRLF行尾序列，这将导致部分脚本(.sh)运行异常，如发现你的文本文件为CRLF行尾序列，请更改为LF行尾序列再运行脚本。
6. 请注意及时备份你的文件，错误的运行脚本可能将导致你的数据丢失。如因错误运行脚本数据丢失无法恢复，后果自负。