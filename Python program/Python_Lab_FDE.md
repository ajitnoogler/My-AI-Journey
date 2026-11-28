<h1 align="center">Python Lab for AI FDE Engineers</h1>

<p align="center">Python from basics to advanced, built for the AI Forward Deployed Engineer track: core language, OOP, async, APIs, cloud automation, data, GenAI and testing.</p>

<p align="center"><img src="https://img.shields.io/badge/programs-66-blue" alt="programs"> <img src="https://img.shields.io/badge/modules-17-green" alt="modules"> <img src="https://img.shields.io/badge/language-Python-yellow" alt="python"></p>

---

## Course Contents

| # | Module | Programs |
|---|--------|----------|
| 01 | [Python Basics](#01-python-basics) | 5 |
| 02 | [Conditional Statements](#02-conditional-statements) | 4 |
| 03 | [Loops](#03-loops) | 4 |
| 04 | [Data Structures](#04-data-structures) | 6 |
| 05 | [Functions and Lambda](#05-functions-and-lambda) | 5 |
| 06 | [File Handling](#06-file-handling) | 4 |
| 07 | [Exception Handling](#07-exception-handling) | 3 |
| 08 | [Regular Expressions](#08-regular-expressions) | 3 |
| 09 | [OOP](#09-oop) | 5 |
| 10 | [Modules, Typing, CLI](#10-modules-typing-cli) | 4 |
| 11 | [Advanced Python](#11-advanced-python) | 4 |
| 12 | [Concurrency](#12-concurrency) | 2 |
| 13 | [APIs and Cloud](#13-apis-and-cloud) | 4 |
| 14 | [Data with NumPy and Pandas](#14-data-with-numpy-and-pandas) | 2 |
| 15 | [GenAI and FDE Toolkit](#15-genai-and-fde-toolkit) | 6 |
| 16 | [Testing and Logging](#16-testing-and-logging) | 2 |
| 17 | [Revision Mini Projects](#17-revision-mini-projects) | 3 |

> Use your browser find (Ctrl+F or Cmd+F) to search for a topic, for example "heap", "regex" or "date".

---

## 01. Python Basics

### #1 Variables and types

```python
x = 10
pi = 3.14
ok = True
name = "edge-01"
print(type(x), type(pi), type(ok), type(name))
```

### #2 f-strings and formatting

```python
host, port = "api.local", 443
print(f"{host}:{port}")
print(f"{3.14159:.2f} {1234567:,}")
```

### #3 Arithmetic operators

```python
print(7 // 2, 7 % 2, 2 ** 10, round(7 / 3, 2))
```

### #4 String methods

```python
s = " BGP, OSPF ,ECMP "
print(s.strip().lower().split(","))
print("-".join(["a", "b", "c"]))
```

### #5 Unpacking and swap

```python
a, b = 1, 2
a, b = b, a
first, *rest = [1, 2, 3, 4]
print(a, b, first, rest)
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 02. Conditional Statements

### #1 Grade calculator

```python
marks = 82
if marks >= 90: grade = "A"
elif marks >= 75: grade = "B"
elif marks >= 50: grade = "C"
else: grade = "Fail"
print(grade)
```

### #2 Leap year

```python
year = 2028
if year % 400 == 0 or (year % 4 == 0 and year % 100 != 0):
    print("Leap year")
else:
    print("Not leap")
```

### #3 match statement

```python
code = 404
match code:
    case 200: print("OK")
    case 404 | 410: print("Gone")
    case _: print("Other")
```

### #4 Truthiness

```python
for v in [0, "", [], None, "x", [0]]:
    print(repr(v), bool(v))
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 03. Loops

### #1 FizzBuzz

```python
for i in range(1, 16):
    print("FizzBuzz" if i % 15 == 0 else "Fizz" if i % 3 == 0
          else "Buzz" if i % 5 == 0 else i)
```

### #2 Prime check

```python
def is_prime(n):
    if n < 2: return False
    for i in range(2, int(n ** 0.5) + 1):
        if n % i == 0: return False
    return True
print([n for n in range(30) if is_prime(n)])
```

### #3 enumerate and zip

```python
hosts = ["r1", "r2"]
ips = ["10.0.0.1", "10.0.0.2"]
for i, (h, ip) in enumerate(zip(hosts, ips), 1):
    print(i, h, ip)
```

### #4 while with break and else

```python
tries = 0
while tries < 3:
    tries += 1
    if tries == 5: break
else:
    print("gave up after", tries)
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 04. Data Structures

### #1 List operations

```python
nums = [5, 2, 9]
nums.append(1); nums.sort(reverse=True)
print(nums, nums[1:3], nums[::-1])
```

### #2 Dictionary and word count

```python
from collections import Counter
text = "bgp ospf bgp ecmp bgp"
print(Counter(text.split()).most_common(2))
```

### #3 Set operations

```python
a = {"10.0.0.1", "10.0.0.2"}
b = {"10.0.0.2", "10.0.0.3"}
print(a & b, a | b, a - b)
```

### #4 Comprehensions

```python
sq = {n: n * n for n in range(5)}
ev = [n for n in range(10) if n % 2 == 0]
print(sq, ev)
```

### #5 Sorting with key

```python
devs = [{"n": "r1", "cpu": 80}, {"n": "r2", "cpu": 35}]
print(sorted(devs, key=lambda d: d["cpu"]))
```

### #6 namedtuple

```python
from collections import namedtuple
Route = namedtuple("Route", "prefix nexthop")
r1 = Route("10.0.0.0/8", "192.168.1.1")
print(r1.prefix, r1.nexthop)
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 05. Functions and Lambda

### #1 Defaults and keyword args

```python
def connect(host, port=22, *, timeout=5):
    return f"{host}:{port} t={timeout}"
print(connect("r1", timeout=10))
```

### #2 *args and **kwargs

```python
def show(*args, **kwargs):
    print(args, kwargs)
show(1, 2, vlan=10, name="mgmt")
```

### #3 map, filter, lambda

```python
n = [1, 2, 3, 4]
print(list(map(lambda x: x * 2, n)))
print(list(filter(lambda x: x % 2, n)))
```

### #4 Recursion with cache

```python
from functools import lru_cache
@lru_cache(maxsize=None)
def fib(n):
    return n if n < 2 else fib(n - 1) + fib(n - 2)
print(fib(50))
```

### #5 Closure

```python
def counter():
    c = 0
    def inc():
        nonlocal c
        c += 1
        return c
    return inc
f = counter(); f(); print(f())
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 06. File Handling

### #1 Write and read text

```python
with open("notes.txt", "w") as f:
    f.write("line1\nline2\n")
with open("notes.txt") as f:
    for line in f: print(line.strip())
```

### #2 CSV with DictReader

```python
import csv
with open("devices.csv", newline="") as f:
    for row in csv.DictReader(f):
        print(row["host"], row["ip"])
```

### #3 JSON load and dump

```python
import json
cfg = {"asn": 65001, "peers": ["10.0.0.1"]}
with open("cfg.json", "w") as f: json.dump(cfg, f, indent=2)
print(json.load(open("cfg.json"))["asn"])
```

### #4 pathlib

```python
from pathlib import Path
for p in Path(".").glob("*.py"):
    print(p.name, p.stat().st_size)
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 07. Exception Handling

### #1 try, except, else, finally

```python
try:
    n = int("42x")
except ValueError as e:
    print("bad input:", e)
else:
    print("ok", n)
finally:
    print("always runs")
```

### #2 Custom exception

```python
class DeviceDown(Exception): pass
def check(up):
    if not up: raise DeviceDown("r1 unreachable")
try: check(False)
except DeviceDown as e: print(e)
```

### #3 Retry with backoff

```python
import time
def retry(fn, n=3, delay=1):
    for i in range(n):
        try: return fn()
        except Exception as e:
            if i == n - 1: raise
            time.sleep(delay * 2 ** i)
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 08. Regular Expressions

### #1 Find IP addresses

```python
import re
log = "src 10.1.1.5 dst 192.168.0.9"
print(re.findall(r"\b\d{1,3}(?:\.\d{1,3}){3}\b", log))
```

### #2 Named groups

```python
import re
m = re.match(r"(?P<ts>\S+) (?P<lvl>\w+) (?P<msg>.*)", "12:01 ERROR link down")
print(m.groupdict())
```

### #3 Validate email and substitute

```python
import re
print(bool(re.fullmatch(r"[\w.+-]+@[\w-]+\.[\w.]+", "a@b.com")))
print(re.sub(r"\d", "#", "pin 4821"))
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 09. OOP

### #1 Class and object

```python
class Router:
    def __init__(self, name): self.name, self.up = name, True
    def status(self): return f"{self.name} up={self.up}"
print(Router("r1").status())
```

### #2 Inheritance and super

```python
class Device:
    def __init__(self, n): self.n = n
class Firewall(Device):
    def __init__(self, n, vendor):
        super().__init__(n); self.vendor = vendor
print(Firewall("fw1", "PaloAlto").vendor)
```

### #3 Dunder methods

```python
class Vec:
    def __init__(self, x, y): self.x, self.y = x, y
    def __add__(self, o): return Vec(self.x + o.x, self.y + o.y)
    def __repr__(self): return f"Vec({self.x},{self.y})"
print(Vec(1, 2) + Vec(3, 4))
```

### #4 dataclass

```python
from dataclasses import dataclass, field
@dataclass
class Peer:
    ip: str
    asn: int = 65000
    tags: list = field(default_factory=list)
print(Peer("10.0.0.1"))
```

### #5 Abstract base class

```python
from abc import ABC, abstractmethod
class Backend(ABC):
    @abstractmethod
    def deploy(self): ...
class AWS(Backend):
    def deploy(self): return "deployed to AWS"
print(AWS().deploy())
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 10. Modules, Typing, CLI

### #1 Virtual env and pip

```python
# run in terminal
# python -m venv .venv
# source .venv/bin/activate      (Windows: .venv\Scripts\activate)
# pip install requests pandas
# pip freeze > requirements.txt
```

### #2 Type hints

```python
def avg(xs: list[float]) -> float:
    return sum(xs) / len(xs)
def find(d: dict[str, int], k: str) -> int | None:
    return d.get(k)
```

### #3 main guard and argparse

```python
import argparse
def main():
    p = argparse.ArgumentParser()
    p.add_argument("--host", required=True)
    p.add_argument("-v", action="store_true")
    print(p.parse_args())
if __name__ == "__main__":
    main()
```

### #4 Environment variables

```python
import os
key = os.getenv("API_KEY")
if not key:
    raise SystemExit("Set API_KEY first")
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 11. Advanced Python

### #1 Generators

```python
def read_chunks(items, n):
    for i in range(0, len(items), n):
        yield items[i:i + n]
print(list(read_chunks(list(range(7)), 3)))
```

### #2 Decorator (timing)

```python
import time, functools
def timed(fn):
    @functools.wraps(fn)
    def w(*a, **k):
        t = time.perf_counter(); out = fn(*a, **k)
        print(fn.__name__, round(time.perf_counter() - t, 4), "s"); return out
    return w
@timed
def work(): sum(range(10**6))
work()
```

### #3 Context manager

```python
from contextlib import contextmanager
@contextmanager
def session(name):
    print("open", name)
    try: yield name
    finally: print("close", name)
with session("db"): print("working")
```

### #4 itertools and functools

```python
from itertools import groupby, islice
from functools import reduce
print(reduce(lambda a, b: a * b, [1, 2, 3, 4]))
print([(k, len(list(g))) for k, g in groupby("aabbbc")])
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 12. Concurrency

### #1 asyncio gather

```python
import asyncio
async def ping(h):
    await asyncio.sleep(1); return f"{h} ok"
async def main():
    print(await asyncio.gather(*(ping(h) for h in ["r1", "r2", "r3"])))
asyncio.run(main())
```

### #2 ThreadPoolExecutor

```python
from concurrent.futures import ThreadPoolExecutor
def check(h): return h, "up"
with ThreadPoolExecutor(max_workers=8) as ex:
    print(list(ex.map(check, ["r1", "r2", "r3"])))
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 13. APIs and Cloud

### #1 requests GET with timeout

```python
import requests
r = requests.get("https://api.github.com", timeout=5)
r.raise_for_status()
print(r.status_code, list(r.json())[:3])
```

### #2 FastAPI endpoint

```python
# pip install fastapi uvicorn
from fastapi import FastAPI
app = FastAPI()
@app.get("/health")
def health(): return {"status": "ok"}
# run: uvicorn app:app --reload
```

### #3 boto3 list S3 buckets

```python
# pip install boto3, then aws configure
import boto3
s3 = boto3.client("s3")
for b in s3.list_buckets()["Buckets"]:
    print(b["Name"])
```

### #4 subprocess ping

```python
import subprocess
res = subprocess.run(["ping", "-c", "1", "8.8.8.8"],
                     capture_output=True, text=True, timeout=5)
print(res.returncode, res.stdout[:80])
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 14. Data with NumPy and Pandas

### #1 NumPy vectors

```python
import numpy as np
a = np.array([1, 2, 3]); b = np.array([4, 5, 6])
print(a + b, a @ b, a.mean(), np.linalg.norm(a))
```

### #2 Pandas groupby

```python
import pandas as pd
df = pd.DataFrame({"dc": ["A", "A", "B"], "cpu": [70, 50, 30]})
print(df.groupby("dc")["cpu"].mean())
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 15. GenAI and FDE Toolkit

### #1 Cosine similarity

```python
import numpy as np
def cos(a, b):
    a, b = np.array(a), np.array(b)
    return float(a @ b / (np.linalg.norm(a) * np.linalg.norm(b)))
print(cos([1, 0, 1], [1, 1, 0]))
```

### #2 Text chunking with overlap

```python
def chunk(text, size=200, overlap=40):
    out, i = [], 0
    while i < len(text):
        out.append(text[i:i + size]); i += size - overlap
    return out
print(len(chunk("x" * 1000)))
```

### #3 Top-k retrieval (RAG core)

```python
import numpy as np
def top_k(q_vec, doc_vecs, docs, k=3):
    sims = doc_vecs @ q_vec / (np.linalg.norm(doc_vecs, axis=1) * np.linalg.norm(q_vec))
    return [docs[i] for i in np.argsort(-sims)[:k]]
```

### #4 Call an LLM API (Anthropic SDK)

```python
# pip install anthropic ; set ANTHROPIC_API_KEY
import anthropic
c = anthropic.Anthropic()
m = c.messages.create(model="claude-sonnet-5-5", max_tokens=300,
    messages=[{"role": "user", "content": "Explain BGP in 2 lines"}])
print(m.content[0].text)
```

### #5 Validate model output (pydantic)

```python
# pip install pydantic
from pydantic import BaseModel, ValidationError
class Ticket(BaseModel):
    severity: int
    summary: str
try: print(Ticket.model_validate_json('{"severity": 2, "summary": "link flap"}'))
except ValidationError as e: print(e)
```

### #6 Tiny evaluation harness

```python
cases = [("2+2", "4"), ("capital of France", "Paris")]
def run(ask):
    hits = sum(exp.lower() in ask(q).lower() for q, exp in cases)
    return hits / len(cases)
print(run(lambda q: "4" if "2+2" in q else "Paris"))
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 16. Testing and Logging

### #1 logging setup

```python
import logging
logging.basicConfig(level=logging.INFO,
    format="%(asctime)s %(levelname)s %(message)s")
log = logging.getLogger("fde")
log.info("deploy started"); log.error("health check failed")
```

### #2 pytest basics

```python
# test_math.py ; run: pytest -q
def add(a, b): return a + b
def test_add(): assert add(2, 3) == 5
import pytest
@pytest.mark.parametrize("a,b,e", [(1, 1, 2), (0, 5, 5)])
def test_many(a, b, e): assert add(a, b) == e
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 17. Revision Mini Projects

### #1 Log analyzer

```python
import re
from collections import Counter
lines = ["ERROR r1 down", "INFO r2 ok", "ERROR r1 down"]
lv = Counter(re.match(r"(\w+)", l).group(1) for l in lines)
print(lv)
```

### #2 Async health checker

```python
import asyncio
async def check(h):
    try:
        _, w = await asyncio.wait_for(asyncio.open_connection(h, 443), 3)
        w.close(); return h, "up"
    except Exception: return h, "down"
print(asyncio.run(asyncio.gather(*[check(h) for h in ["example.com"]])) if False else "run inside async main")
```

### #3 CLI todo with JSON storage

```python
import json, sys, pathlib
f = pathlib.Path("todo.json")
todo = json.loads(f.read_text()) if f.exists() else []
if len(sys.argv) > 1: todo.append(" ".join(sys.argv[1:]))
f.write_text(json.dumps(todo)); print(todo)
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

