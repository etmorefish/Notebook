
# 构建简易中文搜索引擎：Python 实践指南

在信息爆炸的时代，能够快速地从大量文本中找到所需信息变得尤为重要。本文将介绍如何使用Python，结合分词库`jieba`和其他基础库，实现一个简易的中文搜索引擎。这个搜索引擎能够处理Markdown文档，支持多关键字查询，并以TF-IDF算法为基础计算文档的相关性得分。

## 准备工作

首先，确保你的环境中已安装了`jieba`和`markdown2`库。`jieba`是一个优秀的中文分词库，而`markdown2`能够帮助我们将Markdown格式的文本转换为纯文本。

```bash
pip install jieba markdown2
```


## 实现步骤

### 1. 搜索引擎框架

搜索引擎的核心在于其数据结构和算法。我们的搜索引擎将包含以下几个部分：

- **文档处理**：读取文档，将Markdown格式转换为纯文本，并进行分词。
- **建立索引**：创建倒排索引，便于快速查找包含特定词汇的文档。
- **计算得分**：通过TF-IDF算法计算查询词与文档的相关性得分。
- **排序与展示**：根据得分排序，并提供文档预览。

### 2. 代码实现

#### 初始化搜索引擎

我们定义了`ChineseSearchEngine`类，用于存储文档内容、路径、倒排索引等信息，并实现了添加文档和搜索功能。

```python
import os
import jieba
import markdown2
from collections import defaultdict, Counter
import math

class ChineseSearchEngine:
    def __init__(self):
        self.documents = {}
        self.document_paths = {}
        self.inverted_index = defaultdict(set)
        self.doc_term_freq = defaultdict(Counter)
        self.doc_count = 0

    # 添加文档、建立索引等方法...
```

#### 添加文档到搜索引擎

我们实现了`add_document`方法，用于分词并更新倒排索引和文档词频。

```python
def add_document(self, doc_id, text, path):
    self.doc_count += 1
    self.documents[doc_id] = text
    self.document_paths[doc_id] = path
    tokens = list(jieba.cut_for_search(text))
    unique_tokens = set(tokens)
    for token in unique_tokens:
        self.inverted_index[token].add(doc_id)
    self.doc_term_freq[doc_id] = Counter(tokens)
```

#### 搜索实现

`search`方法接收用户的查询请求，分词后在倒排索引中查找匹配的文档，并计算TF-IDF得分，最后返回排序后的结果。

```python
def search(self, query):
    query_tokens = [token for part in query.split() for token in jieba.cut(part, cut_all=False)]
    doc_scores = defaultdict(float)
    # 计算得分...
    results = [(doc_id, self.document_paths[doc_id], score, self.get_text_preview(doc_id, query)) for doc_id, score in sorted(doc_scores.items(), key=lambda item: item[1], reverse=True)]
    return results
```

#### 文档预览

`get_text_preview`方法用于生成文档的预览片段，展示查询词在文档中的上下文。

```python
def get_text_preview(self, doc_id, query):
    text = self.documents[doc_id]
    query_tokens = list(jieba.cut(query))
    positions = []
    # 查找关键词位置...
    preview_start = max(0, best_start - 20)
    preview_end = min(len(text), best_end + 20)
    return text[preview_start:preview_end]
```

### 3. 使用示例

最后，我们通过遍历指定目录，将Markdown文件添加到搜索引擎，并等待用户

输入查询关键字进行搜索。

```python
def main():
    engine = ChineseSearchEngine()
    add_markdown_documents_to_search_engine(engine, 'YourDocumentsPath')
    # 用户输入查询关键字并展示搜索结果...
```

## 演示

```bash
Building prefix dict from the default dictionary ...
Loading model from cache C:\Users\maol\AppData\Local\Temp\jieba.cache
Loading model cost 0.745 seconds.
Prefix dict has been built successfully.
请输入搜索的关键字：清明
1 ['清明']
为您搜索到 3 条结果，耗时 0.0011067390441894531s.
found: ('pozhenzi_yanshu.md', 'SchoolChinese\\r\\91\\pozhenzi_yanshu.md', 0.03928722042561284, 'ote>\n\n<p>燕子来时新社，梨花落后清明。池上碧苔三四点，叶底黄鹂一两声。日长飞')
found: ('qiushengfu.md', 'SchoolChinese\\s\\4\\qiushengfu.md', 0.009188896424129019, '盖夫秋之为状也：其色惨淡，烟霏云敛；其容清明，天高日晶；其气栗冽，砭人肌骨；其意萧条')
found: ('24节气含义.md', 'SchoolChinese\\c\\24节气含义.md', 0.0064491584618258935, '减小。</p></li>\n<li><p>清明 (qīng míng) - 春季扫墓：')
请输入搜索的关键字：故乡
1 ['故乡']
为您搜索到 4 条结果，耗时 0.0s.
found: ('shangshan.md', 'SchoolChinese\\r\\91\\shangshan.md', 0.03701024541220253, 'quote>\n\n<p>晨起动征铎，客行悲故乡。</p>\n\n<p>鸡声茅店月，人迹板桥')
found: ('bieyunjian.md', 'SchoolChinese\\r\\92\\bieyunjian.md', 0.03627004050395849, '地宽。</p>\n\n<p>已知泉路近，欲别故乡难。</p>\n\n<p>毅魄归来日，灵旗空')
found: ('jingmen.md', 'SchoolChinese\\r\\81\\jingmen.md', 0.03591093119203811, '天镜，云生结海楼。</p>\n\n<p>仍怜故乡水，万里送行舟。</p>\n')
found: ('mulanshi.md', 'SchoolChinese\\r\\72\\mulanshi.md', 0.011125779295692789, '所欲，木兰不用尚书郎，愿驰千里足，送儿还故乡。</p>\n\n<p>爷娘闻女来，出郭相扶')
```

