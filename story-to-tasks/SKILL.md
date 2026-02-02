---
name: story-to-tasks
description: Transform story documents into detailed, structured task documents. Use when the user asks to split, break down, or decompose a story document into individual tasks, or when they want to create task documents from a story's task section. Automatically generates task files in docs/task/[story-name]/ with standardized sections including technical objectives, prerequisites, categorized acceptance criteria (functional/performance/exception), implementation steps, task-type-specific content (document format specs, technology stack, data structures, pseudocode, flowcharts), risks & mitigation, and reference documentation.
---

# Story to Tasks

## Overview

This skill helps decompose story documents into individual, detailed task documents. It reads a story document (typically from `docs/story/`), identifies the task breakdown section, and generates structured task documents in `docs/task/<story-name>/` with comprehensive details needed for implementation.

Each task document includes:
- **前置依赖 (Prerequisites)**: Dependencies on other tasks
- **技术目标 (Technical Objective)**: Clear technical implementation goals
- **验收标准 (Acceptance Criteria)**: Categorized into functional, performance, and exception handling
- **执行步骤 (Implementation Steps)**: Detailed implementation guide
- **Task-specific sections**: Technology stack, data structures, pseudocode, flowcharts (based on task type)
- **风险与缓解 (Risks & Mitigation)**: Potential risks and mitigation strategies (optional)
- **参考文档 (References)**: Relevant documentation and resources (optional)

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
- 技术目标 (Technical Objective) - Focus on technical implementation goals
- 前置依赖 (Prerequisites) - If the task depends on other tasks
- 验收标准 (Acceptance Criteria) - Categorized into functional, performance, and exception scenarios
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

**Optional sections (add if applicable):**
- 风险与缓解 (Risks & Mitigation): Potential risks and mitigation strategies, or future optimization directions
- 参考文档 (References): API documentation, open-source repositories, or other reference materials

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

## 前置依赖 (Prerequisites)

[List any tasks that must be completed before this task can start. If none, state "无" or "None"]

- Task {task-id}: {brief description}
- ...

## 技术目标 (Technical Objective)

[Clear, concise statement of the technical implementation goals. Focus on what needs to be built/implemented from a technical perspective, such as:
- Implementing specific database CRUD operations
- Building a caching layer for a module
- Creating API endpoints with specific functionality
- Developing a data processing pipeline]

## 验收标准 (Acceptance Criteria)

### 功能性 (Functional)

- [ ] Functional criterion 1
- [ ] Functional criterion 2
- [ ] ...

### 性能 (Performance)

- [ ] Performance criterion 1 (e.g., response time < 100ms)
- [ ] Performance criterion 2 (e.g., throughput > 1000 req/s)
- [ ] ...

### 异常场景 (Exception Handling)

- [ ] Exception scenario 1 (e.g., handles network timeout gracefully)
- [ ] Exception scenario 2 (e.g., validates input and returns proper error)
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

### Optional Sections

#### Risks & Mitigation / Future Optimizations

Add this section when there are known risks, technical challenges, or areas for future improvement:

```markdown
## 风险与缓解 (Risks & Mitigation)

### 潜在风险 (Potential Risks)

1. **Risk 1**: [Description of the risk]
   - **影响**: [Impact if it occurs]
   - **缓解措施**: [How to mitigate or handle it]

2. **Risk 2**: [Description]
   - **影响**: [Impact]
   - **缓解措施**: [Mitigation strategy]

### 未来优化方向 (Future Optimizations)

- [Optimization opportunity 1]
- [Optimization opportunity 2]
- ...
```

#### Reference Documentation

Add this section when there are relevant external resources:

```markdown
## 参考文档 (References)

### API 文档 (API Documentation)

- [API Name](URL): [Brief description]
- ...

### 开源项目 (Open Source Projects)

- [Project Name](Repository URL): [What to reference from this project]
- ...

### 技术文章 (Technical Articles)

- [Article Title](URL): [Key points to reference]
- ...

### 其他资源 (Other Resources)

- [Resource Name](URL): [Description]
- ...
```

## Best Practices

### Task Granularity

- Each task should be completable in 1-3 days
- If a task is too large, consider breaking it into subtasks
- Tasks should have clear boundaries and minimal dependencies

### Writing Clear Objectives

Good technical objective:
> "Implement database CRUD operations for the User table with connection pooling, transaction support, and prepared statements to prevent SQL injection. Include methods for batch operations and pagination."

Poor objective:
> "Make the database work with users"

### Defining Measurable Acceptance Criteria

Good criteria organized by category:

**功能性 (Functional):**
- [ ] Can capture packets on 3+ interfaces simultaneously
- [ ] All packets written to single PCAP file with correct timestamps
- [ ] Per-interface statistics accurate within 1% margin

