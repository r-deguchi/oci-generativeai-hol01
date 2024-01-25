# PoCテストケース
RAG構成のパーツとなる以下2項目の処理のフローを理解する
- Generative AIサービスでの埋め込み処理
- AI Vector Search LA1でのセマンティック検索

![image](https://github.com/kenkensonson/oci-generativeai-hol01/assets/30405234/9efcfaf0-8cfa-413d-898f-577d0ac3b4f7)

![image](https://github.com/kenkensonson/oci-generativeai-hol01/assets/30405234/ceb34b69-b594-458f-8d7e-855826a3611a)


# ①テスト環境のプロビジョニング
### Oracle Database用 compute インスタンス
### AI Vector Search
### Generative AI Service



# ②サンプルデータの確認
PoC Phase2以降でドキュメントローダー、テキストスプリッターはAI Vector Search LA2の機能を利用することを想定し、Phase1では単文が複数並んでいるシンプルなテキストデータをサンプルデータセットとします。


# ③サンプルデータの埋め込みベクトルの生成
Generative AI Embeddingモデル(embed-multilingual-v3.0)によりサンプルデータのベクトル表現を取得します。

```
import oci

CONFIG_PROFILE = "DEFAULT"
config = oci.config.from_file('~/.oci/config', CONFIG_PROFILE)

# Service endpoint
endpoint = "https://inference.generativeai.us-chicago-1.oci.oraclecloud.com"
generative_ai_inference_client = oci.generative_ai_inference.GenerativeAiInferenceClient(config=config, service_endpoint=endpoint, retry_strategy=oci.retry.NoneRetryStrategy(), timeout=(10,240))
inputs = ["Learn about the Employee Stock Purchase Plan","Reassign timecard approvals during leave","View my payslip online",,,,,]
embed_text_detail = oci.generative_ai_inference.models.EmbedTextDetails()
embed_text_detail.serving_mode = oci.generative_ai_inference.models.OnDemandServingMode(model_id="cohere.embed-multilingual-light-v3.0")
embed_text_detail.inputs = inputs
embed_text_detail.truncate = "NONE"
embed_text_detail.compartment_id = "ocid1.compartment.oc1..aaaaaaaapq43ke3tgcc4cd65teeeccrd6rb4qyqfpllyutrau4j2bijwewlq"
embed_text_response = generative_ai_inference_client.embed_text(embed_text_detail)
# Print result
print("**************************Embed Texts Result**************************")
print(embed_text_response.data)
```

# ④データロード
GenAI 埋め込み処理で得られたベクトルデータをOracle Databaseにロードします。


# ⑤クエリ用テキストの埋め込みベクトルの生成
クエリ用のテキストのうめ込みベクトルを生成します。


# ⑥セマンティック検索の実行

