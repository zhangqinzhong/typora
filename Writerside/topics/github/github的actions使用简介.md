# GitHub actions 简介

可以查看官网文档
[GitHub Actions 的基本功能](https://docs.github.com/zh/enterprise-server@3.11/actions/learn-github-actions/essential-features-of-github-actions)

这里记录一下使用GitHub actions遇到的问题 以及解决方案

在编写build-docs.yml后，触发GitHub的工作流 遇到了
```Bash
Error: Error message: Unable to get ACTIONS_ID_TOKEN_REQUEST_URL env variable

...
Ensure GITHUB_TOKEN has permission "id-token: write".

```

这样的问题。

解决方案：

参考：
[GitHub](https://github.com/actions/deploy-pages/issues/9)

在build-docs.yml 的内容头部加入权限相关配置即可。

例如：
```

permissions:
  contents: read
  pages: write
+ deployments: write
  id-token: write
...

  deploy:
    needs: build
    runs-on: ubuntu-latest

+   environment: 
+     name: github-pages
+     url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: 'Deploy to GitHub Pages'
+       id: deployment
        uses: actions/deploy-pages@v1-beta

```