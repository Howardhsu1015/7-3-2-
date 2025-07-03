def make_rag_prompt(query, relevant_passages):
    """
    建立 Gemini 使用的 RAG prompt，整合檢索段落與使用者問題。

    參數:
        query (str): 使用者輸入的問題
        relevant_passages (List[str]): 與問題最相關的段落列表

    回傳:
        str: 最終送給 LLM 的完整提示語
    """
    # 將相關段落合併成單一字串
    context = "\n\n".join(relevant_passages)

    # 組合成 Prompt 格式
    prompt = (
        f"根據以下資訊，請詳細回答問題：\n\n"
        f"{context}\n\n"
        f"問題：{query}"
    )

    return prompt

