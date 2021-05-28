# 项目页面(Project Page)
- 状态：Draft
- 作者：Zhang Ji zhangji@parsec.com.cn

## 1. 默认布局
### 1.1. 概述
1. 页面顶部应该显示项目名称。
2. 页面顶部应该显示tab，包含：a)接口；b)实体；c)动态；d)成员；e)设置。
3. 点击tab应该切换到对应页面。

## 2. 「项目接口」页面
1. 项目接口页面应该按照[PIP.4.md](PIP.4.md)定义实现。

## 3. 「项目实体」页面
1. 项目实体页面应该按照[PEP.5.md](PEP.5.md)定义实现。

## 4. 「项目动态」页面
1. 项目动态页面应该按照[PHP.6.md](PHP.6.md)定义实现。

## 5. 「项目成员」页面
1. 应该显示项目成员列表，每个项目成员的字段包括：a)头像；b)姓名；c)角色；d)设置按钮（如果当前用户无项目管理权限则不显示）
   * 角色包括：管理员、成员。
   * 一个项目可以有0个或1个管理员。
   * 仅项目管理员和团队管理员有项目管理权限。
   * 如果从团队中移除成员，则自动从团队中所有项目中移除此成员。
   * 点击设置按钮后应该显示下拉菜单，包括：a)「从项目移除」按钮；b)「设为管理员」按钮（如果成员是项目管理员则不显示）。
2. 应该显示添加成员的单行输入框和「添加成员」按钮（如果当前用户无项目管理权限则不显示）。
   * 在输入框输入文字，应该弹出下拉菜单，包含搜索出的用户列表，每个用户字段包括：a)姓名；b)邮箱。
   * 搜索条件包含用户姓名汉字、姓名拼音全拼、姓名拼音首字母、邮箱等。
   * 搜索范围仅限于当前团队的成员。
   * 如果用户已经属于当前项目，则下拉菜单菜单中显示✔。
   * 点击下拉菜单中的成员，则：
      * 下拉菜单中显示✔，下拉菜单不收起。输入框中增加用户的标签。
      * 输入框中插入{用户姓名}的标签，点击标签的✖可以删除标签。
   * 下拉菜单中的✔，不能通过再次点击取消。
   * 点击「添加成员」按钮，可以批量添加已选中的成员。
   * 添加成员不需要被添加的用户同意确认。

## 6. 「项目设置」页面
1. 应该显示「导入openapi」按钮，点击弹出文件上传控件。
2. 应该显示「导出openapi」按钮，点击弹出文件下载控件。
3. 应该显示“删除项目”文字，和「删除当前项目」按钮（如果当前用户无项目管理权限则不显示）。
   * 点击按钮应该弹出模态框。
   * 模态框应该显示如下文字：“删除项目：{项目名称}”。
   * 模态框应该显示如下文字：“重要提示：删除项目后，所有内容都会被立即删除，不可恢复。”。
   * 模态框应该包含form表单，输入项包括：a)当前用户的登录密码；b)「确认删除」和「取消」按钮。