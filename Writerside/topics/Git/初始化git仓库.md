# 初始化git仓库

## You have an empty repository

To get started you will need to run these commands in your terminal.

New to Git? [Learn the basic Git commands](http://docs.atlassian.com/bitbucketserver/docs-065/Basic+Git+commands?utm_campaign=in-app-help&utm_medium=in-app-help&utm_source=stash)

### Configure Git for the first time

```
git config --global user.name "张勤忠"
git config --global user.email "zhangqinzhong@lasoplanet.com"
```

### Working with your repository

#### I just want to clone this repository

If you want to simply clone this empty repository then run this command in your terminal.

```
git clone ssh://git@git.dev.genebox.cn:7999/api/laso_cms.git
```

#### My code is ready to be pushed

If you already have code ready to be pushed to this repository then run this in your terminal.

```
cd existing-project
git init
git add --all
git commit -m "Initial Commit"
git remote add origin ssh://git@git.dev.genebox.cn:7999/api/laso_cms.git
git push -u origin master
```

#### My code is already tracked by Git

If your code is already tracked by Git then set this repository as your "origin" to push to.

```
cd existing-project
git remote set-url origin ssh://git@git.dev.genebox.cn:7999/api/laso_cms.git
git push -u origin --all
git push origin --tags
```

#### All done with the commands?