## 在window下如何安装 Git
去官网只下载吧~~  [点击自己查](http://www.baidu.com/) 

##初次运行git 前的配置
在新的系统上，我们都需要先配置自己的git工作环境，配置一次就可以了。
git提供一个`git config`的工具（命令）。专门用来配置或读取相应的工作环境变量。

###用户信息
第一个要配置的时个人信息，`用户名与电子邮件地址`。 每次git提交时都会引用这两条信息，说明是睡提交了更新。

```java
$ git config --global user.name "M"
$ git config --golbal user.email 283125476@qq.com
```

如果其它项目要用到其它的用户名，那么只需要去掉--global选项重要配置即可，新的设定保存当前项目.git/config文件里