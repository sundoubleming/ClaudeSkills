# 流程图生成指南

## 何时需要流程图

为代码实现类 task 添加流程图，当满足以下条件之一时：

1. **复杂算法**: 包含多个条件分支、循环或递归逻辑
2. **多线程协作**: 涉及多个线程/goroutine/进程之间的交互
3. **状态机**: 包含明确的状态转换逻辑
4. **异步流程**: 包含异步操作、回调或事件驱动逻辑
5. **复杂业务流程**: 多个步骤之间有复杂的依赖关系

## Mermaid 流程图语法

使用 Mermaid 语法创建流程图，支持在 Markdown 中直接渲染。

### 基本语法

\`\`\`mermaid
graph TD
    A[开始] --> B{条件判断}
    B -->|是| C[执行操作1]
    B -->|否| D[执行操作2]
    C --> E[结束]
    D --> E
\`\`\`

### 节点类型

- `[矩形]` - 普通流程
- `(圆角矩形)` - 开始/结束
- `{菱形}` - 判断/条件
- `[[子程序]]` - 子流程
- `[(数据库)]` - 数据存储
- `((圆形))` - 连接点

### 箭头类型

- `-->` - 实线箭头
- `-.->` - 虚线箭头
- `==>` - 粗箭头
- `--文字-->` - 带标签的箭头

### 子图

\`\`\`mermaid
graph TD
    subgraph 主流程
        A --> B
    end
    subgraph 子流程
        C --> D
    end
    B --> C
\`\`\`

## 示例：多线程抓包流程

\`\`\`mermaid
graph TD
    Start((开始)) --> Init[初始化配置]
    Init --> Discover[发现所有网卡]
    Discover --> CreateCapturers[为每个网卡创建Capturer]

    CreateCapturers --> StartGoroutines[启动多个Goroutine]

    subgraph 并发抓包
        StartGoroutines --> G1[Goroutine 1: eth0]
        StartGoroutines --> G2[Goroutine 2: eth1]
        StartGoroutines --> G3[Goroutine N: ethN]

        G1 --> C1{捕获到包?}
        G2 --> C2{捕获到包?}
        G3 --> C3{捕获到包?}

        C1 -->|是| Send1[发送到Channel]
        C2 -->|是| Send2[发送到Channel]
        C3 -->|是| Send3[发送到Channel]

        C1 -->|超时| Check1{是否结束?}
        C2 -->|超时| Check2{是否结束?}
        C3 -->|超时| Check3{是否结束?}

        Check1 -->|否| G1
        Check2 -->|否| G2
        Check3 -->|否| G3
    end

    Send1 --> Writer[统一Writer Goroutine]
    Send2 --> Writer
    Send3 --> Writer

    Writer --> WriteFile[写入PCAP文件]
    WriteFile --> UpdateStats[更新统计信息]
    UpdateStats --> CheckLimit{达到限制?}

    CheckLimit -->|否| Writer
    CheckLimit -->|是| Stop[停止所有Capturer]

    Check1 -->|是| Stop
    Check2 -->|是| Stop
    Check3 -->|是| Stop

    Stop --> SaveMeta[保存元信息]
    SaveMeta --> End((结束))
\`\`\`

## 示例：状态机流程

\`\`\`mermaid
stateDiagram-v2
    [*] --> Idle
    Idle --> Capturing: Start()
    Capturing --> Paused: Pause()
    Paused --> Capturing: Resume()
    Capturing --> Stopping: Stop()
    Paused --> Stopping: Stop()
    Stopping --> Idle: Cleanup完成
    Idle --> [*]

    Capturing --> Error: 错误发生
    Error --> Idle: Reset()
\`\`\`

## 示例：时序图（多组件交互）

\`\`\`mermaid
sequenceDiagram
    participant CLI
    participant Manager
    participant Capturer1
    participant Capturer2
    participant Writer

    CLI->>Manager: Start(config)
    Manager->>Capturer1: Create & Start
    Manager->>Capturer2: Create & Start
    Manager->>Writer: Create Writer

    par 并发抓包
        Capturer1->>Writer: Send Packet
        Capturer2->>Writer: Send Packet
    end

    Writer->>Writer: Write to File

    CLI->>Manager: Stop()
    Manager->>Capturer1: Stop()
    Manager->>Capturer2: Stop()
    Manager->>Writer: Close()
    Writer-->>Manager: Stats
    Manager-->>CLI: Complete
\`\`\`

## 最佳实践

1. **保持简洁**: 流程图应该突出关键逻辑，避免过多细节
2. **使用子图**: 将复杂流程分解为多个子图
3. **标注清晰**: 为判断节点和箭头添加清晰的标签
4. **统一风格**: 在同一个项目中保持流程图风格一致
5. **适当抽象**: 不需要展示每一行代码，只展示关键步骤
6. **考虑读者**: 流程图应该帮助读者快速理解逻辑，而不是增加理解难度

## 何时不需要流程图

- 简单的 CRUD 操作
- 线性的顺序执行流程（可以用列表代替）
- 非常简单的条件判断（1-2个分支）
- 纯数据结构定义（没有复杂逻辑）
