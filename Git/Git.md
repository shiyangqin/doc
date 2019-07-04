### 说明
可以先使用小乌龟，了解基本操作都有哪些，再尝试使用命令行操作
### 基础命令
```
git pull  
git push  
git status  
git add main.py  
git commit -m"update"  
git diff
git log
git reset HEAD main.py
```

### rebase
```
git branch -av  			//查看存在的分支
git checkout 分支名			//切到需要合并的分支
git branch -av  			//看分支
git pull
git rebase develop 1-qsy	//可能会有冲突提示

/*解决冲突
git merhettool 
具体操作百度吧
*/

git rebase --continue 		//解决冲突后继续rebase
git status					//查看工作目录和暂存区的状态
git push origin 1-qsp -f 	//强推  注意：会将本地仓库强制推送到远端，轻易不要用
```

### 删除远端已经没有的分支
```
git fetch -p
```

### 当突然出现身份验证失败的时候
```
运行一下命令缓存输入的用户名和密码：
git config --global credential.helper wincred
清除掉缓存在git中的用户名和密码
git credential-manager uninstall
上面的执行后，缓存清掉了，但再次输入的账号密码没有记录，执行下面语句就可以了，我感觉应该是上面的操作先清楚缓存，在保存账号密码
git config --global credential.helper store
```
