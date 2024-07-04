# 浅尝WebAI

## Gemini Nano in Chrome

### 必备环境
- Chrome Canary or Chrome dev channel， 版本 >= 128.0.6545.0
- 系统要求：
  - 系统版本：win10/11，MacOS≥13(Ventura)
  - 磁盘空间：≥ 22GB
  - GPU：集显/独显
  - 显存：≥ 4GB

### 配置开启
- chrome://flags/#optimization-guide-on-device-model，设置为：`Enabled BypassPerfRequirement`
- chrome://flags/#prompt-api-for-gemini-nano，设置为：`Enabled`
- 重启Chrome
- 打开DevTools，逐步输入以下代码
  ```javascript
  // 第一次输入大概率返回 'no'
  await window.ai.canCreateTextSession();
  // 直接创建session，告诉他我要用
  window.ai.createTextSession();
  // 再次执行，应该就返回 'after-download'
  await window.ai.canCreateTextSession();
  ```
- 打开 chrome://components/ 标签页，找到 `Optimization Guide On Device Model` 点击更新。等待模型下载...
- 模型下载完后，打开DevTools查看是否可用
  ```javascript
  // 如果返回'readily'，表示可以用啦～
  await window.ai.canCreateTextSession();

  // 尽情享受吧
  const session = await window.ai.createTextSession();
  await session.prompt('你好');
  ```

### API简述
```typescript
declare global {
  interface Window {
    ai: {
      canCreateTextSession: () => Promise<AIModelAvailability>;
      createTextSession: () => Promise<AITextSession>;
      defaultTextSessionOptions: () => Promise<AITextSessionOptions>;
    }
  }
}

enum AIModelAvailability {
  "readily",
  "after-download",
  "no"
};

interface AITextSessionOptions {
  topK: number;
  temperature: number;
};

interface AITextSession {
  prompt: (input: string) => Promise<string>;
  promptStreaming: (input: string) => Promise<ReadableStream>;
  destroy: () => void;
  clone(): AITextSession; // Not yet implemented (see persistence and cloning for context)
};
```

## 应用场景
### ASR + LLM + TTS 打造纯web AI语音对话

#### ASR wasm
// todo

#### TTS wasm
// todo

#### MVP Demo
// todo