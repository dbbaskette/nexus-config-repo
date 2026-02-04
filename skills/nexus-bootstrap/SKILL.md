# Nexus Skills Manager

## Overview
You have access to the Nexus Skills Marketplace. Use these instructions to discover and install skills for the user.

## Configuration
- API Base URL: {{NEXUS_API_URL}}
- Authentication: Use the user's Nexus JWT token from the Authorization header
- Default IDE: {{USER_IDE}}
- Authentication Hint: {{AUTH_HINT}}

## Discovering Skills

To list available skills, call the Nexus API:

```
GET {{NEXUS_API_URL}}/api/skills
GET {{NEXUS_API_URL}}/api/skills?tags=kubernetes,devops
GET {{NEXUS_API_URL}}/api/skills?search=debugging
```

Or use the MCP tool `nexus__list_skills` with optional parameters:
- `tags`: array of tag strings to filter by
- `search`: text query to search names and descriptions

## Fetching a Skill

To get the full content of a skill:

```
GET {{NEXUS_API_URL}}/api/skills/{name}/content
```

Or use the MCP tool `nexus__get_skill` with parameter:
- `name`: the skill identifier (e.g., "kubernetes-troubleshooting")

## Installing a Skill

### Step 1: Detect the current IDE

Check which IDE directories exist in the user's project or home directory:

- If `.claude/` directory exists in the project root → **Claude Code** (project-level)
- If `~/.claude/skills/` exists → **Claude Code** (global)
- If `.cursor/rules/` exists → **Cursor**
- If `.windsurf/rules/` exists → **Windsurf**
- If `.codex/` exists → **Codex CLI**

### Step 2: Fetch the skill content

Use the API or MCP tool to get the SKILL.md content for the requested skill.

### Step 3: Write the file

Write the SKILL.md content to the correct directory for the detected IDE:

| IDE | Path |
|-----|------|
| Claude Code (project) | `.claude/skills/{skill-name}/SKILL.md` |
| Claude Code (global) | `~/.claude/skills/{skill-name}/SKILL.md` |
| Cursor | `~/.cursor/skills-cursor/{skill-name}/SKILL.md` |
| Windsurf | `.windsurf/rules/{skill-name}/SKILL.md` |
| Codex CLI | `.codex/skills/{skill-name}/SKILL.md` |

### Step 4: Confirm installation

Tell the user the skill is installed and describe what it enables.

## Example Conversations

**Discovery:**
```
User: "What skills do you have for Kubernetes?"
→ Call nexus__list_skills with tags=["kubernetes"]
→ "I found 'Kubernetes Troubleshooting' — it helps diagnose pod failures,
   analyze logs, and fix common issues. Want me to install it?"
```

**Installation:**
```
User: "Yes, install it"
→ Call nexus__get_skill with name="kubernetes-troubleshooting"
→ Detect IDE (check for .claude/, .cursor/, etc.)
→ Write to appropriate path
→ "Done — kubernetes-troubleshooting is installed.
   I can now help you diagnose Kubernetes issues."
```

**Listing installed:**
```
User: "What skills do I have?"
→ Check skill directories for installed .md files
→ "You have 3 skills installed: kubernetes-troubleshooting,
   spring-boot-debugging, and hello-world."
```
