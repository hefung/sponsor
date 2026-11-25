# sponsor.json 自动生成

每天生成 25 条随机赞助数据（纯 JSON 数组，日期在最近三个月内），自动提交回仓库。

## 文件

```
.github/workflows/sponsor.yml   → GitHub Actions（推荐）
.cnb.yml                        → CNB 流水线（可选）
.cnb/web_trigger.yml            → CNB 页面按钮（可选）
sponsor.json                    → 生成的数据
```

## GitHub 部署 3 步

1. `.github/workflows/sponsor.yml` 放到仓库对应目录。
2. 提交推送。
3. 仓库 **Settings → Actions → General → Workflow permissions** 选择 **Read and write permissions**。

## 触发

- 手动测试：仓库 **Actions → Refresh Sponsors → Run workflow**，选分支后执行。
- 自动：每天 **03:17 北京时间**（UTC 19:17）跑一次。

## 站点读取

GitHub raw 地址：

```
https://raw.githubusercontent.com/<owner>/<repo>/<branch>/sponsor.json
```

GitHub 返回 `Access-Control-Allow-Origin: *`，浏览器可以直接跨域 `fetch`，不需要代理。

## 可调参数

在 workflow 的 `env` 里：

```yaml
SPONSOR_COUNT: "25"         # 条数
SPONSOR_DAYS: "90"          # 日期回溯天数
SPONSOR_FILE: sponsor.json  # 输出路径
```

## 数据规则

- 姓名：单姓 / 姓+名 / 姓+`*` / 姓+`**` 随机
- 金额：5~1000 加权随机，小额为主
- 留言：真实赞助语气语料随机，偶尔为空
- 日期：最近 90 天内随机，按日期倒序
- 每次运行整批重新生成 25 条

## 说明

- 推送使用 `GITHUB_TOKEN`，需要 workflow 的 `contents: write` 权限。
- 提交信息带 `[skip ci]`，不会循环触发。
- 如果默认分支开了分支保护，`git push` 可能被拒绝，需要放行 Actions。
- CNB 的 raw 接口只允许 `https://docs.cnb.cool` 跨域，所以站点直连 CNB 会报 CORS；GitHub 没有这个问题。