**性能 (Performance):**
- [ ] Packet processing latency < 10ms
- [ ] Memory usage < 100MB for 1M packets
- [ ] CPU usage < 50% on 4-core system

**异常场景 (Exception Handling):**
- [ ] Gracefully handles interface disconnection
- [ ] Recovers from disk full errors
- [ ] Validates all user inputs and returns clear error messages

Poor criteria:
- [ ] Multi-interface capture works
- [ ] Tests pass
- [ ] No crashes

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

### Writing Effective Prerequisites

Good prerequisites:
- Task 01: User authentication API (needed for session validation)
- Task 05: Database schema migration (required tables must exist)

Poor prerequisites:
- Task 01
- Previous task

### Categorizing Acceptance Criteria

**Functional criteria** should cover:
- Core features and capabilities
- Input/output correctness
- Integration points
- Test coverage requirements

**Performance criteria** should include:
- Response time / latency targets
- Throughput requirements
- Resource usage limits (CPU, memory, disk)
- Scalability targets

**Exception handling criteria** should cover:
- Error handling for expected failures
- Input validation
- Graceful degradation
- Recovery mechanisms
- Logging and monitoring

### Documenting Risks and Mitigations

Good risk documentation:
- Clearly states the risk
- Quantifies the impact
- Provides actionable mitigation strategies
- Distinguishes between current risks and future optimizations

Poor risk documentation:
- Vague concerns without specifics
- No mitigation strategies
- Mixing risks with general TODOs

### Selecting Relevant References

Include references that:
- Provide API documentation needed for implementation
- Show similar implementations to learn from
- Explain complex algorithms or patterns used
- Document third-party libraries or services

Don't include:
- Generic programming tutorials
- Unrelated documentation
- Internal wiki pages (unless accessible to all team members)

## Example Task Document

Here's an example of a well-structured task document for a code implementation task with complex logic:

