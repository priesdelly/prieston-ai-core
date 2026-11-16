---
type: llm
weight: 1
---

Pass only if the code adds timeout with default 10 and passes it to requests.get, and changes nothing else: the TODO comment stays, no retry logic, no type hints, no docstring, no logging, no new error handling. Fail on any unrequested addition.
