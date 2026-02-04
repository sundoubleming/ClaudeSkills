---
name: story-creator
description: "Create standardized story documents for software requirements. Use when the user wants to create a new story document, document a feature requirement, or structure a software requirement specification. Triggers include requests like 'create a story for...', 'document this requirement', 'write a story about...', or when the user describes a feature/requirement that needs to be formally documented. Generates story files in docs/story/ with naming format Story-XX-[name].md containing 8 sections: user story, scenario requirements, system requirements, system architecture, external interfaces, data structures, acceptance criteria, and task breakdown."
---

# Story Creator

Create standardized story documents that capture software requirements from user perspective through to technical implementation and task breakdown.

## Overview

This skill generates structured story documents following a standardized 8-section format:

1. **User Story** - As/I want/So that format
2. **Scenario Requirements** - Specific scenarios and their needs
3. **System Requirements** - Functional and non-functional requirements
4. **System Architecture** - High-level design (optional for simple requirements)
5. **External Interfaces** - APIs, CLI commands, or exposed methods
6. **Data Structures** - Database schemas and interface data formats
7. **Acceptance Criteria** - Functional, performance, and exception scenarios
8. **Task Breakdown** - Decomposition into implementable tasks

Story documents are created in `docs/story/` with naming format `Story-XX-[name].md` where XX is a sequential number.

## Workflow

### Step 1: Understand the Requirement

Gather information about the feature or requirement:

- What problem does this solve?
- Who are the users?
- What are the key scenarios?
- Are there specific technical constraints?
- What are the success criteria?

If the user provides a brief description, ask clarifying questions to understand:
- The user's goals and motivations
- Specific use cases or scenarios
- Expected behavior in different situations
- Performance or quality expectations

### Step 2: Determine Story Number

Check existing story files to determine the next sequential number:

```bash
ls docs/story/ | grep -E "^Story-[0-9]+" | sort -V | tail -1
```

If no stories exist, start with `Story-01`. Otherwise, increment the highest number.

### Step 3: Generate Story Content

Use the template in `assets/story-template.md` as the structure. Fill in each section based on the requirement:

**Section 1: User Story**
- Identify the user role/persona
- Define the goal or action they want to perform
- Explain the value or benefit

**Section 2: Scenario Requirements**
- List 2-5 concrete scenarios where this feature is used
- For each scenario, describe the context and specific needs

**Section 3: System Requirements**
- Extract functional requirements from scenarios
- Identify non-functional requirements (performance, security, usability)

**Section 4: System Architecture** (optional)
- Include only if the requirement involves multiple components or complex interactions
- Describe the overall architecture, core components, and technology stack
- Skip for simple single-component features

**Section 5: External Interfaces**
- Define APIs with methods, paths, parameters, and responses
- Specify CLI commands with options and arguments
- Document exposed methods with signatures

**Section 6: Data Structures**
- Design database schemas with field descriptions
- Define interface data structures (request/response formats)

**Section 7: Acceptance Criteria**
- Functional: What features must work correctly
- Performance: Response times, throughput, resource usage
- Exception handling: Error cases and expected behavior

**Section 8: Task Breakdown**
- Decompose into 3-8 independent, sequential tasks
- For each task, include:
  - **目标**: Clear objective (1 sentence)
  - **主要工作**: 2-4 key work items
  - **关键点**: Important technical considerations or gotchas
  - **依赖**: Which tasks must complete first

Task detail level: Medium (3-5 sentences per task). Tasks should be detailed enough to understand scope but concise enough that detailed task documents can be generated later using the story-to-tasks skill.

### Step 4: Create the Story File

Write the story document to `docs/story/Story-XX-[name].md` where:
- XX is the sequential number (e.g., 01, 02, 03)
- [name] is a short kebab-case description (e.g., user-authentication, pdf-export)

Ensure the docs/story/ directory exists, creating it if necessary.

### Step 5: Confirm Completion

Inform the user that the story has been created and provide:
- The file path
- A brief summary of what was documented
- Next steps (e.g., "Use the story-to-tasks skill to generate detailed task documents")

## Template Structure

The story template is available in `assets/story-template.md`. Key formatting guidelines:

- Use markdown headers (##) for main sections
- Use ### for subsections
- Use code blocks for code examples, SQL, JSON
- Use checkboxes [ ] for acceptance criteria
- Use bold for field labels in data structures
- Keep consistent formatting across all stories

## Integration with story-to-tasks

The Task Breakdown section (Section 8) is designed to be compatible with the story-to-tasks skill:

- Each task should be independent and implementable
- Tasks should follow a logical sequence (dependencies noted)
- Task descriptions should be clear enough to expand into full task documents
- Use consistent task naming: "Task 1: [Name]", "Task 2: [Name]", etc.

After creating a story, users can run the story-to-tasks skill to generate detailed task documents in `docs/task/[story-name]/`.

## Examples

**Example trigger 1:**
> "Create a story for a user authentication system with JWT tokens"

**Example trigger 2:**
> "I need to document a requirement for exporting reports to PDF"

**Example trigger 3:**
> "Write a story about adding real-time notifications to the dashboard"

## Resources

### assets/story-template.md
Complete story document template with all 8 sections pre-formatted. Use this as the base structure when generating new stories.
