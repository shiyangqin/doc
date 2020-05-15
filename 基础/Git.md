# Git/GitHub

+ [Git初始化设置](#Git初始化设置)
+ [vscode中markdownlint插件语法规则](#vscode中markdownlint插件语法规则)

## Git初始化设置

关于git的使用，我一般使用TortoriseGit进行操作，简单快捷易上手。注：有时会因为Git和TortoriseGit版本差异出现问题。

+ 设置用户名和邮箱

```shell
git config --global user.name '用户名'
git config --global user.email '邮箱'
```

+ 生成ssh密钥

```txt
ssh-keygen -t rsa -C '邮箱'

命令执行后，多次按回车至完成
Windows生成位置：C:\Users\系统登陆账户名\.ssh
```

+ 配置

```txt
用文本模式打开.ssh下的id_rsa.pub，复制里面的全部内容

登陆GitHub，
进入个人设置中的SSH选项，
点击New SSH key按钮，
将id_rsa.pub中的内容粘贴在key里面，若title没有自动填充，则随便取一个即可
点击Add SSH key完成添加
```

## vscode中markdownlint插件语法规则

markdownlint的[rules文档](https://github.com/DavidAnson/markdownlint/blob/master/doc/Rules.md)
