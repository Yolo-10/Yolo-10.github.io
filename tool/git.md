## 常用命令
```bash
# 基本操作
git init                        #初始化仓库
git add .                       #将新创建的所有文件提交到暂存区
git commit -m "explain"         #将暂存区内容提交本地仓库里    
git push -u origin master       #将版本库（本地仓库）里的东西上传到远程仓库origin的master分支
git pull origin master          #将远程仓库origin的master分支拉到本地

# 回退操作
git reset HEAD                  #将暂存区所有修改退回
git reset --soft 版本号          #回退commit操作，保存原有修改

# 查看操作
git log                         #查看commit history
git status                      #查看文件工作区、暂存区的状态

# 分支操作
git add -a                      #查看所有分支(本地+远程)
git branch -v                   #查看本地仓库的所有分支
git branch A                    #创建分支A
git checkout -b A               #创建并转到分支A
git checkout main               #切换到分支main
git branch -d B                 #删除分支B
git branch -m B                 #将当前分支重命名为B

# 连接远程仓库
git remote add origin XX        #将本地仓库与远程仓库连接,明名为origin
git remote remove origin        #移除连接的远程仓库origin    
```