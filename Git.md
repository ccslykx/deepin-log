## 提交PR (Pull Request)

1. fork
2. 克隆到本地
   ```bash
   git clone ${Repository}
   ```
4. 编辑
5. push到自己的github
   ```bash
   git add ./
   git commit -m "comments"
   git push
   ```
6. 提交PR
   在分支选项下有一行`This branch is 1 commit ahead of ${owner}/${Repository}:${branch}.`，后面有个`Contribute`按钮，点击后再点`Open pull request`

## 向已有项目新建分支

```
git checkout -b dev-ccslykx

git push origin dev-ccslykx #推送到远程仓库
```

## 合并分支并推送到远程

```
git checkout main #切换到main分支

git pull #拉取更新

git merge dev-ccslykx #合并分支

git push #推送到远程

git checkout dev-ccslykx #切换回来
```

## 切换tag并编辑

`git clone` 整个仓库后使用，以下命令就可以取得该 `tag` 对应的代码了。 

```
git checkout tag_name 
```

但是，这时候 `git` 可能会提示你当前处于一个“detached HEAD" 状态。因为 `tag` 相当于是一个快照，是不能更改它的代码的。
如果要在 `tag` 代码的基础上做修改，你需要一个分支： 

```
git checkout -b branch_name tag_name
```

其中`branch_name`为新建的分支名。这样会从 tag 创建一个分支，然后就和普通的 git 操作一样了。

> ————————————————
> 版权声明：本文为CSDN博主「DinnerHowe」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
> 原文链接：https://blog.csdn.net/DinnerHowe/article/details/79082769