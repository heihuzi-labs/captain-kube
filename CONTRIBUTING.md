# 参与船长 K8s

谢谢愿意来帮忙！修错字、补文档、报问题、写代码都欢迎。

## 动手之前

- 小改动（修 bug、改文档）直接提合并请求就行。
- 新功能先开个议题聊一下。船长 K8s 只管单个集群、给运维日常用，功能范围写在 [产品方案与需求基线](docs/产品方案与需求基线.md)，超出范围的想法也欢迎聊，但不一定会做。
- 安全问题别公开，按 [SECURITY.md](https://github.com/heihuzi-labs/.github/blob/main/SECURITY.md) 私下报告。

## 跑起来

需要 Node.js 22、Go（版本见 `server/go.mod`），以及一个能连的测试集群。没有集群的话，可以先用演示模式看界面。

```sh
cd web && npm ci && npm run dev        # 前端
cd server && go run ./cmd/kubejojo      # 后端
```

本地实验集群怎么搭、怎么拿测试用的 Token，见 [开发与实验集群操作指南](docs/operation-guide.md)。请用测试集群，别拿生产集群调试。

## 提交前跑这些

和自动检查跑的一样：

```sh
cd web && npm ci && npm run build
cd server && go test ./...
./scripts/build-release.sh             # 确认打包脚本还能跑通
```

改了界面请附截图，截图里别露出真实集群的地址、命名空间名和 Token。

## 合并请求

写清楚改了什么、为什么、怎么验证的，模板里都有。一个合并请求只做一件事。

提交的代码按 [MIT](LICENSE) 许可证发布。

---

## Contributing (English)

Thanks for helping! Typos, docs, bug reports and code are all welcome.

- Small fixes: open a pull request directly. New features: open an issue first; the scope (one cluster, for day-to-day ops) is in [the product baseline](docs/产品方案与需求基线.md) (in Chinese). Security problems: report privately, see [SECURITY.md](https://github.com/heihuzi-labs/.github/blob/main/SECURITY.md).
- Setup: Node.js 22, Go (see `server/go.mod`) and a test cluster, or demo mode to just look at the UI. `cd web && npm ci && npm run dev` for the frontend, `cd server && go run ./cmd/kubejojo` for the backend. See [the operation guide](docs/operation-guide.md) for a local lab cluster. Please don't debug against production.
- Before submitting (same as CI): `cd web && npm ci && npm run build`, `cd server && go test ./...`, `./scripts/build-release.sh`. Attach screenshots for UI changes, with no real cluster addresses, namespaces or tokens.
- Contributions are released under the [MIT](LICENSE) license.
