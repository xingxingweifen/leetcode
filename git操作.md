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
1. **在提交修改的文件之前，需要使用git add 将该文件添加到暂存区**

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
   //输出
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
   ```

   提交成功之后，git日志可查到此次提交的id和提交的描述信息。

## 查看日志

**git log用于查看提交历史**

   

   