# 7-3-2-
def split_text(text, chunk_size=500, overlap=50):
    """
    將全文本切成小段落（chunk），每段限制字數上限，支援重疊。
    
    參數:
        text (str): PDF 中提取的整段文字
        chunk_size (int): 每段的最大字數
        overlap (int): 每段之間的重疊字數，可提升語意連貫性
    
    回傳:
        List[str]: 分割後的小段落清單
    """
    paragraphs = text.split('\n\n')
    chunks = []
    current_chunk = ""

    for para in paragraphs:
        if len(current_chunk) + len(para) < chunk_size:
            current_chunk += para + "\n\n"
        else:
            chunks.append(current_chunk.strip())
            # 加入重疊（重疊最後一小段）
            current_chunk = para[-overlap:] + "\n\n"

    if current_chunk:
        chunks.append(current_chunk.strip())

    return chunks
