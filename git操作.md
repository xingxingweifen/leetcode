# git 常用命令

## 工程准备

1. **git init用于在本地目录下新建git项目仓库**

2. **git clone用于克隆远端工程到本地磁盘**

   如果想从远端服务器获取某个工程，那么：

   - 确定自己Git账号拥有访问、下载该工程的权限
   - 获取该工程的Git仓库URL
   - 本地命令行执行git clone [URL]或git lfs clone [URL]
   - **git lfs专门针对二进制文件进行区别管理，若克隆项目中具有二进制文件则应务必使用git lfs clone。否则克隆操作无法下载到工程中的二进制文件，工程内容不完整**

## 新增/删除/移动文件到暂存区
1. **在提交修改的文件之前，需要使用git add 将该文件添加到暂存区，git add . or git add -all添加所有新增文件**  

2. **git rm将指定文件彻底从当前分支的缓存区删除，因此它从当前分支的下一个提交快照中被删除。**  

3. **git mv命令用于移动文件，也可以用于重命名文件**  

   例1：需要将文件codehunter_nginx.conf从当前目录移动到config目录下，可以执行：  

   ```shell
   git mv codehunter_nginx.conf config
   ```

   例2：需要将文件codehunter_nginx.conf重命名为new_ngix.conf，可执行：

   ```shell
   git mv config/codehunter_ngix.conf config/new_ngix.conf
   ```

## 查看工作区

**git diff用于比较项目中任意两个版本（分支）的差异，也可以用来比较当前的索引和上次提交间的差异。**

1. 比较两个节点之间的差异

2. 比较两个分支之间的差异

   ```shell
   git diff main myBranch
   # 输出
   diff --git "a/git\346\223\215\344\275\234.md" "b/git\346\223\215\344\275\234.md"
   new file mode 100644
   index 0000000..15740ad
   --- /dev/null
   +++ "b/git\346\223\215\344\275\234.md"
   @@ -0,0 +1,4 @@      -代表main下  +代表myBranch下
   +# git 常用命令
   +## 新增/删除/移动文件到暂存区
   +1. **在提交修改的文件之前，需要使用git add 将该文件添加到暂存区**
   +2. **git rm将指定文件彻底从当前分支的缓存区删除，因此它从当前分支的下一个提交快照中被删除。**
   \ No newline at end of file
   diff --git a/test.cpp b/test.cpp
   deleted file mode 100644
   index c9ca697..0000000
   --- a/test.cpp
   +++ /dev/null
   @@ -1,7 +0,0 @@
   -#include <iostream>
   -using namespace std;
   -
   -int main(){
   -       cout << "你好！" << endl;
   -       return 0;
   -}
   
   ```

   

3. 当前的索引和上次提交间的差异

4. 在diff后面加--name-status参数，只看文件列表

   ```shell
   $ git diff main myBranch --name-status
   A       "git\346\223\215\344\275\234.md"   A--means add
   D       test.cpp						   D--means delete
   										   M--means modify
   ```

   **git status 用于显示工作目录和暂存区的状态。** 

   ```shell
   yjw@yjwPc MINGW64 /e/yjw/study/C++/test/leetcode (myBranch)
   $ git status
   On branch myBranch
   Changes not staged for commit:
     (use "git add <file>..." to update what will be committed)
     (use "git restore <file>..." to discard changes in working directory)
           modified:   "git\346\223\215\344\275\234.md"
   
   no changes added to commit (use "git add" and/or "git commit -a")
   
   ```

## 提交更改的文件

   **git commit主要是将暂存区里的文件改动提交到本地的版本库中**

   这个动作是本地动作，是往本地的版本库中记录改动，不影响远端服务器。git commit一般需要附带提交描述信息，所以常见用法是

   ```shell
   git commit yourFile_name -m "commit message"
   or
   git commit -am "commit message"
   git commit -amend # 可以修改最近一次提交的描述
   ```

   提交成功之后，git日志可查到此次提交的id和提交的描述信息。

## 查看日志

**git log用于查看提交历史**可以加上--name--status查看对应的文件名和其状态

## 推送远端仓库

**在使用git commit命令将自己的修改从暂存区提交到本地版本库后，可以使用git push将本地版本库的分支推送到远程服务器上对应的分支**

**window操作系统git分支名大小写不敏感**

