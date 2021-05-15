

# 使用步骤：



​	①准备工作

​		1.初始化身份信息（只要不变换电脑，只需要做1次即可）

​		2.克隆仓库代码（下载仓库代码） /  拉取最新的（pull，更新）

​		【工作第1天：克隆，其后的每1天开工前都是pull】

​	**②写代码（对代码的编辑、创建、删除操作）**

​	**③提交本地仓库（暂存区）**

​	**④提交到远程仓库（当天下班的时候）**



①准备工作

初始化身份信息：

```bash
$ git config --global user.name “用户名”
$ git config --global user.email “邮箱地址”
```

②克隆代码（下载仓库）

```bash
$ git clone HTTPS下载地址
```

③提交到本地仓库

```bash
$ git add .
```

④提交到远程仓库

```bash
$ git commit -m “注释”
$ git push
```

配置文件位于当前本地工作区中的.git文件夹中，文件名叫做config。

<a href="https://www.jianshu.com/p/3421093d9fbf">GIT的基本使用链接</a>>

# 生成SSH公钥添加到Gitee中

- 生成shh公钥蜜月

```bash
ssh-keygen -t rsa -C "youremail"
```

一路回车
linux:公钥秘钥(id_rsa.pub)会保存到~/.ssh文件夹下

windows:C:\Users\admin\.ssh

- 将id_rsa.pub公钥内容复制到gitee的设置中

- 测试ssh是否配置成功

  ```bash
  ssh -T git@gitee.com
  ```

# 使用

1、初始化本地仓库

```bash
git init
```

2、添加远端仓库地址

```bash
git remote add origin <仓库https://gitee.com/xxx/xxx.git>
 
git push -u origin master
```



查看分支：git branch

创建分支：git branch 分支名

切换分支：git checkout 分支名



3、拉取远端仓库master分支的项目内容

```bash
git pull repo master
```

4、创建切换新的本地分支，并关联远端分支

​	以下Feat_xxx为自定义分支名，一般设置成与跟踪的远端分支名一样

```bash
# 创建一个新的本地分支
git branch Feat_xxx

# 删除一个本地分支
git brance -D Feat_xxx

# 切换到新创建的分支
git checkout Feat_xxx

# 将新的分支发布到远端仓库上，并指定当前本地分支跟踪的远端分支
git push -u repo Feat_xxx

# 指定本地分支跟踪某一远端分支
git branch -u repo/Feat_xxx
# 或者
git branch --set-upstream Feat_xxx repo/Feat_xxx

# 查看分支跟踪关系
git branch -vv
```

9、修改完本地代码后

```bash
# 添加当前目录修改了的文件放到暂存区（缓存）
git add .

# 将暂存区文件提交到本地仓库
git commit -m "your changes message"

# 将本地仓库的分支推送到远端服务器上对应的分支
git push repo Feat_xxx
```

10、提交pull request，审核通过，将合并到master分支中

11、其他用户看到master更新了

```bash
# 查看本地分支与远端master分支区别
git log -p Feat_xxx repo/master

# 确认没问题后，将master代码pull到本地分支
git pull repo master
```

## 踩坑操作

