初始化	git init

让git管理	git add . (.表示所有未被管理的文件，也可以 git add fileName)

配置个人信息

git config --global user.email " 邮箱"

git config --global user.name "姓名"

生成版本	git commit -m 'description'

查看版本记录

git log

ps： 在reset 回到之前版本时，在时间线上，HEAD指向的点之后的 版本记录都看不到

如果想知道所有版本变更历史，

改用

git reflog



切换到指定版本：（ v3回滚到v2，以及v2再回滚到v3）

先用 git reflog 查看版本变更记录，

然后用 git reset --hard 版本号， 这里的版本号在git reflog执行后会显示，16进制的大数

![image-20210608021918211](git%E7%AC%94%E8%AE%B0.assets/image-20210608021918211.png)



查看分支

git branch

创建分支

git branch 分支名

切换分支

git checkout 分支名

分支合并

git merge  要合并的分支

注意:切换分支后再合并,合并后可能要解决冲突

删除分支

git branch -d 分支名





2.5.4 git 工作流

![image-20210610233522984](git%E7%AC%94%E8%AE%B0.assets/image-20210610233522984.png)

## 2.6   与远端协同

​		在家里上传代码

```
1. 给远程仓库起别名	(仅一次)
	git remote add origin 远程仓库地址
2. 向远程推送代码
	git push -u origin 分支名称
```

​		到公司新电脑上第一次获取代码

```
1. 克隆远程仓库地址
	git clone 远程仓库地址 (内部已实现git remote add origin 远程仓库地址, 即起别名为origin)
2. 切换分支
	git checkout 分支
```

​		在公司进行开发

```
1. 切换到dev分支进行开发
	git checkout dev
2. dev代码有点老,先把 master分支合并到dev (仅一次)
	git merge master
3. 修改代码
4. 提交代码, 下班
	git add .
	git commit -m "xx"
	git push origin dev
```

​		到家里继续写代码

```
1. 切换到dev分支进行开发
	git checkout dev
2. 拉代码
	git pull origin dev
3. 继续开发
4. 提交代码
	git add .
	git commit -m "xx"
	git push origin dev
```

### 重复如上

开发完毕, 要上线

```
1.将dev分支合并到master,进行上线
	git checkout master
	git merge dev
	git push origin master
2. 把dev分支有也推送到远端
	git checkout dev
	git merge master
	git push origin dev
```

```
git pull origin dev
=
git fetch origin dev
+
git merge origin/dev


```

![image-20210614161446519](git%E7%AC%94%E8%AE%B0.assets/image-20210614161446519.png)





Rebase 应用场景--用来简化提交记录

### 第一种情况 大 任务,时间跨度大,

注意: 合并记录时, rebase操作尽量不要包括已经push到版本库(remote/远端)的记录

rebase 帮助将除了分支初 始版本和最终版本外的所有中间版本都整合为一个版本.即只有一个记录

![image-20210614170222647](git%E7%AC%94%E8%AE%B0.assets/image-20210614170222647.png)

![image-20210614163531712](git%E7%AC%94%E8%AE%B0.assets/image-20210614163531712.png)

![image-20210614163750079](git%E7%AC%94%E8%AE%B0.assets/image-20210614163750079.png)

回车后会跳出一个可编辑的文本页面,

把除了第一个记录的,后面的所有记录的"pick"改为s,

表示把当前指向的 记录合并到上一个记录,

即: v4 合并到 v3, v3 合并到v2

![image-20210614164345923](git%E7%AC%94%E8%AE%B0.assets/image-20210614164345923.png)



改完后,保存退出即可 (ESC + :wq)

②

修改完,且保存退出后,又跳出一个可编辑页面,

用来修改该rebase的提交信息

![image-20210614164924760](git%E7%AC%94%E8%AE%B0.assets/image-20210614164924760.png)

![image-20210614165027550](git%E7%AC%94%E8%AE%B0.assets/image-20210614165027550.png)

![image-20210614165449771](git%E7%AC%94%E8%AE%B0.assets/image-20210614165449771.png)

### 第二种情况 消除dev分支,将dev上的记录强行插入到master,使得提交记录呈现一条线,

类似于合并

![image-20210614172357775](git%E7%AC%94%E8%AE%B0.assets/image-20210614172357775.png)

使用merge

![image-20210614173047104](git%E7%AC%94%E8%AE%B0.assets/image-20210614173047104.png)

![image-20210614173159217](git%E7%AC%94%E8%AE%B0.assets/image-20210614173159217.png)

![image-20210614173307462](git%E7%AC%94%E8%AE%B0.assets/image-20210614173307462.png)

使用rebase(变基, 将dev上的提交变为 基--master 上的一部分, 合并并弄为一条线)

一般将dev 上的提交强行插入到master时间轴中, 则要先切换到dev分支,然后rebase

```
① 切换到dev	git checkout dev
② 执行rebase,把master上的记录放到dev中,使这些记录在dev上呈一条线
	git rebase master
③ 切换回master, 合并rebase后的分支
	git checkout master
	git merge dev 
```

![image-20210614182724343](git%E7%AC%94%E8%AE%B0.assets/image-20210614182724343.png)

![image-20210614182809588](git%E7%AC%94%E8%AE%B0.assets/image-20210614182809588.png)

###  第三种情况:

①在公司写代码,提交一个版本v1到本地,未push到remote版本库

②回到家里,因为拉取不到在公司写的代码, 故另外写一个功能v2,

且 push到了remote

③第二天回到公司,

一般操作:

```
执行 git pull,拉取了v2,在公司的本地仓库的v1会和拉取的v2自动执行合并

(有不一致的会产生冲突),且 因为有一个自动的合并(merge),所以会产生一个分叉,

PS: git pull 会产生分叉
```



为了不产生分叉:

```
不使用git pull,使用如下代替:
PS: git pull = git fetch + git merge
① git fetch 拉取v2, 但没有产生合并
② git rebase (origin/dev)

详细:
git pull origin dev
替换为如下👇
git fetch origin dev
git rebase origin/dev
```



### 如git rebase 产生冲突,执行如下

① 解决冲突

② 按照提示

git add . 

git commit -m 'XXX'

③ 解决完冲突, 继续rebase, 

 git rebase -- continue



快速解决冲突

1.安装beyond compare

2.在git中配置(这里配置为local,表示仅对当前项目生效)

```
git config --local merge.tool bc4
git config --local mergetool.path 'D:\DevelopTools\Beyond Compare 4'
git config --local mergetool.keepBackup false
```

3.应用beyond compare 解决冲突

```
git mergetool
```

### 总结

添加远程连接(别名)

```
git remote add origin 地址
```

推送代码

```
git push origin 分支名
```

下载代码(所有分支)

```
git clone 地址
```

拉取代码

```
git pull origin dev
等价于
git fetch origin dev
+
git merge origin/dev
```

保持代码提交整洁(变基)

```
git rebase 分支名
```

记录图形展示

```
git log --graph --pretty=format:"%h %s"
```









