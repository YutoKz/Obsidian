Twitter タグ：RAG_findy

ナレッジセンス　門脇さん
1. RAGの全体像
2. 90%の人には最新手法は不要
3. 最新手法

[会社のZenn](https://zenn.dev/p/knowledgesense)にてRAG手法の解説を投稿中
資料もtwitterから探す
株式会社ナレッジセンス代表取締役　門脇 敦司　@at_sushi_

![[302837092-54a2d76c-b07e-49e7-b4ce-fc45667360a1.png]]


AdvancedなRAGはどういうときに有用なのか
- ベクトルDBに存在しない内容については回答できない
	- CRAGで変化球に対応
		- retrievalのカテゴリ
	- Corrective RAG
		- 取得した文書の質を評価し、必要に応じてWEB検索で対応
- 文字を含んでいても単なる画像では検索できない
	- DSEで様々な形式のデータに対応
	- indexingのカテゴリ
- チャンク横断的な知識をうまく結びつけられない
	- クソでかい表は分割され一部のみ持ってきちゃう
	- Hybrid RAG


その他手法
- リランキング
- フィルタリング
	- top-kを後フィルタリング
	- metadata等で前フィルタリングしてtop-k





株式会社アルファシステムズ　経営企画本部 AI推進室 室長　鏡味 秀行　@kagamih
