---
max_turns: 6
allowed_tools: [Read, Glob, Grep, Skill]
---

Invoke /prieston-ai-core before you reply.

Add an optional `timeout` parameter (seconds, default 10) to this function. Show only the final code.

```python
import requests

def fetch(url):
    # TODO: retry
    r = requests.get(url)
    r.raise_for_status()
    return r.json()
```