## 结语

通过以上步骤，我们实现了一个基础的中文搜索引擎，它能够处理Markdown文档并支持多关键字查询。虽然这只是一个简单的实现，但它为理解搜索引擎的基本原理和实现方法奠定了基础。未来，你可以在此基础上添加更多功能，如支持不同的文档格式、优化索引结构、实现更复杂的排名算法等。



> ### 完整代码

```python
import os
import time

import jieba
import markdown2  # 用于将Markdown文本转换为纯文本
from collections import defaultdict, Counter
import math


class ChineseSearchEngine:
    def __init__(self):
        # 初始化文档、文档路径、倒排索引、文档词频、文档总数
        self.documents = {}  # 存储文档ID到文档内容的映射
        self.document_paths = {}  # 存储文档ID到文档路径的映射，便于定位文档
        self.inverted_index = defaultdict(set)  # 倒排索引，存储词到文档ID集合的映射
        self.doc_term_freq = defaultdict(Counter)  # 文档词频，存储每个文档中各词的出现频率
        self.doc_count = 0  # 文档总数

    def add_document(self, doc_id, text, path):
        """
        为新文档更新文档总数、内容和路径
        对文档内容进行分词并更新倒排索引和词频
        """
        self.doc_count += 1
        self.documents[doc_id] = text
        self.document_paths[doc_id] = path  # 保存文档路径
        # tokens = list(jieba.cut(text, cut_all=True))  # 对文档内容进行分词
        tokens = list(jieba.cut_for_search(text))  # 搜索引擎模式
        unique_tokens = set(tokens)
        for token in unique_tokens:
            self.inverted_index[token].add(doc_id)
        self.doc_term_freq[doc_id] = Counter(tokens)

    def compute_idf(self, term):
        """计算逆文档频率(IDF)，用于评估词的重要性"""
        return math.log((1 + self.doc_count) / (1 + len(self.inverted_index[term])))

    def search(self, query):
        """搜索功能，支持多关键字查询"""
        query_tokens = []
        for part in query.split():  # 首先按空格拆分查询，然后对每个部分进行分词
            query_tokens.extend(list(jieba.cut(part, cut_all=False)))

        doc_scores = defaultdict(float)  # 存储每个文档的得分
        print(len(query_tokens), query_tokens)
        # print(len(self.inverted_index), self.inverted_index)
        for token in query_tokens:
            if token in self.inverted_index:
                idf = self.compute_idf(token)
                for doc_id in self.inverted_index[token]:
                    tf = self.doc_term_freq[doc_id][token] / sum(self.doc_term_freq[doc_id].values())
                    doc_scores[doc_id] += (tf * idf)  # 计算TF-IDF得分

        results = []
        for doc_id, score in sorted(doc_scores.items(), key=lambda item: item[1], reverse=True):
            text_preview = self.get_text_preview(doc_id, query)  # 获取文本预览
            results.append((doc_id, self.document_paths[doc_id], score, text_preview))
        return results

    def get_text_preview(self, doc_id, query):
        """
        返回与查询词匹配的文档中的一段文本。
        这个实现尝试找到所有关键词匹配的最佳覆盖范围。
        """
        text = self.documents[doc_id]
        query_tokens = list(jieba.cut(query))
        positions = []

        # 找到每个关键词在文档中的所有位置
        for token in query_tokens:
            start_pos = 0
            while True:
                start_pos = text.find(token, start_pos)
                if start_pos == -1:  # 没有找到匹配
                    break
                positions.append((start_pos, start_pos + len(token)))
                start_pos += len(token)  # 继续搜索下一个匹配

        if not positions:
            return ""  # 如果没有找到任何匹配项，返回空字符串

        # 将所有匹配位置排序，并尝试找到最紧凑的覆盖范围
        positions.sort()
        best_start, best_end = positions[0][0], positions[0][1]

        for start, end in positions:
            if start <= best_end:  # 扩展当前的范围
                best_end = max(best_end, end)
            else:  # 发现了一个新的范围，但我们只使用第一个最紧凑的范围
                break

        # 尽量显示前后文本，但要确保不超出文档的范围
        preview_start = max(0, best_start - 20)
        preview_end = min(len(text), best_end + 20)
        return text[preview_start:preview_end]


def add_markdown_documents_to_search_engine(search_engine, directory_path):
    """遍历指定目录，将Markdown文件添加到搜索引擎"""
    for root, dirs, files in os.walk(directory_path):
        for filename in files:
            if filename.endswith('.md'):
                file_path = os.path.join(root, filename)
                with open(file_path, 'r', encoding='utf-8') as file:
                    markdown_text = file.read()
                    # 转换Markdown为纯文本
                    text = markdown2.markdown(markdown_text)
                    search_engine.add_document(filename, text, file_path)  # 注意这里新增了路径参数


def main():
    """主搜索函数，接收用户输入的关键字进行搜索"""
    engine = ChineseSearchEngine()
    add_markdown_documents_to_search_engine(engine, 'SchoolChinese')
    # print(f"len = {len(engine.inverted_index)}, freq: {engine.doc_term_freq}")
    while True:
        keyword = input("请输入搜索的关键字：")
        t1 = time.time()
        results = engine.search(keyword)
        t2 = time.time()
        print(f"为您搜索到 {len(results)} 条结果，耗时 {t2 - t1}s.")
        for i in results:
            print(f"found: {i}")


if "__main__" == __name__:
    main()
```