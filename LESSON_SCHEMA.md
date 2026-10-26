# 课程 JSON Schema（每日追加一课）

主文件：`content/lessons.json`（构建时会复制到 `public/lessons.json`）。

## 顶层结构

```json
{
  "version": 1,
  "updatedAt": "2026-10-27T08:00:00+08:00",
  "title": "Agent 入门 · 零基础",
  "subtitle": "给完全零基础的你 · 每天一小课",
  "remoteContentUrl": null,
  "lessons": [ /* Lesson 数组 */ ]
}
```

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| version | number | 内容格式版本，目前为 1 |
| updatedAt | string | ISO 时间（建议 Asia/Shanghai） |
| title / subtitle | string | App 首页标题 |
| remoteContentUrl | string\|null | 文档备用；App 内也可在「进度」页配置远程 URL |
| lessons | Lesson[] | 课程列表，按 `day` 排序展示 |

## Lesson 对象

```json
{
  "id": "lesson-04",
  "day": 4,
  "title": "大模型在干什么",
  "subtitle": "先弄清大脑会什么、不会什么",
  "durationMin": 20,
  "difficulty": "入门",
  "tags": ["LLM", "大模型"],
  "goals": ["目标1", "目标2"],
  "concepts": [
    { "heading": "小标题", "body": "白话正文" }
  ],
  "compareTable": {
    "headers": ["列A", "列B", "列C"],
    "rows": [["r1c1", "r1c2", "r1c3"]]
  },
  "resources": [
    {
      "title": "资料名",
      "url": "https://...",
      "minutes": 10,
      "required": true,
      "note": "阅读提示"
    }
  ],
  "exercises": [
    { "title": "练习名", "steps": ["步骤1", "步骤2"] }
  ],
  "quiz": [
    {
      "q": "题干？",
      "options": ["A选项", "B选项", "C选项"],
      "answer": 1,
      "explain": "解析（提交后显示）"
    }
  ],
  "preview": "明日预告文案"
}
```

要点：

- `id` 全局唯一，建议 `lesson-XX`
- `answer` 是选项下标，从 **0** 开始
- `compareTable` 可选；没有就省略整个字段
- 追加新课时：**不要改已有课的 id**，只往 `lessons` 数组末尾 push，并更新 `updatedAt`

## 每日例行任务怎么写

1. 读 `content/lessons.json`
2. 生成下一课对象（day = 最大 day + 1）
3. `lessons.push(newLesson)`，写回文件
4. 运行 `npm run build && npx cap sync android && cd android && ./gradlew assembleDebug`
5. 把新 APK 或更新后的 `dist/` / 远程 JSON 发给她

若只更新内容、不重打包：把新的 `lessons.json` 托管到可访问 URL，她在 App「进度 → 课程更新」填入该 URL 即可拉取。
