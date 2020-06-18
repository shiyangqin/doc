# 在Windows上编写的shell文件上传到linux下执行报错

可能是部分编辑器问题，vscode创建shell文件的没问题，有些软件创建的文件会出现这种错误

Windows上文件格式是dos，linux上是unix，需要手动修改

使用vi/vim可以修改脚本的格式

1、vim 打开文件

2、使用 shift + ：进入命令行模式

3、输入 set ff 会发现 输出的 fileformat = dos

4、命令行模式下使用 set ff = unix 即可修复问题

也可以使用命令直接在linux上创建文件然后执行:

```shell
tee ./shell.sh <<-'EOF'
shell脚本内容
EOF
sh ./shell.sh
```
