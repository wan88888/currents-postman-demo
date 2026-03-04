# Currents Postman Demo

使用 Newman 运行 Postman 集合，并将测试结果上报到 [Currents.dev](https://currents.dev)。

## 环境准备

- 已通过 npm 将 **@currents/cmd** 和 **newman** 安装为开发依赖。

## 使用说明

### 1. 使用 JUnit 报告运行集合

```bash
npx newman run ./collection.json -r cli,junit --reporter-junit-export ./results.xml
```

或使用 npm 脚本：

```bash
npm run test:postman
```

若集合路径不同，请将 `./collection.json` 替换为你的 Postman 集合文件路径。

### 2. 将 JUnit 报告转换为 Currents 格式

```bash
npx currents convert --input-format=junit --input-file=./results.xml --framework=postman
```

或：

```bash
npm run test:currents:convert
```

### 3. 上传到 Currents

完成步骤 1、2 生成并转换结果后，执行上传：

```bash
npx currents upload --key=YOUR_RECORD_KEY --project-id=gnRUTN
```

或使用环境变量（推荐，避免 key 泄露）：

```bash
export CURRENTS_RECORD_KEY=YOUR_RECORD_KEY
npm run test:currents:upload
```

**请勿将 record key 写进代码或提交到仓库。** 在 CI 中请使用 secrets 配置 `CURRENTS_RECORD_KEY`。

### 一键执行

依次执行「运行集合 → 转换 → 上传」：

```bash
CURRENTS_RECORD_KEY=你的_record_key npm run test:currents
```

## CI 集成

在 CI 中将 `CURRENTS_RECORD_KEY` 配置为 secret，然后执行：

```bash
npm ci
npm run test:currents
```

如需更细粒度控制，也可在 CI 中分步执行上述命令。
