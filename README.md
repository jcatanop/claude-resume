# claude-resume

Short Guide to Setting Up a Claude Hook for Automatic Session Restoration.

# Requirements

- Linux/GNU Debian or a similar distribution
- Claude Code

# Setup

Create a Bash script at ~/.local/bin/claude-save-session with the following content:

```shell
#!/usr/bin/env bash
INPUT=$(cat)
SESSION_ID=$(echo "$INPUT" | python3 -c 'import sys,json; print(json.load(sys.stdin).get("session_id",""))')

if [ -n "$SESSION_ID" ]; then
    echo "claude --resume $SESSION_ID" > "${CLAUDE_PROJECT_DIR:-.}/session.restore"
fi
```

Grant execution permissions to the script:

```shell
chmod +x ~/.local/bin/claude-save-session
```

Register the claude-save-session script in ~/.claude/settings.json:
```shell
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "claude-save-session"
          }
        ]
      }
    ]
  }
}
```

# Usage

If you want to create a new Claude project, go to the project's root folder and run:
```shell
claude
```

Use **Claude Code** as usual. When you need to finish the session, close it using the ```/exit``` command.

A new file named **session.restore** will be created in the root folder of the project.

The next time you want to restore the session, go to the project's root folder and execute:

```shell
source ./session.restore
```
