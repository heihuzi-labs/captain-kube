<div align="center">

# 船长 K8s · Captain Kube

一个给运维用的 Kubernetes 单集群管理台：看全局、查问题、改配置，都在一个页面里。

简体中文 · [English](README.en.md)

</div>

![集群总览](docs/assets/readme/overview.jpg)

## 这是什么

管一个 Kubernetes 集群，平时要在 kubectl、各种面板和日志工具之间来回切：先看哪里红了，再找是哪个 Pod，翻事件、看日志、进容器，最后改 YAML。船长 K8s 把这一串放到了一起。

- 打开就是集群全貌，资源之间的关系画成拓扑图，出问题的地方直接标出来。
- 点进一个 Pod，状态、事件、日志、YAML、关联资源和终端都在同一页，不用换工具。
- 大多数资源都能直接看、改、建、删 YAML，常用的扩缩容、重启、暂停一键就行。
- 用集群自己的 ServiceAccount Token 登录，权限跟着 RBAC 走，不另搞一套账号。
- 发布时前端打进后端，一个二进制就能跑；还能在页面里检查新版本、升级和回滚。

它只管一个集群，不做多集群。

> 以前叫 kubejojo。程序名、环境变量现在还沿用 `kubejojo`，后面的版本会统一改名。

## 看一眼

<table>
<tr>
<td width="50%"><img src="docs/assets/readme/topology-demo.gif" alt="资源拓扑"><br><sub>资源拓扑：工作负载、网络、存储画在一张关系图里，异常直接标红</sub></td>
<td width="50%"><img src="docs/assets/readme/pod-debug-demo.gif" alt="Pod 排障"><br><sub>Pod 排障：日志、事件、终端在同一条路径上</sub></td>
</tr>
<tr>
<td><img src="docs/assets/readme/pods.jpg" alt="Pod 列表"><br><sub>Pod 列表：命名空间、健康状态、资源用量一眼看全</sub></td>
<td><img src="docs/assets/readme/deployment-detail.jpg" alt="Deployment 详情"><br><sub>Deployment 详情：匹配的 Pod、事件和常用操作</sub></td>
</tr>
<tr>
<td><img src="docs/assets/readme/serviceaccounts.jpg" alt="ServiceAccounts"><br><sub>权限：按 ServiceAccount、Role、Binding 查看和编辑</sub></td>
<td><img src="docs/assets/readme/system-updates.jpg" alt="更新管理"><br><sub>更新管理：检查新版本、安装、回滚、重启服务</sub></td>
</tr>
</table>

## 装起来

开发时前后端分开跑。后端需要 Go，前端需要 Node.js。

```bash
# 后端，默认 http://127.0.0.1:8080
cd server
export KUBEJOJO_KUBECONFIG=/path/to/your/kubeconfig
go run ./cmd/kubejojo

# 前端，默认 http://127.0.0.1:5174，会把 /api 转给后端
cd web
npm install
npm run dev
```

后端按 `KUBEJOJO_KUBECONFIG`、`KUBECONFIG`、`~/.kube/config` 的顺序找集群配置。登录真实集群要输入 ServiceAccount Token；只想看看界面，可以用演示模式。

实验环境可以这样拿一个管理员 Token（正式环境请按最小权限绑定，别直接用 `cluster-admin`）：

```bash
kubectl create serviceaccount kubejojo-dev -n kube-system
kubectl create clusterrolebinding kubejojo-dev \
  --clusterrole=cluster-admin \
  --serviceaccount=kube-system:kubejojo-dev
kubectl create token kubejojo-dev -n kube-system
```

正式部署用发布版：到 [Releases](https://github.com/heihuzi-labs/captain-kube/releases) 下载对应平台的包（Linux amd64/arm64、macOS arm64），里面是一个内嵌了前端的二进制和一个 systemd 服务文件。也可以自己打包：

```bash
./scripts/build-release.sh                              # 当前平台
GOOS=linux GOARCH=arm64 ./scripts/build-release.sh      # 指定平台
```

产物在 `server/dist/release/`。

## 在线更新

打开在线更新后，系统管理页里能检查新版本、安装、回滚和重启服务。

| 环境变量 | 作用 |
| --- | --- |
| `KUBEJOJO_UPDATE_ENABLED` | 打开在线更新入口 |
| `KUBEJOJO_UPDATE_REPOSITORY` | 从哪个仓库的 Releases 取新版本，默认 `heihuzicity-tech/kubejojo`（已自动跳转到本仓库） |
| `KUBEJOJO_UPDATE_ALLOWED_SUBJECTS` | 谁能执行更新、回滚、重启，填 Kubernetes 身份，逗号分隔 |
| `KUBEJOJO_UPDATE_ALLOW_PRERELEASES` | 要不要检测 rc、beta 这类预发布版本 |
| `KUBEJOJO_UPDATE_GITHUB_TOKEN` | 可选，访问 GitHub 接口更稳、限流更宽 |
| `KUBEJOJO_UPDATE_TARGET_PATH` | 可选，指定被管理的二进制路径 |

## 开发

```text
server/   Go 后端：client-go、更新服务、内嵌前端
web/      React 前端：Ant Design、TanStack Query
scripts/  发布打包脚本
docs/     产品边界、操作指南、README 截图
```

打 `v*` 标签会触发发布：跑前端构建和后端 `go test ./...`，产出三个平台的包和 `checksums.txt`，自动发 GitHub Release。

完整的功能范围见 [产品方案与需求基线](docs/产品方案与需求基线.md)，本地实验集群的操作见 [开发与实验集群操作指南](docs/operation-guide.md)。

## 许可证

[MIT](LICENSE)。Kubernetes 是 Linux 基金会的商标，本项目与其没有关联。

---

<sub>船长系列，来自 [heihuzi-labs](https://github.com/heihuzi-labs)：[船长派活](https://github.com/heihuzi-labs/captain-agents) · **船长 K8s** · [船长运维](https://github.com/heihuzi-labs/captain-ops) · [船长密码箱](https://github.com/heihuzi-labs/captain-password) · [船长待办](https://github.com/heihuzi-labs/captain-todo)</sub>
