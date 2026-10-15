# monetr - LazyCat App

[monetr](https://github.com/monetr/monetr)（预算管理应用，专注周期性支出规划）的懒猫微服（LazyCat）打包。

## 模式

- **镜像模式（mirror）**：`ghcr.io/monetr/monetr` 镜像经 `ghcr.1ms.run` 加速器拉取，`require_digest_match: true` 校验镜像内容与上游一致；postgres/valkey 固定 `docker.1ms.run`。
- **自动跟随上游**：每日检查上游 semver tag（无 `v` 前缀，`channel: stable`），发现新版本自动更新版本号与镜像、构建 LPK、发布商店。
- **喵喵商店发布**：仅私有商店（MiaoMiao private store），不发布官方商店。

## 配置

- 三服务：`monetr` + `postgres:17` + `valkey:9`（与上游 compose 一致）
- 数据卷：`/lzcapp/var/monetr:/etc/monetr`（证书/配置）+ `/lzcapp/var/postgres`（数据库）
- `MONETR_SERVER_EXTERNAL_URL` 自动指向子域名（Cookie/邮件链接依赖它）
- PG 密码 `stable_secret` 自动生成
- 首次打开注册账号（`MONETR_ALLOW_SIGN_UP=true`）；注册完建议在 UI 关闭注册
- 银行同步（Plaid 等）需自行配置 API 凭据，国内用户可纯手动预算

## 所需 Secrets

| Secret | 说明 |
| --- | --- |
| `APPSTORE_URL` / `APPSTORE_TOKEN` | 喵喵商店 API 地址与发布令牌 |
| `APP_ID` | 可选 |
| `PRIVATE_STORE_GROUP_CODES` | 可选，私有分组码 |

## 许可

上游 LICENSE.md（自定义源码可用许可，非标准 OSI 模板）
