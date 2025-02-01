##### Level.3
[YouTube](https://youtu.be/wdzVQuZfZUw)
##### Stable Diffusionの基本
![[BeginningLLMLevel3-1.png]]
- Text Encoder 
	- プロンプトとしてTextをユーザから受け取る
	- Stable Diffusionが理解可能なベクトルデータに変換
	- CLIPからその機能を借用
- CLIP
	- OpenAIが2021に発表
	- 画像にテキストラベルをつけてクラス分けするのが目的
	- 画像エンコーダーとテキストエンコーダーの事前学習により、画像とテキストのペアを予測
- 拡散モデル
	- ノイズ画像から少しずつノイズを取り除くことできれいな画像を生成
- VAE
	- 変分オートエンコーダー
	- 潜在空間と画像を変換するアルゴリズム
	- Stable Diffusionでは、img2imgにおける入力画像の変換と、画像の最終的な仕上げに使用されている

より詳細でわかりやすい解説
- https://qiita.com/omiita/items/ecf8d60466c50ae8295b
- https://romptn.com/article/11036

##### 用語について
- Checkpoint
	- 訓練済みモデルファイル
- LoRA
	- Low-Rank Adaptation
	- 低コストで追加学習
	- 特定のタスクや用途に合わせて学習(強化学習)
- SDXL
	- デフォルトよりも上位互換のモデル
	- パラメータ数倍以上
	- 生成サイズ拡大
- Sampling Method
	- 拡散過程(？)でのノイズ除去で使用するサンプリング手法に種類があり、結果が変わる

##### Level. 4
[YouTube](https://youtu.be/3oL3m2L-koo)
Create.xyzのハンズオン

##### Level. 5
[YouTube](https://youtu.be/is06pafZMqA)
Create.xyzのハンズオン

##### Level.6
[YouTube]()
RAG × NotebookLM