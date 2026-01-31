---
name: story-to-tasks  
description: Transform story documents into detailed, structured task documents. Use when the user asks to split, break down, or decompose a story document into individual tasks, or when they want to create task documents from a story's task section. Automatically generates task files in docs/task/[story-name]/ with standardized sections including objectives, acceptance criteria, implementation steps, and task-type-specific content (document format specs, technology stack, data structures, pseudocode, and flowcharts for complex logic).
---

# Story to Tasks

## Overview

This skill helps decompose story documents into individual, detailed task documents. It reads a story document (typically from `docs/story/`), identifies the task breakdown section, and generates structured task documents in `docs/task/<story-name>/` with comprehensive details needed for implementation.

**IMPORTANT**: All generated task documents MUST be in Markdown (.md) format.

## Workflow

### Step 1: Read and Analyze the Story Document

Read the story document provided by the user. Look for:

1. **Story metadata**: Story name, priority, module overview
2. **Task breakdown section**: Usually titled "Task 拆分", "Tasks", or similar
3. **Task details**: Each task should have a title and description
4. **Technical context**: Technology stack, data structures, interfaces defined in the story

Example story structure:
```markdown
# Story: Module Name

## 模块概述
[Overview content]

## Task 拆分

### Task 1: Task Title
**目标**: [Objective]
**子任务**: [Subtasks]
**验收标准**: [Acceptance criteria]
```

### Step 2: Determine Task Type and Required Sections

For each task, determine its type and what sections are needed:

**All tasks require:**
- 基本目标 (Task Objective)
- 验收标准 (Acceptance Criteria)
- 执行步骤 (Implementation Steps)

**Document output tasks additionally require:**
- 文档格式 (Document Format): Specify the structure, sections, and format requirements

**Code implementation tasks additionally require:**
- 技术栈 (Technology Stack): Programming language, frameworks, libraries
- 核心数据结构和方法 (Data Structures & Methods): Key classes, interfaces, functions
- 伪代码 (Pseudocode): High-level code examples showing the approach

**Complex code tasks (algorithms, multi-threading, state machines) additionally require:**
- 流程图 (Flowchart): Mermaid diagram showing the logic flow
  - See `references/flowchart_guide.md` for when and how to create flowcharts

### Step 3: Create Task Directory Structure

Create the directory structure:
```
docs/task/<story-name>/
├── task-01-<task-name>.md
├── task-02-<task-name>.md
└── ...
```

Where:
- `<story-name>` is derived from the story filename (e.g., "01-capture" from "01-capture.md")
- Task files are numbered sequentially
- Task names are kebab-case versions of the task title
- **All task files MUST use .md extension (Markdown format)**

### Step 4: Generate Task Documents

For each task, create a Markdown document using this structure:

**IMPORTANT**: All task documents MUST be in Markdown (.md) format with proper Markdown syntax.

```markdown
# Task: {Task Title}

> **Story**: {Story Name}
> **Task ID**: {task-number}
> **Created**: {current-date}

## 基本目标 (Task Objective)

[Clear, concise statement of what this task aims to achieve]

## 验收标准 (Acceptance Criteria)

[Specific, measurable criteria that define task completion]

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] ...

## 执行步骤 (Implementation Steps)

[Detailed step-by-step instructions for implementing this task]

1. Step 1
2. Step 2
3. ...

[OPTIONAL SECTIONS BASED ON TASK TYPE - see below]

---

**Status**: Pending
**Assignee**: TBD
**Estimated Effort**: TBD
```

### Step 5: Add Task-Type-Specific Sections

#### For Document Output Tasks

Add after "执行步骤":

```markdown
## 文档格式 (Document Format)

### 文档结构

[Specify the required sections and organization]

### 格式要求

- **文件格式**: [e.g., Markdown, YAML, JSON]
- **命名规范**: [File naming conventions]
- **内容要求**: [What must be included]

### 示例

[Provide a concrete example or template]
```

#### For Code Implementation Tasks

Add after "执行步骤":

