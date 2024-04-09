# Centos8安装yarn

Yarn 是一个 JavaScript 包管理器，它兼容于 npm，可以帮助你自动处理安装，升级，配置，和移除 npm 包。

它被创建，用于解决 npm 的一系列问题，例如通过并行操作提高软件包安装处理速度并且减少网络连接相关的错误。

这篇指南将会引导你在 CentOS 8 上进行 [Yarn](https://yarnpkg.com/) 的安装。

## 一、在 CentOS 8 上安装 Yarn

在 CentOS 8 上以 root 或者其他 sudo 用户身份执行下面步骤，安装 Yarn：

1. 如果你的系统上没有安装Node.js,先安装 Node.js 软件包，输入：

   ```bash
   sudo dnf install @nodejs
   ```

2. 启用 Yarn 软件源，并且导入源 GPG key：

   ```bash
   curl --silent --location https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo
   sudo rpm --import https://dl.yarnpkg.com/rpm/pubkey.gpg
   ```

3. 一旦软件源被启用，安装 Yarn：

   ```bash
   sudo dnf install yarn
   ```

4. 验证安装，打印 Yarn 版本号：

   ```bash
   yarn --version
   ```

   

