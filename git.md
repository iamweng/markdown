#Git

## ssh
```bash
# 生成ssh密钥
ssh-keygen -t rsa
ssh-keygen -t rsa -C "<EMAIL>"
# 验证ssh可用性
ssh -T git@github.com
```


## 代码仓库

### 创建仓库
```bash
# 改变当前目录
cd <dir>
# 初始化仓库
git init
# 拉取远程仓库到本地
git clone git@github.com:<USER_NAME>/<REPOSITORY_NAME>.git <FOLDER_NAME>
# 关联远程仓库
git remote add origin git@github.com:<USER_NAME>/<REPOSITORY_NAME>.git
```

### 添加文件到仓库
```bash
# 添加单个文件到暂存区
git add <FILE_NAME>
# 添加所有文件到暂存区
git add .
# 追踪文件
git add
# 取消追踪文件
git rm
# 丢弃修改
git  checkout
# 忽略的文件
.gitignore
# 提交到本地仓库，不建议使用git commit -m "<COMMIT_MESSAGE>"，建议提交遵循规范
git commit -m "<COMMIT_MESSAGE>"
# 查看工作区状态
git status
# 对比工作区变化，建议将beyongd compare配置为diff工具，用于diff以及merge冲突
git diff
```

### 仓库配置
```bash
# 配置全局用户名
git config --global user.name "<USER_NAME>"
# 配置全局邮箱
git config --global user.email "<EMAIL_ADDR>"
# 配置当前仓库用户名
git config  user.name "<USER_NAME>"
# 配置当前仓库邮箱
git config  user.email "<EMAIL_ADDR>"
```

## 代码版本/提交切换
```bash
# 查看提交详情
git log
# 查看提交简介
git log --pretty=oneline
# 回退到当前最新提交
git reset --hard HEAD
# 回退到上次提交
git reset --hard HEAD^
# 回退到前n次提交
git reset --hard HEAD~<N>
# 回退到某次提交
git reset --hard <COMMIT_ID>
# 查看历史提交以及被回退的提交，该记录有时限且只存在本地
git reflog
# 回到未来版本
git reset --hard <COMMIT_ID>
# 撤销工作区文件修改
git checkout <FILE_NAME>
# 撤销暂存区文件修改
git reset HEAD <FILE_NAME>
# 从版本库中删除文件
git rm <FILE_NAME>
# 从版本库中删除文件，但本地不删除
git rm --cached <FILE_NAME>
# 将文件/文件夹重命名
git mv <FILE_NAME>/<FOLDER_NAME>
```

## 分支

### 创建与合并分支
```bash
# 创建分支
git branch <BRANCH_NAME>
# 创建分支并切换
git checkout -b <BRANCH_NAME>
# 切换分支
git checkout <BRANCH_NAME>
# 合并某个分支到当前分支
git merge <BRANCH_NAME>
# 删除本地未合并分支
git branch -D <BRANCH_NAME>
# 删除本地合并分支
git branch -d <BRANCH_NAME>
# 删除远程分支
git push origin -d <BRANCH_NAME>
git push <REPOSITORY_NAME> -d <BRANCH_NAME>
# 查看当前分支
git branch
# 查看所有本地分支信息
git branch -a <BRANCH_NAME>
# 查看所有远程仓库分支信息
git branch -a <REPOSITORY_NAME>/<BRANCH_NAME>
# 合并分支，解决分支冲突
1.将要合并的分支更新到最新
2.切换到主分支并合并分支
3.解决合并时的conflict并提交到版本库
# 查看分支状态
git log --graph
git log --graph --petty=oneline --abbrey-commit
# 暂存工作现场
git stash
# 恢复工作现场
git stash apply
# 删除工作现场
git stash drop
# 恢复+删除工作现场
git stash pop
# 查看远程仓库详细信息
git remote -v
# 查看远程仓库信息
git remote
# 更新远程仓库信息
git fetch
# 将远程仓库最新修改更新到本地
git pull
# 将本地最新修改推送到远程仓库
git push
# 使用远程分支创建本地分支
git checkout -b <BRANCH_NAME> origin/<BRANCH_NAME>
# 将本地分支与远程分支作关联
git branch --set-upstream <BRANCH_NAME> origin/<BRANCH_NAME>
```

## 代码版本tag
```bash
# 查看本地tag
git tag
# 查看远程tag
git tag -r
# 给当前版本添加tag
git tag <TAG_NAME>
# 给历史版本添加tag
git tag <TAG_NAME> <COMMIT_ID>
# 删除本地tag
git tag -d <TAG_NAME>
# 删除远程tag
git push origin -d <TAG_NAME>
# 推送到远端仓库
git push origin <TAG_NAME>
# 推送所有未提交的tag
git push origin --tags
# tag更新到本地
git pull origin --tags
```

## 其他生僻命令
```bash
git blame
git bisect
git relog
```