```markdown
## 技术栈 (Technology Stack)

- **编程语言**: [e.g., Python 3.10+, Go 1.21+]
- **核心库/框架**: [e.g., gopacket, pyroute2]
- **依赖项**: [Other dependencies]
- **开发工具**: [Build tools, testing frameworks]

## 核心数据结构和方法 (Data Structures & Methods)

### 主要类/接口

\`\`\`[language]
// Core interface or class definitions
// Include key methods and their signatures
\`\`\`

### 关键方法说明

**MethodName(params)**
- **功能**: [What it does]
- **参数**: [Parameter descriptions]
- **返回值**: [Return value description]
- **注意事项**: [Important considerations]

## 伪代码 (Pseudocode)

\`\`\`
// High-level pseudocode showing the approach
function mainLogic():
    initialize resources
    for each item:
        process item
        handle errors
    cleanup and return
\`\`\`
```

#### For Complex Code Tasks (Add Flowchart)

When the task involves:
- Complex algorithms with multiple branches
- Multi-threading or concurrent operations
- State machines
- Asynchronous workflows
- Complex business logic with dependencies

Add after "技术栈" or "核心数据结构和方法":

```markdown
## 流程图 (Flowchart)

\`\`\`mermaid
graph TD
    Start((开始)) --> Step1[步骤1]
    Step1 --> Decision{判断条件}
    Decision -->|是| Step2[步骤2]
    Decision -->|否| Step3[步骤3]
    Step2 --> End((结束))
    Step3 --> End
\`\`\`

### 流程说明

[Explain the key decision points and flow logic]
```

Refer to `references/flowchart_guide.md` for detailed guidance on creating effective flowcharts.

## Best Practices

### Task Granularity

- Each task should be completable in 1-3 days
- If a task is too large, consider breaking it into subtasks
- Tasks should have clear boundaries and minimal dependencies

### Writing Clear Objectives

Good objective:
> "Implement multi-interface packet capture with concurrent goroutines, unified output to a single PCAP file, and per-interface statistics tracking"

Poor objective:
> "Make the capture work with multiple interfaces"

### Defining Measurable Acceptance Criteria

Good criteria:
- [ ] Can capture packets on 3+ interfaces simultaneously
- [ ] All packets written to single PCAP file with correct timestamps
- [ ] Per-interface statistics accurate within 1% margin
- [ ] Unit test coverage > 80%

Poor criteria:
- [ ] Multi-interface capture works
- [ ] Tests pass

### Writing Effective Implementation Steps

- Start with setup/initialization
- Include verification steps after key operations
- Mention error handling explicitly
- End with testing and validation

### Choosing the Right Level of Detail

**Data Structures**: Include enough detail to guide implementation but not full code
- ✅ Show interface signatures, key fields, important methods
- ❌ Don't include every getter/setter or trivial method

**Pseudocode**: Focus on the algorithm/logic, not syntax
- ✅ Show the approach, key operations, control flow
- ❌ Don't write actual compilable code

**Flowcharts**: Show the big picture and decision points
- ✅ Highlight parallel operations, state transitions, error paths
- ❌ Don't diagram every single line of code

## Example Task Document

Here's an example of a well-structured task document for a code implementation task with complex logic:

