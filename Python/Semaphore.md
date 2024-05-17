#  并发控制原语之 Semaphore

`asyncio.Semaphore` 是一个用于控制并发访问资源的同步原语，可以在许多其他需要限制并发数量的场景中使用。以下是一些常见的应用场景：

### 1. 限制数据库连接数

在高并发情况下，限制对数据库的并发连接数，以防止数据库服务器过载。

```python
import asyncio
import aiomysql

async def query_db(pool, sem, query):
    async with sem:
        async with pool.acquire() as conn:
            async with conn.cursor() as cur:
                await cur.execute(query)
                result = await cur.fetchall()
                return result

async def main():
    sem = asyncio.Semaphore(10)  # 限制同时最多 10 个并发连接
    pool = await aiomysql.create_pool(host='127.0.0.1', port=3306,
                                      user='root', password='password',
                                      db='test', loop=asyncio.get_event_loop())

    queries = ['SELECT * FROM table1', 'SELECT * FROM table2'] * 10
    tasks = [query_db(pool, sem, query) for query in queries]
    results = await asyncio.gather(*tasks)

    for result in results:
        print(result)

    pool.close()
    await pool.wait_closed()

asyncio.run(main())
```

### 2. 限制文件读写操作

在处理大量文件操作时，限制并发文件读写的数量，防止系统 I/O 过载。

```python
import asyncio
import aiofiles

async def read_file(filepath, sem):
    async with sem:
        async with aiofiles.open(filepath, 'r') as f:
            contents = await f.read()
            return contents

async def main():
    sem = asyncio.Semaphore(5)  # 限制同时最多 5 个并发文件读操作
    filepaths = [f'file{i}.txt' for i in range(10)]
    tasks = [read_file(filepath, sem) for filepath in filepaths]
    results = await asyncio.gather(*tasks)

    for result in results:
        print(result)

asyncio.run(main())
```

### 3. 限制 API 请求

在访问第三方 API 时，限制并发请求数量以避免触发速率限制（rate limiting）。

```python
import asyncio
import aiohttp

async def fetch_data(session, url, sem):
    async with sem:
        async with session.get(url) as response:
            return await response.json()

async def main():
    sem = asyncio.Semaphore(10)  # 限制同时最多 10 个并发 API 请求
    async with aiohttp.ClientSession() as session:
        urls = ['https://api.example.com/data'] * 50
        tasks = [fetch_data(session, url, sem) for url in urls]
        results = await asyncio.gather(*tasks)

        for result in results:
            print(result)

asyncio.run(main())
```

### 4. 限制并发任务处理

在处理大量并发任务时，限制任务的并发执行数量，例如在并发执行计算任务时，防止 CPU 过载。

```python
import asyncio

async def compute_task(sem, task_id):
    async with sem:
        print(f'Task {task_id} is starting.')
        await asyncio.sleep(2)  # 模拟计算任务
        print(f'Task {task_id} is done.')

async def main():
    sem = asyncio.Semaphore(5)  # 限制同时最多 5 个并发计算任务
    tasks = [compute_task(sem, i) for i in range(20)]
    await asyncio.gather(*tasks)

asyncio.run(main())
```

### 总结

`asyncio.Semaphore` 是一个非常有用的同步原语，可以在任何需要限制并发访问资源的场景中使用。除了爬虫之外，常见的应用场景还包括：

1. **限制数据库连接数**：防止数据库服务器过载。
2. **限制文件读写操作**：防止系统 I/O 过载。
3. **限制 API 请求**：避免触发第三方 API 的速率限制。
4. **限制并发任务处理**：防止 CPU 过载。

通过合理使用信号量，可以有效地控制并发任务的执行，确保系统的稳定性和性能。