```shell
git push origin branch_name
该语句将本地分支branch_name的内容用于更新远程分支的branch_name
```

```shell
git push origin branch_name:new_branch_name
该语句将本地分支branch_name 保存为远端上新建分支new_branch_name（注意:之间不能有空格）
```

## 分支管理

**git branch命令即可查看本地工程的所有git分支名称**

```shell
yjw@yjwPc MINGW64 /e/yjw/study/C++/test/leetcode (myBranch)
$ git branch
  main
* myBranch
  new_myBranch
其中带*的即为当前激活的分支
```

**使用git branch -r查看远端服务器上所拥有的分支，返回的分支名带origin前缀，表示在远端**

```shell
yjw@yjwPc MINGW64 /e/yjw/study/C++/test/leetcode (myBranch)
$ git branch -r
  origin/HEAD -> origin/main
  origin/main
  origin/newMyBranch
```

**如果想查看远端服务器和本地工程所有的分支，那么执行git branch -a即可**

git branch 和 git checkout -b -的异同

**相同点：**

git branch 和git checkout -b都可以用于新建分支（默认基于当前分支节点创建）   

**区别点：**

git branch 新建分支后并不会切换到新分支

git checkout -b新建分支后会自动切换到新分支

常见的新建分支命令格式

```shell
git branch new_branch_name
or 
git checkout -b branch_name
```

**删除本地分支**

git branch -d branch_name和git branch -D branch_name都可以用来删除本地分支，后者大写代表强制删除

**删除远程分支**

```shell
git branch -d -r branch_name（远程分支应当写完整像origin/newMyBranch）
其中branch_name为本地分支名
删除后还要推送到服务器上才行
git push origin :branch_name（需要注意origin后面有一个空格）
or
git push origin --delete oldBranch # 推荐方式
```

**使用git checkout切换分支**

有时候，当前分支工作区存在修改而未提交的文件，与目的分支上的内容冲突，会导致checkout切换失败，这时候，可以使用git checktout -f 进行强制切换。

**常用的切换分支命令格式：git checkout branch_name**

### 更新

**使用git pull从远端服务器获取某个分支的更新，在于本地指定的分支进行自动合并**

```shell
git pull origin remote_branch:local_branch
如果远程指定的分支与本地指定的分支相同，则可以直接执行
git pull origin remote_branch
```

**git fetch的作用是，从远端服务器中获取某个分支的更新到本地仓库。注意，与git pull不同，git fetch在获取到更新后，并不会进行合并（即git merge操作），这样能留给用户一个操作空间，确认git fetch内容符合预期之后，再决定是否手动合并节点。**

```shell
常用的获取远端分支更新命令格式：
git fetch origin remote_branch:local_branch
如果远程指定的分支与本地指定的分支相同，则可直接执行
git fetch origin remote_branch

不能在当前分支内执行对当前分支的更新操作
```

### 分支合并
**git merge命令是指从指定的分支（节点）合并到当前分支的操作**
常用的命令格式为：
```shell
git merge branch_name  # 意味着将分支branch_name合并到当前分支上，形成新的节点，同时对历史的节点无影响。
```

**git rebase也是用于合并目标分支内容到当前分支**（与git merge并不完全相同）

常用命令格式：

```shell
git rebase branch_name
```
### 撤销操作
**git reset**通常用于撤销当前工作区中的某些**git add/commit**操作，可将工作区内容回退到历史提交节点。常用的工作区回退命令格式为:
```shell
git reset --soft commit_id # 若直接使用该命令会保留工作区的修改，即文件改动还在但会从暂存区移除。(default --soft)
git reset --hard commit_id # 若想彻底丢弃修改需使用这条命令，会永久删除未提交的改动。
```

**git checkout .** 用于回退本地所有修改而未提交的文件内容。  
**git checkout .** 是一条有风险的命令，因为它会取消本地工作区的修改（相对于暂存区），用暂存区的所有文件直接覆盖本地文件，达到回退内容的目的。但它不给用户任何确认机会，**所以谨慎使用**。  
常用的回退命令格式为：
```shell
git checkout .
git checkout -filename # 仅仅回退某个文件的未提交改动
git checkout commit_id # 将工具区回退（检出）到某个提交版本。 更接近git reset --hard 但是更加安全（若工作区有未提交的改动，checkout会拒绝切除/除非添加-f强制）。
```