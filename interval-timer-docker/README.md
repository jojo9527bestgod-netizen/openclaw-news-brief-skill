# HIIT Interval Timer (Docker)

默认训练方案：**50秒训练 + 20秒休息，共5组**。

## 启动

```bash
cd interval-timer-docker
docker compose up -d --build
```

打开：<http://localhost:8088>

## 使用

- 可在页面上修改训练秒数、休息秒数、组数
- 点击“开始/暂停/重置”控制
- 每段结束会有提示音

## 停止

```bash
docker compose down
```
