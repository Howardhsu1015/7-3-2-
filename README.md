import chromadb
import os

def create_chroma_db(documents, path, name):
    """
    建立 Chroma 向量資料庫並加入文件嵌入向量。

    參數:
        documents (List[str]): 切割後的文本清單
        path (str): 資料庫儲存路徑
        name (str): collection 名稱

    回傳:
        chromadb.Client: 可進行查詢的資料庫物件
    """
    # 初始化 ChromaDB
    if not os.path.exists(path):
        os.makedirs(path)

    client = chromadb.Client(Settings(chroma_db_impl="duckdb+parquet", persist_directory=path))

    # 建立或取得 collection
    embed_fn = GeminiEmbeddingFunction()
    collection = client.get_or_create_collection(name=name, embedding_function=embed_fn)

    # 為每段文字建立唯一 ID
    ids = [f"doc_{i}" for i in range(len(documents))]

    # 加入至資料庫
    collection.add(documents=documents, ids=ids)

    print(f"✅ 已加入 {len(documents)} 筆文件至 Chroma 資料庫：{name}")
    return client