```markdown
# Task: Multi-Interface Concurrent Packet Capture

> **Story**: 01-capture
> **Task ID**: task-03
> **Created**: 2026-01-31

## 基本目标 (Task Objective)

Implement concurrent packet capture across all network interfaces, with unified output to a single PCAP file and per-interface statistics tracking. Support graceful shutdown and resource cleanup.

## 验收标准 (Acceptance Criteria)

- [ ] Can capture packets on multiple interfaces simultaneously
- [ ] All interfaces' packets written to single PCAP file
- [ ] Per-interface statistics accurate (packets, bytes, drops)
- [ ] Graceful shutdown on context cancellation or signal
- [ ] No resource leaks (goroutines, file handles)
- [ ] Unit test coverage > 80%

## 执行步骤 (Implementation Steps)

1. Create MultiCapturer struct to manage multiple Capturer instances
2. Implement interface discovery and filtering logic
3. Create unified PCAP writer with thread-safe access
4. Implement goroutine pool for concurrent capture
5. Set up packet channel for aggregating packets from all interfaces
6. Implement statistics collection per interface
7. Add context-based cancellation support
8. Implement graceful shutdown and cleanup
9. Write unit tests with mock interfaces
10. Write integration tests with real network interfaces

## 技术栈 (Technology Stack)

- **编程语言**: Go 1.21+
- **核心库/框架**:
  - github.com/google/gopacket - Packet processing
  - github.com/google/gopacket/pcap - Packet capture
  - github.com/google/gopacket/pcapgo - PCAP file writing
- **依赖项**: libpcap-dev (system dependency)
- **开发工具**: Go test framework, gomock for mocking

## 核心数据结构和方法 (Data Structures & Methods)

### 主要接口

\`\`\`go
// MultiCapturer manages concurrent packet capture on multiple interfaces
type MultiCapturer struct {
    capturers  []Capturer
    writer     *pcapgo.Writer
    packetChan chan CapturedPacket
    statsChan  chan Statistics
    ctx        context.Context
    cancel     context.CancelFunc
    wg         sync.WaitGroup
}

// CapturedPacket represents a packet with its source interface
type CapturedPacket struct {
    Interface string
    Packet    gopacket.Packet
    Timestamp time.Time
}
\`\`\`

### 关键方法说明

**NewMultiCapturer(config *Config) (*MultiCapturer, error)**
- **功能**: Create a new multi-interface capturer
- **参数**: config - Configuration including filter, output file, etc.
- **返回值**: MultiCapturer instance or error
- **注意事项**: Discovers interfaces and creates individual capturers

**StartAll(ctx context.Context) error**
- **功能**: Start concurrent capture on all interfaces
- **参数**: ctx - Context for cancellation
- **返回值**: Error if any capturer fails to start
- **注意事项**: Launches goroutines for each interface and writer

**GetAllStats() map[string]*Statistics**
- **功能**: Get statistics for all interfaces
- **返回值**: Map of interface name to statistics
- **注意事项**: Thread-safe, can be called during capture

## 伪代码 (Pseudocode)

\`\`\`
function StartAll(ctx):
    // Start writer goroutine
    spawn writerGoroutine():
        for packet in packetChan:
            write packet to file
            update statistics
            if ctx.Done():
                break

    // Start capturer goroutines
    for each capturer:
        spawn capturerGoroutine(capturer):
            while not ctx.Done():
                packet = capturer.NextPacket()
                if packet != nil:
                    send packet to packetChan
                    update per-interface stats

    // Wait for context cancellation
    wait for ctx.Done()

    // Graceful shutdown
    close packetChan
    wait for all goroutines to finish
    close writer
    return statistics
\`\`\`

## 流程图 (Flowchart)

\`\`\`mermaid
graph TD
    Start((开始)) --> Init[初始化MultiCapturer]
    Init --> Discover[发现所有网卡]
    Discover --> CreateCapturers[为每个网卡创建Capturer]
    CreateCapturers --> CreateWriter[创建统一Writer]
    CreateWriter --> StartWriter[启动Writer Goroutine]

    StartWriter --> StartCapturers[启动Capturer Goroutines]

    subgraph Writer Goroutine
        W1[等待packetChan] --> W2{收到包?}
        W2 -->|是| W3[写入PCAP文件]
        W3 --> W4[更新统计]
        W4 --> W5{Context取消?}
        W5 -->|否| W1
        W5 -->|是| W6[关闭Writer]
    end

    subgraph Capturer Goroutines
        C1[Capturer.NextPacket] --> C2{捕获到包?}
        C2 -->|是| C3[发送到packetChan]
        C3 --> C4[更新接口统计]
        C4 --> C5{Context取消?}
        C2 -->|超时| C5
        C5 -->|否| C1
        C5 -->|是| C6[清理资源]
    end

    StartCapturers --> C1
    C3 --> W1

    W6 --> Wait[等待所有Goroutine]
    C6 --> Wait
    Wait --> Cleanup[清理资源]
    Cleanup --> End((结束))
\`\`\`

### 流程说明

1. **初始化阶段**: 发现网卡并为每个网卡创建独立的Capturer
2. **并发抓包**: 每个Capturer在独立的goroutine中运行，捕获的包发送到共享channel
3. **统一写入**: Writer goroutine从channel接收包并写入单个PCAP文件
4. **优雅退出**: Context取消时，所有goroutine停止工作并清理资源
5. **统计收集**: 每个接口维护独立的统计信息，可随时查询

---

**Status**: Pending
**Assignee**: TBD
**Estimated Effort**: 2-3 days
```

## Resources

### references/flowchart_guide.md

Comprehensive guide on when and how to create flowcharts using Mermaid syntax. Includes:
- Criteria for when flowcharts are needed
- Mermaid syntax reference
- Examples for different scenarios (multi-threading, state machines, sequences)
- Best practices for effective flowcharts

Load this reference when you need to create flowcharts for complex tasks.

### assets/task_template.md

Basic template structure for task documents. Use this as a starting point and customize based on task type.
