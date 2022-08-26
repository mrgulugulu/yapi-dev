# yapi-dev 
基于yapi的二次开发版本，主要修改了登录的逻辑，从原始的账号密码登录改为通过智能网关的内容来登录，鉴权方式暂时使用的是t_uid（即员工的id）。

# 修改的主要内容
1. yapi-dev/client/containers/Login/Login.js
将密码修改为不必须填写
```
{/* 密码 */}
        <FormItem style={formItemStyle}>
          {getFieldDecorator('password', {
            rules: [{ required: false, message: '请输入密码!' }]
          }
```
2. yapi-dev/server/controllers/user.js
```
    let name = ctx.cookies.get("t_uid")
    // 接下来就是进行在mongodb中鉴权的操作
    let result = await userInst.findByName(name);
    // 判断result是否存在，进行对应登录或者拒绝的操作
    ......
```
3. yapi-dev/server/model/user.js
```
  // findByName 通过姓名查询mongodb中是否存在用户
  findByName(username){
    return this.model.findOne({ username: username });
  }

```
# 使用说明
1. 在mongodb上添加免登录的成员信息，并进行二次开发，修改登录的逻辑，实现免登录的操作。

