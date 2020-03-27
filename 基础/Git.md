+ [Git初始化设置](#Git初始化设置)
+ [GitHub图片无法显示问题解决](#GitHub图片无法显示问题解决)


### Git初始化设置

+ 设置用户名和邮箱
```
git config --global user.name '用户名'
git config --global user.email '邮箱'
```

+ 生成ssh密钥
```
ssh-keygen -t rsa -C '邮箱'

命令执行后，多次按回车至完成
Windows生成位置：C:\Users\系统登陆账户名\.ssh
```

+ 配置
```
用文本模式打开.ssh下的id_rsa.pub，复制里面的全部内容

登陆GitHub，
进入个人设置中的SSH选项，
点击New SSH key按钮，
将id_rsa.pub中的内容粘贴在key里面，若title没有自动填充，则随便取一个即可
点击Add SSH key完成添加
```

### GitHub图片无法显示问题解决

此方法有效，如果失效，自行百度新的地址。2020.3.25

```
打开C:\Windows\System32\drivers\etc\hosts
在最后添加：

# GitHub Start 
192.30.253.112    github.com 
192.30.253.119    gist.github.com
151.101.184.133    assets-cdn.github.com
151.101.184.133    raw.githubusercontent.com
151.101.184.133    gist.githubusercontent.com
151.101.184.133    cloud.githubusercontent.com
151.101.184.133    camo.githubusercontent.com
151.101.184.133    avatars0.githubusercontent.com
151.101.184.133    avatars1.githubusercontent.com
151.101.184.133    avatars2.githubusercontent.com
151.101.184.133    avatars3.githubusercontent.com
151.101.184.133    avatars4.githubusercontent.com
151.101.184.133    avatars5.githubusercontent.com
151.101.184.133    avatars6.githubusercontent.com
151.101.184.133    avatars7.githubusercontent.com
151.101.184.133    avatars8.githubusercontent.com
# GitHub End
```
