# GitHub 学习笔记

## 注册一个 GitHub 账号

不知道你是否和我一样，认为注册一个外国网站的账号是一个十分困难的事情，其实不然。
在国内我们注册的每一个账号都离不开手机号，而国外注册的账号则都离不开邮箱。你只需要一个邮箱，然后打开 GitHub 官网，走完流程，你就可以拥有一个属于自己的 GitHub 账号了。

## 常见问题解决

### GitHub 图片无法显示

1. 参考资料
   [1][Python-Core-50-Courses](<https://github.com/jackfrued/Python-Core-50-Courses>)
2. 解决方法
   将下面的信息添加到 hosts 文件中
   ```INI
   151.101.184.133    assets-cdn.github.com
   151.101.184.133    raw.githubusercontent.com
   151.101.184.133    gist.githubusercontent.com
   151.101.184.133    cloud.githubusercontent.com
   151.101.184.133    camo.githubusercontent.com
   ```
