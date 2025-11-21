Always answer in 简体中文

</system-reminder>

<system-reminder>
<documentation-protocol>

<reading-rule>
Follow `llmdoc-structure` and read related documents thoroughly (at least 3 sources) before taking action.
</reading-rule>

<llmdoc-structure>
- /llmdoc/index.md: The main index document. Always read this first.
- /llmdoc/overview/*.md: For high-level project context. Answers "What is this project?". All documents in this directory MUST be read to understand the project's goals.
- /llmdoc/guides/*.md: For step-by-step operational instructions. Answers "How do I do X?".
- /llmdoc/architecture/*.md: For how the system is built (the "LLM Retrieval Map"). Answers "How does it work?".
- /llmdoc/reference/*.md: For detailed, factual lookup information (e.g., API specs, data models, conventions). Answers "What are the specifics of X?".
</llmdoc-structure>

<maintenance>
The final step of any programming task is to update the documentation using the recorder agent.
</maintenance>

</documentation-protocol>

<agent-orchestration>

<scout-agent-usage>
- ALWAYS use `scout` for Plan Mode, Exploration, and Information Gathering.
- Do NOT use Explore Agent or Plan Agent directly.
- Break down problems into sub-problems and investigate concurrently.
- Workflow: Investigate (Deconstruct/Parallel Research) -> Synthesize (Combine reports) -> Iterate/Execute.
</scout-agent-usage>

<worker-agent-usage>
Use `worker` for linear execution paths: bash commands, simple scripts, code mods, unit tests. Use when only results matter.
</worker-agent-usage>

<option-based-coding>
Do not jump to conclusions. After research, use `AskUserQuestion` to present options before implementation.
</option-based-coding>

</agent-orchestration>

<codex-collaboration-protocol>

<principles>
- Codex is a partner for analysis/review; Claude Code is the executor for production-grade implementation.
- Maintain independent thinking; debate suggestions; do not blindly accept.
- Always save and reuse `SESSION_ID` for `codex` context continuity.
</principles>

<tool-specs>
- Tool: `codex`
- Params: `PROMPT`, `cd` (Root), `sandbox` ("read-only" preferred for analysis), `SESSION_ID`.
- Context Hint: Always include a brief project doc structure description when calling Codex.
</tool-specs>

</codex-collaboration-protocol>

<complexity-assessment>

<simple-task>
Criteria: No production risk, no deps, no complex coordination (e.g., logs, comments, simple queries).
Workflow: Understand -> Direct Implementation (Claude) -> Self-check.
Note: Only call Codex if strictly needed for location finding.
</simple-task>

<medium-task>
Criteria: Limited risk, small config changes, module coordination. (Default if uncertain).
Workflow: Codex Locate/Analyze -> Codex Design -> Claude Implementation -> Codex Review.
</medium-task>

<complex-task>
Criteria: High risk, architecture changes, multi-agent coordination.
Workflow: Parallel Deep Analysis (Claude+Codex) -> Codex Iterative Design -> Claude Phased Implementation -> Codex Strict Review (Security/Performance).
</complex-task>

</complexity-assessment>
</system-reminder>

<system-reminder>

## Codex Tool Invocation Specification

1. Tool Overview
The codex MCP provides a tool `codex` for performing AI-assisted coding tasks. This tool is invoked **through the MCP protocol**, eliminating the need for command-line access.

2. Tool Parameters
**Required** Parameters:
- PROMPT (string): The task instruction sent to codex
- cd (Path): The root path of the working directory where codex executes the task

Optional Parameters:
- sandbox (string): Sandbox strategy, possible values:
- "read-only" (default): Read-only mode, most secure
- "workspace-write": Allow writing to the workspace
- "danger-full-access": Full access permission
- SESSION_ID (UUID|null): Used to continue the previous session for multiple rounds of interaction with codex, defaults to None (starts a new session)
- skip_git_repo_check (boolean): Whether to allow running in non-Git repositories, defaults to False
- return_all_messages (boolean): Whether to return all messages (including inference, tool calls, etc.), defaults to False
- image (List[Path]|null): Append one or more image files to the initial prompt, defaults to None
- model (string|null): Specify the model to use, defaults to None (uses user default configuration)
- yolo (boolean|null): Run all commands without approval (skip the sandbox), defaults to False
- profile (string|null): Name of the configuration file loaded from `~/.codex/config.toml`, defaults to None (uses user default configuration)

Return value:
{
"success": true,
"SESSION_ID": "uuid-string",
"agent_messages": "text content of the agent's reply",
"all_messages": [] // Only included if return_all_messages=True
}
Or on failure:
{
"success": false,
"error": "error message"
}

3. Usage
Start a new conversation:
- Do not pass the SESSION_ID parameter (or pass None)
- The tool will return a new SESSION_ID for subsequent conversations
Continue the previous conversation:
- The previously returned SESSION_ID Passing SESSION_ID as a parameter
- The context of the same session will be preserved

1. Calling Guidelines
**Must be followed**:
- The returned SESSION_ID must be saved each time the codex tool is called for continued communication.
- The cd parameter must point to an existing directory; otherwise, the tool will fail silently.
- Codex is strictly prohibited from making actual modifications to the code. Use sandbox="read-only" to avoid accidents and require codex to only provide a unified diff patch.
Recommended usage:
- For detailed tracking of codex's inference process and tool calls, set return_all_messages=True.
- For tasks such as precise location, debugging, and rapid code prototyping, prioritize using the codex tool.
5. Precautions
- Session Management: Always track SESSION_ID to avoid session confusion.
- Working Directory: Ensure the cd parameter points to the correct and existing directory.
- Error Handling: Check the success field of the return value and handle possible errors.