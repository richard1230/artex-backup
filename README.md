# ARTEX v0.3.14 离线备份

上游仓库 [Autumn-27/ARTEX](https://github.com/Autumn-27/ARTEX) 已于 2026-10 从 GitHub 删除（API 复核 404）。本仓库为本地孤本的完整备份，仅供自有环境恢复使用。

上游协议：**AGPL-3.0**（见源码包内 LICENSE）。修改后对外提供网络服务需公开源码；保留作者 Autumn-27 署名。

## 备份内容（Release 附件）

| 文件 | 大小 | 内容 |
|---|---|---|
| `artex-src-v0.3.14-20261008.tar.gz` | 13MB | 完整源码 + 全部 git 历史（HEAD=`86162d7` 定版 0.3.14）。已剔除 `.env` 与 `data/`，不含任何密钥/会话 |
| `artex-images-20261008.tar.gz` | 709MB | `docker save`：`autumn27/artex:latest`（1.67GB，含 chromium/nmap/playwright）+ `postgres:16-alpine`（294MB），gzip 校验通过 |

## 离线恢复流程

```bash
# 1. 导入镜像
docker load < artex-images-20261008.tar.gz

# 2. 解开源码，进入目录
tar xzf artex-src-v0.3.14-20261008.tar.gz && cd artex

# 3. 写 .env（模板见 .env.example），必填：
#    POSTGRES_PASSWORD=<强密码>
#    OPENAI_API_KEY=<你自己的 LLM key>
#    ARTEX_LLM_PROVIDER / ARTEX_LLM_MODEL / ARTEX_LLM_BASE_URL 按需
cp .env.example .env && vi .env

# 4. 启动
docker compose up -d
# UI: http://localhost:8787  账号 ARTEX（源码硬编码大写），首次访问设置密码
```

## 注意

- 备份不含任何任务数据与登录态（那些在部署机的 pgdata 数据卷里，不在本仓库）
- 镜像包不含 postgres 数据，恢复后是全新空库
