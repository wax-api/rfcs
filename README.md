## RFC流程简介
RFC(request for comments)流程的目的，是为项目的演进提供公开的决策机制。

例如错误修复、文档改进等修改，都可以通过正常的GitHub PR工作流程合并到主干(main)。

但有些修改是"重大的(substantial)"的，我们要求这些修改要经过一定的设计过程，并在wax核心团队和社区中产生共识后，方可合并到主干。

## RFC演进规则
1. 新建或修改RFC的PR只能由“维护者”提出。
2. RFC的PR只能由“管理者”参考“维护者团队”的一般价值判断，决定是否同意或关闭。
3. 新建RFC的状态必须是Draft。
4. 如果Draft已经在main分支实现，则状态应修改为Stable。
5. 如果Draft无法在限定时间内成为Stable状态，则状态应修改为Deferred。
6. 如果RFC需要被弃用，则状态应修改为Deprecated，并列出替换它的RFC。
7. Deprecated经过足够的时间后，main分支不再支持，则状态应改为Retired。
