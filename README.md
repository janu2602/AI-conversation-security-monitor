# AI Conversation Security Monitor

![Python](https://img.shields.io/badge/Python-3.8+-blue) ![Splunk](https://img.shields.io/badge/Splunk-Compatible-green)

Logs every AI conversation message as structured JSON, scores risk, tracks session patterns,
and fires security alerts — feeding directly into Splunk for SOC visibility.

## How It Works
```
Message → Injection scan → PII check → Risk score →
  0-69: ALLOWED + INFO log | 70-89: WARNING | 90+: BLOCKED + ALERT
  3+ warnings / 10 min: SESSION_ALERT
```

## Quick Start
```bash
git clone https://github.com/YOUR_USERNAME/ai-conversation-security-monitor
cd ai-conversation-security-monitor && python3 monitor.py
```

## Log Format (Splunk-Compatible)
```json
{"timestamp":"2026-03-17T10:30:00Z","level":"ALERT",
 "user_id":"user_B","risk_score":100,"verdict":"BLOCKED"}
```

## Alert Thresholds
| Score | Level | Action |
|-------|-------|--------|
| 0-69 | INFO | Log only |
| 70-89 | WARNING | Flag |
| 90-100 | ALERT | Block |
| 3+ warnings/10min | SESSION_ALERT | Escalate |

## Project Context
Project 4 of 10 — AI Application Security Portfolio
# AI-conversation-security-monitor
