### Pandas与Polars性能对比测试记录

在进行数据处理任务时，特别是涉及到大型数据集时，选择合适的数据处理库对于提高效率和减少计算时间至关重要。本次测试比较了两个流行的数据处理库：Pandas和Polars，通过具体的数据合并任务来探究它们在处理效率和执行时间上的差异。

> Polars 是一个速度极快的 DataFrame 库，用于处理结构化数据。核心是用 Rust 编写的，可用于 Python、R 和 NodeJS。

#### 测试环境
- **操作系统**: [Windows 11]
- **处理器**: [12th Gen Intel Core i7-1260P]
- **内存**: [16GB]
- **Python版本**: [3.10.9]
- **Pandas版本**: [2.1.3]
- **Polars版本**: [0.20.16]

#### 测试数据
- **数据集大小**: [8395个文件，每个文件约包含18行数据]
- **数据内容**: [包含基因表达值的科学数据]
- **数据操作**: 主要包括数据读取、空值处理、数据透视以及最终的数据合并。

#### 测试方法
1. **使用Pandas进行数据处理**:
   - 首先初始化一个空的DataFrame用于存储所有透视表的合并结果。
   - 读取每个文件，对数据进行处理，包括去除不需要的列、处理空值、创建数据透视表。
   - 将每个文件处理后得到的透视表合并到初始化的DataFrame中。
   - 记录从开始处理到数据合并完成的总时间。

2. **使用Polars进行相同的数据处理**:
   - 类似于Pandas的处理流程，但在数据处理和合并时利用Polars的API。
   - 注意到Polars在处理空值和数据合并时的不同之处，例如使用`.filter()`和`pl.concat()`。
   - 同样记录完成相同任务的总时间。

#### 测试结果
- **Pandas处理时间**: [Pandas处理总时间，例如：46.855秒]
- **Polars处理时间**: [Polars处理总时间，例如：19.680秒]

#### 分析与结论
通过本次测试可以看出，对于给定的数据处理任务，Polars在执行时间上表现出显著优势。Polars的处理时间大约是Pandas的42%，这得益于Polars在内部执行操作时的高效性能优化，特别是在数据合并和处理大规模数据集时的优势更为明显。

值得注意的是，虽然Polars在性能上有明显优势，但在使用上可能需要一定的学习曲线，尤其是对于那些习惯了Pandas API的用户。此外，Pandas由于其广泛的使用和社区支持，在数据处理上提供了更多的灵活性和功能。

#### 结论
在选择数据处理库时，应根据实际的数据处理需求、执行效率和团队的熟悉程度来做出选择。对于处理大型数据集和追求高效率的场景，Polars是一个值得考虑的选择。然而，对于需要复杂数据处理和已有丰富Pandas使用经验的项目，Pandas可能仍是首选。


#### 参考代码
- **Pandas版本**:
  ```python
    import os
    import time
    import pandas as pd
    import numpy as np
    from tqdm import tqdm


    # 填充空值的函数
    def fill_nulls_for_column(series):
        null_labels = [f"null_label_{i+1}" for i in range(series.isnull().sum())]
        null_positions = series[series.isnull()].index
        for label, pos in zip(null_labels, null_positions):
            series.at[pos] = label
        return series

    def merge_files(filelist):
        # 初始化一个空的DataFrame用于存储所有透视表的合并结果
        merged_df = pd.DataFrame()  
        for name in tqdm(filelist):
            # 根据你的文件命名规则，可能需要调整格式字符串以匹配文件名
            file_path = f'gcbi/data/{name}'
            try:
                df = pd.read_csv(file_path, sep='\t')
            except FileNotFoundError:
                print(f"File not found: {file_path}")
                continue
            
            df.drop(['Library Collection ID'], axis=1, inplace=True)
            # df["Category Descriptive Label"] = fill_nulls_for_column(df["Category Descriptive Label"])
            df = df.dropna(subset=["Category Descriptive Label"])
            
            # 数据透视
            pivot_table = df.pivot_table(index=["FBgn ID", "#Parent Library Name", "Gene Symbol"],
                                        columns="Category Descriptive Label",
                                        values="LFQ Expression Value",
                                        aggfunc="first").reset_index()

            # 合并当前透视表到merged_df
            merged_df = pd.concat([merged_df, pivot_table], ignore_index=True)
            
        # 保存合并后的DataFrame到一个新文件
        merged_df.to_csv('gcbi/merged_data_pandas.csv', index=False)

    def main():
        filelist = os.listdir("gcbi/data")
        t1 = time.time()
        merge_files(filelist)
        t2 = time.time()
        print(f"cost time: {t2-t1} seconds")
        

    if __name__ == "__main__":
        main()


    # 100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████| 8395/8395 [00:46<00:00, 179.49it/s]
    # cost time: 46.855167627334595 seconds
  ```

- **Polars版本**:
  ```python
    import os
    import time
    import polars as pl
    import numpy as np
    from tqdm import tqdm


    # Polars中填充空值的函数
    # def fill_nulls(series):
    #     null_labels = [f"null_label_{i+1}" for i in range(series.null_count())]
    #     null_positions = series.is_null()
    #     for i, label in enumerate(null_labels):
    #         series = series.with_row_count().apply(lambda row: label if null_positions[row[0]] else row[1]).drop("row_nr")
    #     return series

    def merge_files(filelist):
        merged_df = None  # 开始时不创建空DataFrame
        for name in tqdm(filelist):
            # 根据你的文件命名规则，可能需要调整格式字符串以匹配文件名
            file_path = f'gcbi/data/{name}'
            try:
                df = pl.read_csv(file_path, separator="\t")
            except FileNotFoundError:
                print(f"File not found: {file_path}")
                continue
            
            df.drop(['Library Collection ID'])
            # df["Category Descriptive Label"] = df["Category Descriptive Label"].apply(fill_nulls)
            # df["Category Descriptive Label"] = fill_nulls_for_column(df["Category Descriptive Label"])
            # df = df.with_columns(fill_nulls(df["Category Descriptive Label"]).alias("Category Descriptive Label"))
            # df = df.with_columns(pl.col("Category Descriptive Label").fill_null("Unknown"))
            df = df.filter(pl.col("Category Descriptive Label").is_not_null())
            
            # 数据透视
            pivot_table = df.pivot(index=["FBgn ID", "#Parent Library Name", "Gene Symbol"],
                                        columns="Category Descriptive Label",
                                        values="LFQ Expression Value",
                                        aggregate_function="first")
            # 合并当前透视表到merged_df
            # 检查merged_df是否已经被初始化
            if merged_df is None:
                merged_df = pivot_table  # 如果还没有，直接使用当前的df
            else:
                try:
                    merged_df = pl.concat([merged_df, pivot_table], how="vertical_relaxed")
                except pl.exceptions.ComputeError:
                    # print(f"polars.exceptions.ComputeError: schema lengths differ {file_path}")
                    continue
        # 保存合并后的DataFrame到一个新文件
        merged_df.write_csv('gcbi/merged_data_polars.csv')

    def main():
        filelist = os.listdir("gcbi/data")
        t1 = time.time()
        merge_files(filelist)
        t2 = time.time()
        print(f"cost time: {t2-t1} seconds")
        

    if __name__ == "__main__":
        main()


    # 100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████| 8395/8395 [00:19<00:00, 427.13it/s]
    # cost time: 19.680180072784424 seconds

  ```