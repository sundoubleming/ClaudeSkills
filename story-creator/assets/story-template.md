# Story-XX-[Story Name]

## 1. 用户故事 (User Story)

**As** [角色/用户类型]
**I want** [目标/需求]
**So that** [价值/原因]

---

## 2. 场景需求 (Scenario Requirements)

### 场景 1: [场景名称]
- **描述**: [场景的具体描述]
- **需求**: [该场景下的具体需求]

### 场景 2: [场景名称]
- **描述**: [场景的具体描述]
- **需求**: [该场景下的具体需求]

---

## 3. 系统需求 (System Requirements)

### 3.1 功能需求
- [功能需求1]
- [功能需求2]

### 3.2 非功能需求
- **性能**: [性能要求]
- **安全**: [安全要求]
- **可用性**: [可用性要求]

---

## 4. 系统架构 (System Architecture)

> 注：简单需求可省略此部分

### 4.1 架构概述
[系统架构的整体描述]

### 4.2 核心组件
- **组件1**: [组件描述]
- **组件2**: [组件描述]

### 4.3 技术栈
- **后端**: [技术选型]
- **前端**: [技术选型]
- **数据库**: [技术选型]

---

## 5. 对外接口 (External Interfaces)

### 5.1 API接口

#### 接口1: [接口名称]
- **方法**: GET/POST/PUT/DELETE
- **路径**: `/api/xxx`
- **描述**: [接口功能描述]
- **请求参数**:
  ```json
  {
    "param1": "value1"
  }
  ```
- **响应示例**:
  ```json
  {
    "status": "success",
    "data": {}
  }
  ```

### 5.2 CLI命令

#### 命令1: [命令名称]
```bash
command-name [options] [arguments]
```
- **描述**: [命令功能描述]
- **选项**:
  - `--option1`: [选项说明]

### 5.3 暴露的方法

#### 方法1: [方法名称]
```python
def method_name(param1, param2):
    """方法描述"""
    pass
```

---

## 6. 数据结构 (Data Structures)

### 6.1 数据库Schema

#### 表1: [表名]
```sql
CREATE TABLE table_name (
    id INT PRIMARY KEY AUTO_INCREMENT,
    field1 VARCHAR(255) NOT NULL,
    field2 TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**字段说明**:
- `id`: [字段说明]
- `field1`: [字段说明]

### 6.2 接口数据结构

#### 数据结构1: [结构名称]
```json
{
  "field1": "string",
  "field2": 123,
  "field3": {
    "nested_field": "value"
  }
}
```

**字段说明**:
- `field1`: [字段说明]
- `field2`: [字段说明]

---

## 7. 验收标准 (Acceptance Criteria)

### 7.1 功能性验收标准
- [ ] [功能验收标准1]
- [ ] [功能验收标准2]

### 7.2 性能验收标准
- [ ] [性能指标1]: [具体要求]
- [ ] [性能指标2]: [具体要求]

### 7.3 异常场景验收标准
- [ ] [异常场景1]: [预期行为]
- [ ] [异常场景2]: [预期行为]

---

## 8. Task拆分 (Task Breakdown)

### Task 1: [Task名称]
**目标**: [Task的主要目标]

**主要工作**:
- [工作项1]
- [工作项2]

**关键点**:
- [关键技术点或注意事项]

**依赖**: 无 / 依赖Task X

---

### Task 2: [Task名称]
**目标**: [Task的主要目标]

**主要工作**:
- [工作项1]
- [工作项2]

**关键点**:
- [关键技术点或注意事项]

**依赖**: 依赖Task 1

---

### Task 3: [Task名称]
**目标**: [Task的主要目标]

**主要工作**:
- [工作项1]
- [工作项2]

**关键点**:
- [关键技术点或注意事项]

**依赖**: 依赖Task 2
