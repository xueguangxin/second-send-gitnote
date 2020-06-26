```
Git详解
https://blog.csdn.net/jiahuan_/article/details/105933423
```



# Git命令行

## 1.0 本地库初始化

###      -- 命令：git init![](C:\Users\未闻花名\AppData\Roaming\Typora\typora-user-images\image-20200625083407012.png)

``` 
ll|less 看我们系统文件
ls -lA|less 看我们的隐藏文件，因为我们的.config是隐藏文件，所以需要通过这个来查看
ctrl + Z 返回命令行



//本地库的创建
mkdir [filename]  创建一个文件夹
cd [filename]     cd到文件夹
git init          初始化一下
----------------------------------------------
后面接着创建vim 文件就可以
```

   --注意：git 目录存放的是本地库相关的子目录和文件，不要删除，也不要胡乱修改

1.1 设置签名

​     --形式

​       用户名：xueguangxin

​       Email地址：550489119@qq.com

​       作用: 区分不同开发人员的身份

​       辨析：这里设置的签名和登陆远程库(代码托管中心)的账号、密码没有任何关系

​       命令：

​               项目级别(仓库级别)： 仅在当前本地库范围内有效

```
git config user.name xueguangxin
git config user.email 550489119@qq.com
信息保存位置 ./git/comfig 文件
```

​               系统用户级： 登陆当前操作系统用户的范围

```
git config --global user.name xueguangxin
git config --global 550489119@qq.comn
```

​                查询系统用户级签名

```
cat .git/config
```

​               查询系统用户级签名

```
cd ~  (查询+目录)
Pwd   (可以看到我们的系统用户级签名)

可以直接用一个命令查询
~/.gitconfig (直接可以看到)
```

​               优先级级别：就近原则：项目级别优先于系统用户级别，二者都有时候采用项目级别的签名

​                                       二者都没有：不会被允许



### 2.0 添加提交以及检查状态

插入一个vim txt文件

```
vim good.txt(文件名可以随意定义)
            进入编辑模式，可以随意的写一段文本
            写完之后的保存退出:  --按下ESC退出编辑模式
                              --打开字母大写，连按2次ZZ 可以退出
            退出之后，我们可以查看一下文件状态
            
git status(查看vim txt 文件状态)
```



把vim txt文件放入到暂存区

```
git status  --可以看到txt文件的状态等待我们add
git add good.txt (将文件放到入暂存区)

文件放到到暂存区之后可以通过查看状态再次从暂存区取出来
git status
git rm --cached good.txt

```

提交vim txt文件

```
git commit good.txt
```

修改原来的vim txt文件

```
直接跟写入vim文件一样，修改完之后 ESC + ZZ
这个时候可以看到status 改变了提示没有add到暂存区
这里会提示可以直接commit 来提交了，不需要在add添加之后在提交

这里有一个快捷操作，一般我们commit 之后会进入vm编辑器保存
这里可以直接输入
git commit -m "my second commit,change good.txt" good.txt
```



### 3.0基本操作

​       --状态栏查看

```
git status  查看工作区、暂存区状态
```

​      --添加文件

```
git add[file name]   将工作区的"新建/修改" 添加到暂存区
```

​      -- 提交操作

```
git commit -m "备注信息" [file name]  将暂存区的内容提交到本地库
```

​      --查看历史记录操作

```
git log  最完整的形式--查看自己修改的所有文件历史记录，能很好的看到每次修改日期和备注
git log --pretty=oneline  简洁方式--留取了Hash值，将历史记录直接以一行的形式显示
git log --oneline  更简洁发方式，Hash值更简洁了
git reflog  在oneline版本之上，显示了 移动到当前版本需要的步数
多屏显示控制方式：space 向下翻页
               b 向上翻页
               q 退出
              
```

   --历史记录的前进和后退

```
基于索引值操作
首先先查看一下历史版本需要移动的步数  然后跳转
git reflog
git reset --hard index(index这里就是要跳转的哈希值)
cat good.txt(查看一下文档内部的内容)
```

```
基于^符号
缺点：只能后退不能前进 移动后也不能看到以前的版本了
git reflog
git reset --hard HEAD^   (一个^符号后退一步)
git reset --hard HEAD~3  (~number  表示后退多少步 )

```

--reset 命令的三个参数对比

