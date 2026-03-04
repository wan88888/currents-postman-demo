# Currents Postman Demo

Run Postman collections with Newman and report results to [Currents.dev](https://currents.dev).

## Setup

- **@currents/cmd** and **newman** are installed as development dependencies (npm).

## Usage

### 1. Run collection with JUnit reporter

```bash
npx newman run ./collection.json -r cli,junit --reporter-junit-export ./results.xml
```

Or use the npm script:

```bash
npm run test:postman
```

Replace `./collection.json` with your Postman collection path if different.

### 2. Convert JUnit to Currents format

```bash
npx currents convert --input-format=junit --input-file=./results.xml --framework=postman
```

Or:

```bash
npm run test:currents:convert
```

### 3. Upload to Currents

上传到 Currents 只需执行（先完成步骤 1、2 生成并转换结果后）：

```bash
npx currents upload --key=YOUR_RECORD_KEY --project-id=gnRUTN
```

或使用环境变量（推荐，避免 key 泄露）：

```bash
export CURRENTS_RECORD_KEY=YOUR_RECORD_KEY
npm run test:currents:upload
```

**请勿将 record key 写进代码或提交到仓库。** 在 CI 中用 secrets 配置 `CURRENTS_RECORD_KEY`。

### All-in-one

Run collection, convert, and upload in one go:

```bash
CURRENTS_RECORD_KEY=your_record_key npm run test:currents
```

## CI

In CI, set `CURRENTS_RECORD_KEY` as a secret and run:

```bash
npm ci
npm run test:currents
```

Or run the steps separately if you need more control.
