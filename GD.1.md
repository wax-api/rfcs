# 治理文件(Governance Document)
- 状态: Draft
- 作者: Zhang Ji zhangji@parsec.com.cn

wax社区对其项目治理使用GD.1流程。

## 1. 项目管理
1. 任何人都可以创建issue，用来提bug、建议、问题等等。
2. 任何用户都可以创建PR(pull request)，如果PR被同意，则此用户可以被称为“贡献者(contributor)”。
3. 如果PR是“重大的(substantial)”，则必须要求列出对应的rfc。
4. “维护者(maintainer)”有同意和关闭PR的权限。“维护者团队”应该在限定时间内同意或关闭PR。
5. “维护者”可以邀请“贡献者”成为“维护者”。
6. “管理者(administrator)”应该将长期无贡献的“维护者”移入离休名单。
7. “管理者”可以邀请值得托付的“维护者”成为“管理者”。

## 2. RFC演进规则
1. 新建或修改RFC的PR只能由“维护者”提出。
2. RFC的PR只能由“管理者”参考“维护者团队”的一般价值判断，决定是否同意或关闭。
3. 新建RFC的状态必须是Draft。
4. 如果Draft已经在main分支实现，则状态应修改为Stable。
5. 如果Draft无法在限定时间内成为Stable状态，则状态应修改为Deferred。
6. 如果RFC需要被弃用，则状态应修改为Deprecated，并列出替换它的RFC。
7. Deprecated经过足够的时间后，main分支不再支持，则状态应改为Retired。

## 3. UI演进规则
1. 任何人都可以创建UI相关PR。
2. UI相关PR只能由“管理者”参考各种意见后，根据“管理者团队”的偏好，决定是否同意或关闭。
