# 同步-异步-多线程 爬虫测试


### 同步版本爬虫代码

```python
import requests
from bs4 import BeautifulSoup
import time

def fetch_url(url):
    response = requests.get(url)
    soup = BeautifulSoup(response.text, 'html.parser')
    return soup.title.string

def main(urls):
    results = []
    for url in urls:
        title = fetch_url(url)
        results.append((url, title))
    return results

urls = [
    "https://www.python.org",
    "https://www.djangoproject.com",
    "https://flask.palletsprojects.com",
    "https://fastapi.tiangolo.com",
    "https://docs.aiohttp.org",
    "https://kubernetes.io",
    "https://www.cncf.io",
    "https://www.linux.org",
    "https://www.hello-algo.com",
    "https://www.apple.com.cn",
] 


start_time = time.time()
results = main(urls)
end_time = time.time()

for url, title in results:
    print(f'Title of {url}: {title}')

print(f'Total time taken: {end_time - start_time} seconds')
```

### 解释

1. **`fetch_url` 函数**：使用 `requests.get` 方法同步请求 URL，并使用 `BeautifulSoup` 提取 HTML 内容中的 `<title>` 标签。
2. **`main` 函数**：迭代 URL 列表，对每个 URL 调用 `fetch_url` 函数，并收集结果。
3. **计时**：使用 `time.time()` 记录脚本开始和结束的时间，计算总耗时。

### 异步版本爬虫代码

```python
import asyncio
import aiohttp
from bs4 import BeautifulSoup
import time

async def fetch_url(session, url):
    async with session.get(url) as response:
        html = await response.text()
        soup = BeautifulSoup(html, 'html.parser')
        return soup.title.string

async def main(urls):
    async with aiohttp.ClientSession() as session:
        tasks = [fetch_url(session, url) for url in urls]
        results = await asyncio.gather(*tasks)
        return list(zip(urls, results))

urls = [
    "https://www.python.org",
    "https://www.djangoproject.com",
    "https://flask.palletsprojects.com",
    "https://fastapi.tiangolo.com",
    "https://docs.aiohttp.org",
    "https://kubernetes.io",
    "https://www.cncf.io",
    "https://www.linux.org",
    "https://www.hello-algo.com",
    "https://www.apple.com.cn",
]

start_time = time.time()
results = asyncio.run(main(urls))
end_time = time.time()

for url, title in results:
    print(f'Title of {url}: {title}')

print(f'Total time taken: {end_time - start_time} seconds')
```

### 多线程版本爬虫代码

```python
import requests
from bs4 import BeautifulSoup
import time
from concurrent.futures import ThreadPoolExecutor, as_completed


def fetch_url(url):
    response = requests.get(url)
    soup = BeautifulSoup(response.text, "html.parser")
    return soup.title.string


def main(urls):
    results = []
    with ThreadPoolExecutor(max_workers=10) as executor:
        future_to_url = {executor.submit(fetch_url, url): url for url in urls}
        for future in as_completed(future_to_url):
            url = future_to_url[future]
            try:
                title = future.result()
                results.append((url, title))
            except Exception as exc:
                print(f"{url} generated an exception: {exc}")
    return results


urls = [
    "https://www.python.org",
    "https://www.djangoproject.com",
    "https://flask.palletsprojects.com",
    "https://fastapi.tiangolo.com",
    "https://docs.aiohttp.org",
    "https://kubernetes.io",
    "https://www.cncf.io",
    "https://www.linux.org",
    "https://www.hello-algo.com",
    "https://www.apple.com.cn/",
]
start_time = time.time()
results = main(urls)
end_time = time.time()

for url, title in results:
    print(f"Title of {url}: {title}")

print(f"Total time taken: {end_time - start_time} seconds")
```

### 测试和比较

1. **同步版本**：运行同步版本脚本并记录总耗时。
2. **异步版本**：运行异步版本脚本并记录总耗时。
3. **多线程版本**：运行多线程版本脚本并记录总耗时。
4. **比较结果**：比较三种方法的耗时，以评估异步编程在处理 I/O 密集型任务时的性能优势。

通过比较三种方法的总耗时，可以了解异步编程在并发处理 I/O 操作时的性能优势。

### 运行结果示例

```bash
# 同步版本输出

Title of https://www.python.org: Welcome to Python.org
Title of https://www.djangoproject.com: The web framework for perfectionists with deadlines | Django
Title of https://flask.palletsprojects.com: Just a moment...
Title of https://fastapi.tiangolo.com: FastAPI
Title of https://docs.aiohttp.org: Just a moment...
Title of https://kubernetes.io: Kubernetes
Title of https://www.cncf.io: Cloud Native Computing Foundation
Title of https://www.linux.org: Just a moment...
Title of https://www.hello-algo.com: 
      Hello 算法

Title of https://www.apple.com.cn: Apple (中国大陆) - 官方网站
Total time taken: 3.418560028076172 seconds

# 异步版本输出
···
Total time taken: 0.5966830253601074 seconds

# 多线程版本输出
···
Total time taken: 0.8671779632568359 seconds
```
这表明异步编程显著提高了爬虫的效率。

通过这种测试，你可以清晰地看到异步编程在处理 I/O 密集型任务时的性能提升。