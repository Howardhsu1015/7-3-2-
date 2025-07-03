import google.generativeai as genai
from chromadb import Client
from chromadb.config import Settings
from chromadb.utils.embedding_functions import EmbeddingFunction

# 初始化 Gemini API
genai.configure(api_key="你的 GEMINI_API_KEY")  # 或用 Colab 的 `userdata.get`

class GeminiEmbeddingFunction(EmbeddingFunction):
    def __init__(self, model_name="models/embedding-001"):
        self.model = genai.GenerativeModel(model_name)

    def __call__(self, texts):
        # 接收 list[str] 回傳 list[float vector]
        embeddings = []
        for text in texts:
            response = self.model.embed_content(
                content=text,
                task_type="retrieval_document"
            )
            embeddings.append(response["embedding"])
        return embeddings

