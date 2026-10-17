# dsh-market-index

DSH 插件市场索引（**plugin market index format 1**），由社区公开数据自动转换生成，供 DeepSeek Harness 桌面端「插件 → 发现」页使用。

## 在 DSH 里怎么用

插件页 → **发现** tab → **添加索引源**，填：

```
https://cdn.jsdelivr.net/gh/tang-coder-hub/dsh-market-index@main/index.json
```

首次读取约 1.8MB（内网约 6~11 秒），成功后缓存 10 分钟。

## 内容与口径

- **3215 个插件**（hub.omdsh.dev 精选目录 8663 ∩ dshfind.com 已 npm 发布，去重后），7 个分类、7 个标签、按 Star 排序。
- 版本为生成当天经 jsDelivr data API 核对的 npm `latest`（生成时约 45% 的社区探测数据已过时，故必须核对）。
- `publishTimestamp` 用社区爬虫的验证时间近似（格式合法，非精确发布时间）。
- ⚠️ 全部为**第三方内容**：索引与插件的可信度由来源社区保证，安装后以你的权限运行，请自行斟酌。

## 刷新方法

生成管线在 `D:\deepseekagent\_fw_tools\`（本机）：

```bash
# 1. 拉 dshfind 全量 + 与 hub join（约 9 分钟）
python _fw_tools/build_market_index.py
# 2. 转换为 format 1（jsDelivr 并发核对 latest，约 14 分钟）
python _fw_tools/convert_market_index.py
# 3. 用宿主官方验证器校验（从 app.asar 提取的 @deepseek-ai/dsh-plugin-market-index）
python _fw_tools/validate_market_index.py
# 4. 发布（经典 token，repo scope）
python _fw_tools/publish_market_index.py <GITHUB_TOKEN> market_index_v1.json tang-coder-hub/dsh-market-index index.json
```

数据来源：hub.omdsh.dev/marketplace.json + api.dshfind.com（社区目录，非官方）。
