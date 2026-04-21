<!--
Extracted from: C:/Users/ptann/OneDrive/Work/motion granted/Main System/Motion-Granted-Production/.claude/skills/sql/SKILL.md
Original purpose: Execute SQL directly against the project's database via MCP, with show-before-write discipline.
Genericized: 2026-04-21
Operator: Porter Tanner
-->

---
name: sql
description: >
  Execute SQL directly against the project's database (via an MCP execute_sql tool or
  equivalent). For schema changes, data investigations, debugging queries, and direct
  data fixes. Always confirms before any write operation. Read operations execute immediately.
disable-model-invocation: true
argument-hint: "[sql query or description of what to do]"
allowed-tools: Bash(grep *) Read Grep
---

# SQL — Direct Database Access

Execute SQL against the project's database.

**Request**: $ARGUMENTS

> **Project DB identifier**: fill in the project's database / MCP project ID here before first use.

## Rules

### BEFORE ANY WRITE OPERATION (INSERT, UPDATE, DELETE, ALTER, CREATE, DROP):
1. **Show the exact SQL** you intend to run
2. **Explain what it will change** in plain English
3. **Show a SELECT first** that demonstrates the current state
4. **Wait for the operator's explicit approval** before executing
5. **Schema changes**: migrations typically deploy before code changes. Production migrations belong to a human.

### READ OPERATIONS (SELECT):
- Execute immediately. No approval needed.
- Format results clearly.

### SAFETY
- Never DROP a table without explicit instruction
- Never UPDATE/DELETE without a WHERE clause
- Always use transactions for multi-step writes when possible
- For bulk updates, show COUNT(*) of affected rows before executing

## Common Queries Template

Populate this section with project-specific queries. Example shape:

```sql
-- Entity overview
SELECT <core_fields>
FROM <main_table>
LEFT JOIN <related_table> ON ...
WHERE <identifier> = '{id}';

-- Status distribution
SELECT <status_column>, COUNT(*) FROM <table> GROUP BY <status_column>;

-- Workflow execution history
SELECT * FROM <execution_history_table>
WHERE <entity_id> = '{id}'
ORDER BY created_at;
```
