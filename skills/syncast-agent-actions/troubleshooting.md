# Syncast Agent Action 异常处理

## `agent_not_initialized`

外部 Agent 还没有初始化身份。所有外部自动化操作前都必须先调用：

```ts
await window.__syncastAgent.initialize({
  name: "Project Operator Agent",
  description: "负责检查项目、委托内部 Agent，并验收生成结果。",
  emojiAvatar: "🤖",
});
```

## `not_in_project`

bridge 尚未绑定到已初始化的 Syncast 项目页。

请用户打开项目页面后重试：

```ts
await window.__syncastAgent.run("syncast.project.current");
```

## `doc_loading`

项目页面存在，但 Loro 仍在加载。等待片刻后重试。

## `sync_unsafe`

Loro Streams 初始同步状态不安全。不要强制执行 mutation，请用户先在应用中处理同步状态。

## `permission_denied`

当前用户没有编辑此项目的权限。读取类 action 仍可能可用；`agent.delegate`、`imagine.submit` 或 GraphQL mutation 等写入操作应停止。

## `timeout`

任务没有在等待窗口内完成。这不代表 Syncast 任务失败。

保存 `ref`，稍后调用：

```ts
await window.__syncastAgent.run("syncast.notifications.list", {
  filter: { ref }
});
```

或：

```ts
await window.__syncastAgent.run("syncast.task.result", { ref });
```

## 内部 Agent 失败

读取最终 assistant 消息和工具结果：

```ts
await window.__syncastAgent.run("syncast.agent.chat.result", { ref });
```

然后检查项目状态：

```ts
await window.__syncastAgent.run("syncast.project.inspect", {
  limit: 20
});
```
