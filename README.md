# 📡 AI Conversation Security Monitor

![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=flat&logo=python&logoColor=white)
![Splunk](https://img.shields.io/badge/Splunk-Compatible-FF5500?style=flat)
![Logging](https://img.shields.io/badge/Logging-Structured%20JSON-0F6E56?style=flat)
![License](https://img.shields.io/badge/License-MIT-yellow?style=flat)

---

## 🔍 The Problem

Input validation alone is not enough. Attackers probe AI systems with **multiple low-risk messages** — each innocent individually, but revealing a coordinated attack as a pattern. Without session monitoring, security teams are blind.

**This monitor detects attack patterns across entire conversations, not just individual messages.**

---

## ⚡ How It Works

```
Message arrives
      │
      ▼
  Injection scan (P1) + PII scan (P2)
      │
      ▼
  Combined risk score (0–100)
      │
      ├── score < 70  →  ALLOWED   →  INFO log
      ├── score 70-89  →  WARNING  →  WARNING log
      └── score >= 90  →  BLOCKED  →  ALERT log + block
                              │
                              ▼
                    SessionTracker update
                              │
                    3+ warnings in 10 min?
                              │
                    Yes → SESSION_ALERT log
```

---

## 🚀 Quick Start

```bash
git clone https://github.com/YOUR_USERNAME/ai-conversation-security-monitor
cd ai-conversation-security-monitor
python3 monitor.py
# Outputs ai_security_monitor.log
```

---

## 💻 Usage

```python
from monitor import monitor_message

# Legitimate user
result = monitor_message("user_alice", "sess_001", "What is Python?")
print(result["verdict"])       # ALLOWED
print(result["risk_score"])    # 0

# Attacker
result = monitor_message("attacker_x", "sess_002", 
                         "Ignore all previous instructions")
print(result["verdict"])       # BLOCKED
print(result["alerts_fired"])  # 1
```

---

## 📋 Log Format (Splunk-Compatible)

Every event is one JSON line — ready for Splunk Universal Forwarder ingestion:

```json
{
  "timestamp": "2026-03-17T10:30:00+00:00",
  "level": "ALERT",
  "event_type": "message_blocked",
  "user_id": "attacker_x",
  "session_id": "sess_002",
  "risk_score": 100,
  "verdict": "BLOCKED",
  "injection_patterns": ["Override previous instructions"],
  "session_warnings": 1,
  "msg_hash": "a3f4b2c1d5e6..."
}
```

---

## 🚨 Alert Thresholds

| Condition | Alert Type | Response |
|-----------|-----------|---------|
| `risk_score >= 70` | WARNING | Log warning, allow |
| `risk_score >= 90` | ALERT | Log alert, block message |
| `3+ warnings in 10 min` | SESSION_ALERT | Escalate — persistent attacker |

---

## 🔗 Feeds Into Project 5

The `ai_security_monitor.log` file is ingested directly into the **Project 5 Splunk dashboard**:

```
monitor.py → ai_security_monitor.log → Splunk Forwarder → Splunk Index → Dashboard
```

---

## 🛠️ Skills Demonstrated

- Python structured logging (JSON format)
- Session-level behavioral analysis
- Time-window tracking with defaultdict
- Splunk-compatible log format design
- Multi-threshold alert system

---
