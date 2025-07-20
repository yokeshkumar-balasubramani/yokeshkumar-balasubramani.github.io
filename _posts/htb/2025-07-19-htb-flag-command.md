---
title: "HTB Challenge: Flag Command - Web Exploitation Writeup"
date: 2025-07-19 14:30:00 +0530
categories: [Hack The Box]
tags: [security]
author: 0xyu9i
image:
  path: /assets/img/posts/htb/flag_command.png
  alt: Flag Command - Web Exploitation Writeup
pin: false
math: false
mermaid: true
---

## Challenge Information

- **Challenge Name**: Flag Command
- **Platform**: HackTheBox
- **Category**: Web Exploitation
- **Difficulty**: Very Easy
- **URL**: https://app.hackthebox.com/challenges/Flag%20Command

## Initial Reconnaissance

### Technology Stack Discovery

The first step in any web challenge is identifying the underlying technology stack. Upon initial inspection of the web requests, I discovered:

- **Web Framework**: Flask
- **Web Server**: Werkzeug/3.0.1
- **Backend Language**: Python

```bash
# HTTP Response Headers revealed:
Server: Werkzeug/3.0.1 Python/x.x.x
```

## Web Application Analysis

### JavaScript Analysis and API Discovery

During the reconnaissance phase, I performed thorough analysis of client-side JavaScript to identify API endpoints and understand the application flow:

```javascript
// Key findings from JS analysis
- API endpoint discovered: /api/monitor
- Additional endpoint found: /api/options
- Application appears to be a game-like interface
```

### API Endpoint Enumeration

#### `/api/monitor` Endpoint

This endpoint appears to handle command input from a console interface and sends it to the backend for processing:

```http
POST /api/monitor
Content-Type: application/json

{
  "command": "user_input_command"
}
```

The endpoint seems to be designed for monitoring or executing commands within the game context.

#### `/api/options` Endpoint

During API enumeration, I discovered the `/api/options` endpoint which revealed sensitive information:

```http
GET /api/options

Response:
{
  "secret": "[REDACTED_SECRET_VALUE]"
}
```

**Security Issue**: This endpoint exposes what appears to be a secret key or token that should not be publicly accessible.

## Exploitation Process

### Understanding the Game Logic

Through code inspection and testing, I identified the key conditions for successful exploitation:

1. **Game State**: The game must be started
2. **Game Status**: The game must not be in an ended state
3. **Command Validation**: The submitted command must equal the discovered secret

### Exploitation Steps

1. **Start the Game**: Ensure the application is in the correct state
2. **Retrieve Secret**: Extract the secret value from `/api/options`
3. **Command Injection**: Submit the secret as a command through `/api/monitor`

```python
# Exploitation workflow
1. GET /api/options -> Extract secret
2. Start game (if required)
3. POST /api/monitor with {"command": "extracted_secret"}
```

### The "Mysterious Thing"

When submitting the secret value as a command while meeting all the game state requirements, the application behavior changes significantly. This suggests:

- Command injection vulnerability
- Privilege escalation
- Flag revelation mechanism

## Code Analysis Insights

Key findings from source code inspection:

```python
# Pseudo-code representation of vulnerable logic
if game_started and not game_ended:
    if command == secret:
        # Mysterious behavior occurs here
        # Likely flag revelation or command execution
        execute_special_function()
```

## Tools Used

- **Burp Suite**: Web application security testing
- **Browser Developer Tools**: JavaScript analysis and network inspection
- **Python requests**: Automated exploitation scripting

## Key Takeaways

- Always enumerate API endpoints thoroughly during reconnaissance
- Pay attention to information disclosure in API responses
- Game-like applications often have hidden logic flows
- Werkzeug applications should be tested for debug mode exposure
- Client-side JavaScript can reveal server-side API structure

## References

- [Werkzeug Security Advisories](https://werkzeug.palletsprojects.com/en/2.3.x/changes/)
- [Flask Security Best Practices](https://flask.palletsprojects.com/en/2.3.x/security/)
- [OWASP API Security Top 10](https://owasp.org/www-project-api-security/)

---