####打标签
Git 也可以对某一时间点上的版本打上标签。人们在发布某个软件
版本（比如v1.0 等等）的时候，经常这么做。

####列显已有的标签
列出现有的标签非常简单
```java
$ git tag
```
显示的标签按字母顺序排列，所以标签的先后并不表示重要程度的轻重。
我们可以用特定的搜索模式列出符合条件的标签。在Git 自身项目仓库中，有着超过
240 个标签，如果你只对1.4.2 系列的版本感兴趣，可以运行下面的命令：

```
    $ git tag -l 'v1.4.2.*'
    v1.4.2.1
    v1.4.2.2
    v1.4.2.3
    v1.4.2.4
```

####新建标签
Git 使用的标签有两种类型：轻量级的（lightweight）和含附注的（annotated）。轻量
级标签就像是个不会变化的分支，实际上它就是个指向特定提交对象的引用。而含附注标
签，实际上是存储在仓库中的一个独立对象，它有自身的校验和信息，包含着标签的名字，
电子邮件地址和日期，以及标签说明，标签本身也允许使用GNU Privacy Guard (GPG) 来
签署或验证。一般我们都建议使用含附注型的标签，以便保留相关信息；当然，如果只是临
时性加注标签，或者不需要旁注额外信息，用轻量级标签也没问题。

含附注的标签
创建一个含附注类型的标签非常简单，用-a （译注：取annotated 的首字母）指定标
签名字即可：
```java
$ git tag -a v1.4 -m 'my version 1.4'
$ git tag
v0.1
v1.3
v1.4
```

####签署标签
如果你有自己的私钥，还可以用GPG 来签署标签，只需要把之前的-a 改为-s （译注：
取Signed 的首字母）即可：
```java
$ git tag -s v1.5 -m 'my signed 1.5 tag'
You need a passphrase to unlock the secret key for
user: "Scott Chacon <schacon@gee-mail.com>"
1024-bit DSA key, ID F721C45A, created 2009-02-09
```
现在再运行git show 会看到对应的GPG 签名也附在其内：


####轻量级标签

轻量级标签实际上就是一个保存着对应提交对象的校验和信息的文件。要创建这样的标
签，一个-a，-s 或-m 选项都不用，直接给出标签名字即可：
```java
$ git tag v1.4-lw
$ git tag
v0.1
v1.3
v1.4
v1.4-lw
v1.5
```
现在运行git show 查看此标签信息，就只有相应的提交对象摘要：

###验证标签
可以使用git tag -v [tag-name] （译注：取verify 的首字母）的方式验证已经签署的
标签。此命令会调用GPG 来验证签名，所以你需要有签署者的公钥，存放在keyring 中，
才能验证：
```java
$ git tag -v v1.4.2.1
object 883653babd8ee7ea23e6a5c392bb739348b1eb61
type commit
tag v1.4.2.1
tagger Junio C Hamano <junkio@cox.net> 1158138501 -0700
GIT 1.4.2.1
Minor fixes since 1.4.2, including git-mv and git-http with alternates.
gpg: Signature made Wed Sep 13 02:08:25 2006 PDT using DSA key ID F3119B9A
gpg: Good signature from "Junio C Hamano <junkio@cox.net>"
gpg: aka "[jpeg image of size 1513]"
Primary key fingerprint: 3565 2A26 2040 E066 C9A7 4A7D C0C6 D9A4 F311 9B9A
```
若是没有签署者的公钥，会报告类似下面这样的错误：
```java
gpg: Signature made Wed Sep 13 02:08:25 2006 PDT using DSA key ID F3119B9A
gpg: Can't check signature: public key not found
error: could not verify the tag 'v1.4.2.1'
```

我们忘了在提交“updated rakefile” 后为此项目打上版本号v1.2，没关系，现在也
能做。只要在打标签的时候跟上对应提交对象的校验和（或前几位字符）即可：
```java
$ git tag -a v1.2 9fceb02
```
可以看到我们已经补上了标签：

####分享标签
默认情况下，git push 并不会把标签传送到远端服务器上，只有通过显式命令才能分享
标签到远端仓库。其命令格式如同推送分支，运行git push origin [tagname] 即可：
```java
$ git push origin v1.5
Counting objects: 50, done.
Compressing objects: 100% (38/38), done.
Writing objects: 100% (44/44), 4.56 KiB, done.
Total 44 (delta 18), reused 8 (delta 1)
To git@github.com:schacon/simplegit.git
* [new tag] v1.5 -> v1.5
```
如果要一次推送所有（本地新增的）标签上去，可以使用--tags 选项：
```java
$ git push origin --tags
Counting objects: 50, done.
Compressing objects: 100% (38/38), done.
Writing objects: 100% (44/44), 4.56 KiB, done.
Total 44 (delta 18), reused 8 (delta 1)
To git@github.com:schacon/simplegit.git
* [new tag] v0.1 -> v0.1
* [new tag] v1.2 -> v1.2
* [new tag] v1.4 -> v1.4
* [new tag] v1.4-lw -> v1.4-lw
* [new tag] v1.5 -> v1.5
```
现在，其他人克隆共享仓库或拉取数据同步后，也会看到这些标签。

####Git 命令别名
Git 并不会推断你输入的几个字符将会是哪条命令，不过如果想偷懒，少敲几个命令的字
符，可以用git config 为命令设置别名。来看看下面的例子：
```java
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
```
现在，如果要输入git commit 只需键入git ci 即可。而随着Git 使用的深入，会有
很多经常要用到的命令，遇到这种情况，不妨建个别名提高效率。
使用这种技术还可以创造出新的命令，比方说取消暂存文件时的输入比较繁琐，可以自己
设置一下：
```java
$ git config --global alias.unstage 'reset HEAD --'
```
这样一来，下面的两条命令完全等同：
```java
$ git unstage fileA
$ git reset HEAD fileA
```
显然，使用别名的方式看起来更清楚。另外，我们还经常设置last 命令：
```java
$ git config --global alias.last 'log -1 HEAD'
```
然后要看最后一次的提交信息，就变得简单多了：
```java
$ git last
commit 66938dae3329c7aebe598c2246a8e6af90d04646
Author: Josh Goebel <dreamer3@example.com>
Date: Tue Aug 26 19:48:51 2008 +0800
test for current head
Signed-off-by: Scott Chacon <schacon@example.com>
```
可以看出，实际上Git 只是简单地在命令中替换了你设置的别名。不过有时候我们希望
运行某个外部命令，而非Git 的附属工具，这个好办，只需要在命令前加上! 就行。如果
你自己写了些处理Git 仓库信息的脚本的话，就可以用这种技术包装起来。作为演示，我
们可以设置用git visual 启动gitk：
```java
$ git config --global alias.visual "!gitk"
```