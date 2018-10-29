# zricethezav/gitleaks [![explain]][source] [![translate-svg]][translate-list]

<!-- [![size-img]][size] -->

[explain]: http://llever.com/explain.svg
[source]: https://github.com/chinanf-boy/Source-Explain
[translate-svg]: http://llever.com/translate.svg
[translate-list]: https://github.com/chinanf-boy/chinese-translate-list
[size-img]: https://packagephobia.now.sh/badge?p=Name
[size]: https://packagephobia.now.sh/result?p=Name

「 审核git存储库的密码 」

[中文](./readme.md) | [english](https://github.com/zricethezav/gitleaks)

---

## 校对 ✅

<!-- doc-templite START generated -->
<!-- repo = 'zricethezav/gitleaks' -->
<!-- commit = '03c53d297840ee6f6b7f82f1c94a2df8aa0d528c' -->
<!-- time = '2018-10-27' -->

| 翻译的原文 | 与日期        | 最新更新 | 更多                       |
| ---------- | ------------- | -------- | -------------------------- |
| [commit]   | ⏰ 2018-10-27 | ![last]  | [中文翻译][translate-list] |

[last]: https://img.shields.io/github/last-commit/zricethezav/gitleaks.svg
[commit]: https://github.com/zricethezav/gitleaks/tree/03c53d297840ee6f6b7f82f1c94a2df8aa0d528c

<!-- doc-templite END generated -->

### 贡献

欢迎 👏 勘误/校对/更新贡献 😊 [具体贡献请看](https://github.com/chinanf-boy/chinese-translate-list#贡献)

## 生活

[help me live , live need money 💰](https://github.com/chinanf-boy/live-need-money)

---

<p align="center">
  <img alt="gitleaks" src="https://raw.githubusercontent.com/zricethezav/gifs/master/gitleaks5.png" height="140" />
  <p align="center">
      <a href="https://travis-ci.org/zricethezav/gitleaks"><img alt="Travis" src="https://img.shields.io/travis/zricethezav/gitleaks/master.svg?style=flat-square"></a>
  </p>
</p>

## 审核git存储库的密码

Gitleaks为您提供了一种在git源代码库中，查找**未加密**的密码和其他**不需要**的数据类型的方法.

作为其核心功能的一部分,它提供了;

-   Github支持包括对批量组织和repo所有者(用户)存储库扫描,以及用于常见CI工作流的Pull Request的扫描.
-   支持私有存储库扫描，以及基于需要密钥的身份验证的存储库
-   以CSV和JSON格式输出，以供其他报告工具和框架使用
-   特定环境的可自定义的外部配置，包括正则表达式规则
-   可自定义的存储库名称,文件类型,提交ID,分支机构和正则表达式白名单,以减少误报
-   使用src-d的高性能[go-git](https://github.com/src-d/go-git)框架

它已经成功地用于许多不同的场景,包括;

-   通过文件系统路径或clone的URL，临时扫描本地和远程存储库
-   github用户和组织的自动扫描(公共和企业平台)
-   作为CICD工作流程的一部分,在密码进入代码库之前,识别密码
-   大环境git数据中，更常用的密码审计自动化功能的一部分

### 示例执行

<p align="left">
    <img src="https://cdn.rawgit.com/zricethezav/5bf8259b7fea0170becffc06b8588edb/raw/f762769fe20ef3669bff34612b1bede6457631e6/termtosvg_je8bp82s.svg">
</p>

#### 安装

Go语言实现,gitleaks当然可以用二进制形式提供,适用于许多流行的平台和操作系统类型，都在[发布页面](https://github.com/zricethezav/gitleaks).或者,通过Docker执行, 其可直接使用Go安装,如下所示;

##### Docker

```bash
# 运行 gitleaks 搞搞 公共存储库
docker run --rm --name=gitleaks zricethezav/gitleaks -v -r  https://github.com/zricethezav/gitleaks.git

# 运行 gitleaks 搞搞 已clone到 /tmp/ 的本地存储库
docker run --rm --name=gitleaks -v /tmp/:/code/  zricethezav/gitleaks -v --repo-path=/code/gitleaks

# 运行 gitleaks 搞搞 一个指定的 Github Pull request
docker run --rm --name=gitleaks -e GITHUB_TOKEN={your token} zricethezav/gitleaks --github-pr=https://github.com/owner/repo/pull/9000
```

##### Go

```bash
go get -u github.com/zricethezav/gitleaks
```

#### 用法和选项

gitleaks具有多种配置选项,可以在运行时，或根据您的特定要求，对配置文件进行调整.

```
Usage:
  gitleaks [OPTIONS]

Application Options:
  -r, --repo=          审核Repo url
      --github-user=   审核的Github用户
      --github-org=    审计的Github组织
      --github-url=    GitHub API基本URL，用于GitHub Enterprise。 例：https://github.example.com/api/v3/ (default: https://api.github.com/)
      --github-pr=     审核的Github PR url。 这不会clone存储库。 必须设置GITHUB_TOKEN
  -b, --branch=        要审核的分支名称（默认为HEAD）
  -c, --commit=        commit 的 sha码
      --depth=         最大提交深度
      --repo-path=     repo路径
      --owner-path=    所有者目录的路径（已发现的repos）
      --threads=       gitleaks产生的最大线程数
      --disk           克隆repo（s）到磁盘
      --all-refs       对所有refs运行审计
      --single-search= 单个正则表达式来搜索
      --config=        gitleaks配置的路径
      --ssh-key=       ssh-key的路径
      --exclude-forks  排除审核 组织/用户 的fork
  -e, --entropy=       在审计期间包括entropy-墒检查。 entropy量表：0.0（无entropy） - 8.0（最大entropy）
  -l, --log=           日志级别
  -v, --verbose        显示gitleaks审计的详细输出
      --report=        写报告文件的路径
      --redact         从日志消息和报告中，编辑秘密
      --version        版本号
      --sample-config  打印示例配置文件

Help Options:
  -h, --help           Show this help message
```

#### 退出代码

Gitleaks提供一致的现有代码,以协助自动化工作流程,如CICD平台和批量扫描.

这些可以与报告输出文件一起有效地使用,以检测有意义的数据，并将其返回给用户或外部系统,以确定是否已检测到泄漏,以及它们所在位置.

代码返回码是:

> `leaks` == 泄露-漏洞

```
0: no leaks
1: leaks present
2: error encountered
```

#### 附加信息

-   有关gitleaks函数如何如何，可在其[维基页面](https://github.com/zricethezav/gitleaks/wiki)上找到
-   以下链接详细介绍了，在git repos中修复不需要的数据的各种方法
    -   [从存储库中删除敏感数据(github.com)](https://help.github.com/articles/removing-sensitive-data-from-a-repository/)
    -   [从提交历史记录中删除敏感文件(atlassian.com)](https://community.atlassian.com/t5/Bitbucket-questions/Remove-sensitive-files-from-commit-history/qaq-p/243807)
    -   [使用BFG重写git历史记录(theguardian.com)](https://www.theguardian.com/info/developer-blog/2013/apr/29/rewrite-git-history-with-the-bfg)
-   [审核AWS中的Bitbucket服务器数据凭据(sourcedgroup.com)](https://www.sourcedgroup.com/blog/auditing-bitbucket-server-data-credentials-in-aws)

    这博文详细介绍了如何在AWS上托管时，使用gitleaks审计Atlassian Bitbucket服务器中的数据,并使用Splunk在规范仪表板中显示结果.

-   gitleaks与Github令牌扫描有何不同?
    -   [Github最近宣布](https://blog.github.com/2018-10-16-future-of-software/#github-token-scanning-for-public-repositories-public-beta)他们的云平台的新功能,可检测许多常见服务和平台的公开凭据,并自动通知提供商撤销或类似操作。而Gitleaks能为非Github云用户提供了类似的检测功能，其中可以轻松审核存储库 - 并以多种格式提供结果.
