```bash
# 表示只是改变了HEAD的指向，本地代码不会变化，我们使用git status依然可以看到，同时也可以git commit提交
git reset --soft
# 直接回改变本地源码，不仅仅指向变化了，代码也回到了那个版本时的代码。
git reset --hard
```



```bash
# 回退到上一次commit。
git reset --soft HEAD^ 
# 回退到上2次commit。你提交了几次可以改这个数字。
git reset --soft HEAD~2 
```

