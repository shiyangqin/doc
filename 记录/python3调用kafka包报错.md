# python3调用kafka包报错

python3.7新增关键字：async、await

kafka-python 用到了关键字async，产生了冲突

解决方案1：

将kafka-python中的async改名

解决方案2：

更换到3.7以下的python版本