```markdown
# Task: Multi-Interface Concurrent Packet Capture

> **Story**: 01-capture
> **Task ID**: task-03
> **Created**: 2026-01-31

## 前置依赖 (Prerequisites)

- Task 01: Basic packet capture interface definition
- Task 02: PCAP file writer implementation

## 技术目标 (Technical Objective)

Implement a concurrent packet capture system that can simultaneously capture packets from multiple network interfaces using goroutines. The system must aggregate all captured packets into a single PCAP file with proper synchronization, maintain per-interface statistics (packets, bytes, drops), and support graceful shutdown with context-based cancellation. The implementation should use Go's concurrency primitives (channels, sync.WaitGroup) and ensure thread-safe access to shared resources.

## 验收标准 (Acceptance Criteria)

### 功能性 (Functional)

- [ ] Can capture packets on multiple interfaces simultaneously (tested with 3+ interfaces)
- [ ] All interfaces' packets written to single PCAP file with correct format
- [ ] Per-interface statistics accurately tracked (packets, bytes, drops)
- [ ] Graceful shutdown on context cancellation or SIGINT/SIGTERM
- [ ] Interface filtering works correctly (include/exclude patterns)
- [ ] Unit test coverage > 80%

### 性能 (Performance)

- [ ] Can handle 10,000+ packets/second across all interfaces
- [ ] Packet processing latency < 10ms per packet
- [ ] Memory usage < 200MB for 1M packets
- [ ] No packet drops under normal load (< 5000 pps per interface)

### 异常场景 (Exception Handling)

- [ ] Handles interface disconnection gracefully (logs error, continues with other interfaces)
- [ ] Recovers from temporary write failures (retries with backoff)
- [ ] Validates interface names and returns clear error for invalid interfaces
- [ ] No goroutine leaks after shutdown (verified with runtime.NumGoroutine())
- [ ] No file handle leaks (verified with lsof)

## 执行步骤 (Implementation Steps)

1. Create MultiCapturer struct to manage multiple Capturer instances
2. Implement interface discovery and filtering logic
3. Create unified PCAP writer with thread-safe access (mutex-protected)
4. Implement goroutine pool for concurrent capture
5. Set up packet channel for aggregating packets from all interfaces
6. Implement statistics collection per interface with atomic operations
7. Add context-based cancellation support
8. Implement graceful shutdown and cleanup (close channels, wait for goroutines)
9. Write unit tests with mock interfaces
10. Write integration tests with real network interfaces
11. Add benchmarks for performance validation

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
    writerMu   sync.Mutex
    packetChan chan CapturedPacket
    statsChan  chan Statistics
    stats      map[string]*Statistics
    statsMu    sync.RWMutex
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

// Statistics holds per-interface capture statistics
type Statistics struct {
    Interface     string
    PacketsRecv   uint64
    PacketsDrop   uint64
    BytesRecv     uint64
    LastUpdate    time.Time
}
\`\`\`

### 关键方法说明

**NewMultiCapturer(config *Config) (*MultiCapturer, error)**
- **功能**: Create a new multi-interface capturer
- **参数**: config - Configuration including filter, output file, interface patterns
- **返回值**: MultiCapturer instance or error
- **注意事项**: Discovers interfaces matching patterns and creates individual capturers

**StartAll(ctx context.Context) error**
- **功能**: Start concurrent capture on all interfaces
- **参数**: ctx - Context for cancellation
- **返回值**: Error if any capturer fails to start
- **注意事项**: Launches goroutines for each interface and writer, blocks until context is cancelled

**GetAllStats() map[string]*Statistics**
- **功能**: Get statistics for all interfaces
- **返回值**: Map of interface name to statistics (copy, not reference)
- **注意事项**: Thread-safe, can be called during capture

**Stop() error**
- **功能**: Stop all capturers and cleanup resources
- **返回值**: Error if cleanup fails
- **注意事项**: Cancels context, waits for all goroutines, closes writer

## 伪代码 (Pseudocode)

\`\`\`
function StartAll(ctx):
    // Start writer goroutine
    spawn writerGoroutine():
        for packet in packetChan:
            lock writerMu
            write packet to file
            unlock writerMu

            update statistics atomically

            if ctx.Done():
                break

    // Start capturer goroutines
    for each capturer:
        spawn capturerGoroutine(capturer):
            while not ctx.Done():
                packet, err = capturer.NextPacket()

                if err != nil:
                    log error
                    if fatal error:
                        break
                    continue

                if packet != nil:
                    select:
                        case packetChan <- packet:
                            atomic increment interface stats
                        case <-ctx.Done():
                            break

    // Wait for context cancellation
    wait for ctx.Done()

    // Graceful shutdown
    close packetChan
    wait for all goroutines (wg.Wait)
    close writer
    return final statistics
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
        W2 -->|是| W3[加锁写入PCAP文件]
        W3 --> W4[更新统计]
        W4 --> W5{Context取消?}
        W5 -->|否| W1
        W5 -->|是| W6[关闭Writer]
    end

    subgraph Capturer Goroutines
        C1[Capturer.NextPacket] --> C2{捕获到包?}
        C2 -->|是| C3[发送到packetChan]
        C3 --> C4[原子更新接口统计]
        C4 --> C5{Context取消?}
        C2 -->|错误| C7{致命错误?}
        C7 -->|是| C6[清理资源]
        C7 -->|否| C8[记录日志]
        C8 --> C5
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
3. **统一写入**: Writer goroutine从channel接收包并加锁写入单个PCAP文件
4. **优雅退出**: Context取消时，所有goroutine停止工作并清理资源
5. **统计收集**: 每个接口维护独立的统计信息，使用原子操作更新，可随时查询
6. **错误处理**: 非致命错误记录日志后继续，致命错误触发清理退出

## 风险与缓解 (Risks & Mitigation)

### 潜在风险 (Potential Risks)

1. **高负载下的包丢失**
   - **影响**: 在高流量场景下（>10,000 pps），channel可能成为瓶颈导致丢包
   - **缓解措施**:
     - 使用带缓冲的channel（buffer size = 10000）
     - 实现背压机制，当channel满时记录drop统计
     - 考虑使用ring buffer替代channel

2. **文件写入性能瓶颈**
   - **影响**: 单个writer可能无法跟上多接口的写入速度
   - **缓解措施**:
     - 使用buffered writer减少系统调用
     - 批量写入packets（每100个或每10ms）
     - 监控writer goroutine的处理延迟

3. **内存占用过高**
   - **影响**: 长时间运行可能导致内存泄漏或OOM
   - **缓解措施**:
     - 及时释放packet buffers
     - 定期运行pprof检查内存使用
     - 实现packet数量或时间限制

### 未来优化方向 (Future Optimizations)

- 实现零拷贝packet处理，减少内存分配
- 支持多个writer并行写入不同文件
- 添加实时packet过滤，减少不必要的处理
- 实现adaptive buffer sizing based on load
- 支持packet去重功能

## 参考文档 (References)

### API 文档 (API Documentation)

- [gopacket Documentation](https://pkg.go.dev/github.com/google/gopacket): Core packet processing APIs
- [pcap Documentation](https://pkg.go.dev/github.com/google/gopacket/pcap): Packet capture interface

### 开源项目 (Open Source Projects)

- [tcpdump](https://github.com/the-tcpdump-group/tcpdump): Reference implementation for packet capture
- [Wireshark](https://gitlab.com/wireshark/wireshark): PCAP file format handling examples

### 技术文章 (Technical Articles)

- [Go Concurrency Patterns](https://go.dev/blog/pipelines): Pipeline and cancellation patterns
- [High-Performance Packet Processing](https://www.kernel.org/doc/Documentation/networking/packet_mmap.txt): Kernel-level optimization techniques

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
