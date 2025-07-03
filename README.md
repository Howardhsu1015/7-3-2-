def get_relevant_passage(query, db, n_results=3):
    """
    從 ChromaDB 中根據 query 查詢最相關的文字段落。

    參數:
        query (str): 使用者輸入的問題或查詢字串
        db (chromadb.Client): Chroma 資料庫物件
        n_results (int): 要回傳的最相似段落數量

    回傳:
        List[str]: 最相關的文字段落清單
    """
    try:
        # ⚠️ 假設你只有一個 collection（例如叫 pdf_chunks）
        collection = db.get_collection(name="pdf_chunks")

        results = collection.query(
            query_texts=[query],
            n_results=n_results
        )

        # 取出文件內容
        relevant_texts = results["documents"][0]
        return relevant_texts

    except Exception as e:
        print(f"❌ 查詢發生錯誤：{e}")
        return []