```
--soft   仅仅在本地库移动HEAd指针
--mixed  在本地库移动HEAD指针，也会重置暂存区
--hard   在本地库移动HEAD指针，也会重置暂存区，也会重置工作区
```



### 4.0 分支结构

```
分支的好处
1.同时并进推进多个功能开发，提高发开效率
2.各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有影响，失败的分支删除重新开始即可
```

--创建分支

```
git branch name      name就是需要创建的分支名
git branch -v        查看当前所在的分支
git checkout name    切换分支名

```

--合并分支

```
第一步：切换到接受修改的分支上(被合并，增加新内容)上
git checkout host_fix   
1.(host_fix 就是我们创建的新分支)
2.切换完之后，修改或者添加文件(用 git checkout)
3.修改之后的文件添加到暂存区然后上传到本地库（用add/commit 命令来操作）
                         
第二部：执行merge命令
1.上传完分支的内容之后，可以看一下我们的分支(git branch -v)
2.切换到我们的主干上 (git checkout master) 这里会发现主干和分支的前面的哈希值版本号不一样
2.git merge host_fix (合并分支到主干上面)
```

--分支冲突

```
1.当一个内容在master 和 host_fix 同时修改之后，我们在合并分支的时候，就会显示分支合并冲突
2.打开冲突的文件查看下(cat [filename]) 会发现有2处修改，一处是master 一处是host_fix
3.编辑文件，删除特殊符号，修改完文件后保存退出
4.git add [filename]   再次添加到缓存区
5.git status           再次查看一下文件的状态
5.git commit           提交文件(注意这里，不能添加文件名，否则会报错)
6.git status           这里会显示冲突被解决了
```



### 5.0 本地库和远程库的连接

``` 
1.创建本地库
mkdir [filename]   //创建文件夹
cd [filename]
git init

vim [filename01.txt]  //创建text文件  添加内容保存
git add [filename01.txt]
git add commit -m "备注" [filename.txt]


2.创建远程库
  1.GitHub中创建一个resposity 然后给一个文件名
  2.复制一下我们GitHub仓库的Http地址(比如：https://github.com/xueguangxin/OneDay.git)
  
  
3.返回git命令行储存地址
  因为GitHub链接很长，所以我们可以在git 当中存储起来方便使用
  
  git remote -v    //查看已经储存文件/地址
  git remote add origin https://github.com/xueguangxin/OneDay.git
  git remote -v    //这里就能看到我们的地址
  
  
4.推送操作
  git push origin master      //推送到主干
  刷新一下我们的GitHub 就会看到内容了
  
  
5.克隆操作    
  1.完整的把远程库下载到本地
  2.创建origin远程地址别名
  3.初始化本地库
  mkdir[filename]   //创建自己的文件夹
  git clone https://github.com/xueguangxin/OneDay.git    //地址是我们远程仓库的地址
  
  ll    //查看我们下载的文件夹
  cd [filename]   //进入文件夹
  ll              //可以看到我们的具体文件了
  
  
6.添加新成员
  这里在GitHub中 邀请新成员，同意一下就可以了
  然后新成员克隆代码修改后也可以上传到远程仓库
  
  
7.拉取最新分支(pull = fetch + merge)
  当新成员修改代码上传之后，我们本地库没有更新这个时候需要更新一下我们的本地库
  
  ---------------------------------------------------------------------------
  7.1 当我们操作复杂的时候 选择用 fetch + merge 不停从查看文件变化防止错了
  
  git fetch origin master([远程库地址别名][远程分支名]) //此时本地还没有更新
  
  git checkout origin/master      //我们可以看到远程仓库的内容
  cat [filename.txt]
  
  git checkout origin             //可以再次切换到我们的本地的库
  cat [filename.txt]              //可以看到和远程仓库内容是不同步的
  
  git merge origin/master([远程库地址别名]/[远程分支名]) //合并远程仓库的内容到我们本地库
  cat [filename.txt]              //可以看到和远程库内容是一致的了
  
  
  ------------------------------------------------------------------------------
  7.2 当我们修改的内容特别简单的时候  可以使用 pull 操作 可以一部到位
  git pull origin master([远程库地址别名][远程分支名])
  cat [filename.txt]   //可以直接看到已经和远程仓库同步了
  
  
  
  
  
  
  
  
```

