2024年5月~
Spin Qubit周りの**Automation**をテーマに論文調査.
「他研究グループの取り組みを踏まえ、日立でやりたいことを見つけたい」
モデルの出力を実験にフィードバックしてCalibrationしたり、Screeningしたりも

--- 
### 動向
###### tuningの目的
- 目標の電子数を閉じ込めるのに必要なゲート電圧の特定 (Charge State Autotuning)
	- 手動でゲート電圧を調整するのは、今後のビット数増加を考えると非現実的
	- scalableな自動化手法が必要
###### tuningへのニーズ
- scalabilityを鑑み、測定時間を抑えるため解像度が低く済むようにしたい
###### 手法の傾向
- 機械学習
	- モデル
		- 画像の分類やパターン認識にCNN
		- binary classifier
		- FFNN
	- 学習データ
		- シミュ＋実験が有効
- Script-based
	- 
###### 前処理の必要性
- ノイズ低減により、続くエッジ検出/セグメンテーション/パターン認識等の精度向上

---
### 論文
#### [Noise Reduction Methods for Charge Stability Diagrams of Double Quantum Dots](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=9754273)
###### どんなもの？
- CSDのノイズ除去手法の調査
- 2量子ドットのtuningにハニカム構造が重要だが、人力は時間かかる
- 加えて、低コントラストとノイズはtuningをより困難にする
- そこで、多数のノイズ除去フィルタを定量的定性的に評価・比較
###### 先行研究からの改善
- 既存のエッジ平滑化フィルタ群を評価・比較し、auto-tuningに最適なものを探索
###### 技術・手法の肝
- エッジ平滑化フィルタ
	- もとの形状を保存しつつノイズをならす
	- 種類
		- bilateral filters
		- anisotropic diffusion
		- nonlinear Gaussian filter chains
		- block-matching and 3D transform-domain filtering (BM3D)
		- rolling guidance filter (RGF)
- ノイズ低減効率とエッジ保存に関する性能指標
	- シミュデータ：PSNR, SSIM
	- 実験データ：RSC, RES
	- PSNR: Peak Signal-to-Noise Ratio
		- A metric that measures the quality of reconstruction or compression of an image by comparing the peak value of the signal to the noise level.
	- SSIM: Structural Similarity Index Measure
		- A metric that compares two images based on their structural similarity, considering aspects like luminance, contrast, and structure to measure perceptual quality.
	- RSC: Relative Standard Deviation Change
		- In this context, it measures the noise reduction achieved by a filter, calculated based on the standard deviation change of noise before and after filtering.
	- RES: Relative Edge Sharpness
		- A custom measure used to assess how well a filter maintains or enhances the sharpness of edges in an image compared to the original.
###### 有効性の検証方法
- 多様なノイズをシミュレーションで生成、各フィルタを適用しノイズ低減とエッジ保存の性能を評価
- 実験データにも適用、性能を確かめる
###### 議論
- BM3DとRGFがauto-tuningの前処理に有望
###### 次に読むべき論文
- [11-14] 画像の分類やパターン認識にCNNを使用
  [11], [15] binary classifier を比較 (ロジスティック回帰, 多層パーセプトロン,  …..)
- [11] J. Darulova, M. Troyer, and M. C. Cassidy, “Evaluation of synthetic and experimental training data in supervised machine learning applied to charge state detection of quantum dots,” Mach. Learning: Sci. Technol., vol. 2, no. 4, 2021, Art. no. 045023, doi: 10.1103/PhysRevApplied.13.054019.
- [12] S. S. Kalantre et al., “Machine learning techniques for state recognition and auto-tuning in quantum dots,” NPJ Quantum Inf., vol. 5, no. 1, Dec. 2019, Art. no. 6, doi: 10.1038/s41534-018-0118-7. これ引用多そう
- [13] R. Durrer et al., “Automated tuning of double quantum dots into specific charge states using neural networks,” Phys. Rev. Appl., vol. 13, no. 5, May 2020, Art. no. 054019, doi: 10.1103/PhysRevApplied.13.054019.
- [14] J. P. Zwolak et al., “Autotuning of double-dot devices in situ with machine learning,” Phys. Rev. Appl., vol. 13, no. 3, Mar. 2020, Art. no. 34075, doi: 10.1103/PhysRevApplied.13.034075.
- [15] J. Darulová et al., “Autonomous tuning and charge state detection of gate defined quantum dots,” Phys. Rev. Appl., vol. 13, no. 5, May 2020, Art. no. 054005, doi: 10.1103/PhysRevApplied.13.054005.

#### [Miniaturizing neural networks for charge state autotuning in quantum dots](https://arxiv.org/abs/2101.03181v3)
###### どんなもの？
- 小さいNNを使ってAuto-tuningしたい
- 電荷状態の遷移(直線？)を検知
- 将来的なOn-chipを意識、低消費電力でも動作する効率性を目指す
###### 先行研究からの改善
- 機械学習を用いた先行研究
	- サポートベクターマシン
	- DNN
- 小規模なFFNNの使用を提案
###### 技術・手法の肝
- 小規模FFNN
- CSDから切り取った小さな領域(patch)が直線を含むかをクラス分け
- 学習はシミュデータ
- テストは実験データ、良さげ
- 直線クラス分類結果をもとに、目的の電荷状態を探索するShifting algorithm
###### 有効性の検証方法
- シミュで学習 => 前処理済み実験データに適用
- 手動でのクラス分類と比較(正解ラベルとして？)、性能を評価
###### 議論
- 小規模FFNNでも正確に状態遷移検知可能、大規模NNに引けとらず
- 小規模ゆえ、On-chip可能
- モデルそのものに限らず、Auto-tuningの他の側面での小規模化についてさらなる研究の必要性
	- 信号処理
###### 重要視している先行研究
- [11]    [Machine learning techniques for state recognition and auto-tuning in quantum dots](https://www.nature.com/articles/s41534-018-0118-7)
- [15]    [Automated Tuning of Double Quantum Dots into Specific Charge States Using Neural Networks](https://journals.aps.org/prapplied/abstract/10.1103/PhysRevApplied.13.054019)
#### [Automated tuning of double quantum dots into specific charge states using neural networks](https://arxiv.org/abs/1912.02777)
	被引用数が多い印象。これ全文読みたい。
	2019なので、初期の研究等をここのIntroとかから見つけてもいいかも
###### どんなもの？
- DQDを特定の電荷状態にauto-tuningするアルゴリズムを提案
- 現在の電荷状態と目的の状態を達成するのに必要な電圧値の探索
- CNNを使用
- 実験データで学習
###### 背景/モチベ
###### 先行研究からの改善
- 先行研究での機械学習の使い方
	- パラメータ推定
	- 量子システムのtuning
- 提案手法
	- 特定の量子状態実現のための量子ドットtuningに特化
	- CNNの利用
###### 技術・手法の肝
- CNNを電荷状態遷移の検出に使用
- 2つのネットワークを使用した2stepプロセス
	1. 基準となる(0, 0)領域を探索
	2. 目的の電荷状態を実現する電圧を探索
- GaAs量子ドットの実験データで学習/テスト
	- 汎化性能向上のため、データ拡張も活用
###### 有効性の検証方法
- 学習データのaccuracy: 98.9% and 96%
- テスト結果: 57%
###### 議論
- 人力の必要性を確実に減らしている
- NNを繰り返し適用した際の累積誤差が全体の精度に悪影響
- ノイズ低減と高解像度化?が精度向上に寄与しそう
###### 重要視している先行研究
- [22]-[27]機械学習でパラメータ推定/量子システムのtuning
- [22]    [Efficiently measuring a quantum device using machine learning (2019)](https://www.nature.com/articles/s41534-019-0193-4)
- [23]    [“Quantum parameter estimation with a neural network,” arXiv preprint arXiv:1711.05238 (2017).](https://arxiv.org/pdf/1711.05238)
- [24]    [A machine learning approach for automated fine-tuning of semiconductor spin qubits (2019)](https://arxiv.org/abs/1901.01972)
- [25]    [Machine learning for discriminatingquantum measurement trajectories and improving readout (2015)](https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.114.200501)
- [26]    [Autonomous tuning and charge state detection of gate defined quantum dots (2019)](https://arxiv.org/abs/1911.10709)
- [27]    [Machine learning techniques for state recognition and auto-tuning in quantum dots (2019)](https://www.nature.com/articles/s41534-018-0118-7)
#### [Evaluation of Line Detection Methods for the Analysis of Charge Stability Diagrams](https://juser.fz-juelich.de/record/1009243/files/J%C3%BCl%204441_Scherer%2C%20Benedikt.pdf)
###### どんなもの？
- CSDの解析に向け、複数の直線検知手法の評価
###### 背景/モチベ
- 量子ドットで実装した量子ビットは、電子の位置を正確に制御する必要あり
	- 電極の電圧調整
- 人力はscalableでない
- 効率的なキャリブレーションのためrobustかつautomaticな直線検知手法が必要
- CSDのもつ多様な品質やノイズにrobustな手法を探索する
###### 先行研究からの改善
- 既存の直線検知手法と、それを古典的で機械学習を使わない方法で強化する方法を提案
- 上記の手法の中から最適な組み合わせを見つけることを目的とする
- 性能評価には独自の方法論
	- 出力と人力ラベルの比較
###### 技術・手法の肝
- 後処理についても検討
- 2つの独自の評価指標を提案
###### 有効性の検証方法
- 新規の評価指標2つ
	- edge mapの比較
	- 検知した直線のベンチマーク
###### 議論
- 複数の検知手法の適用結果を多様な品質のCSDで試す
###### 重要視している先行研究
既存手法の改善による、CSDの分析に特化した検知手法構築のため、手法提案論文が良く引用
#### [Autonomous tuning and charge state detection of gate defined quantum dots](https://arxiv.org/pdf/1911.10709)
	全文読みたい。
	量子ドットの特性評価が特徴的で、初見。
###### どんなもの？
- 機械学習でシリコン量子ドットをcharacterize(特性評価) and tune
- 主目的はscalability向上に向けた自動化
###### 背景/モチベ
 - 教師有学習で実験者の役割を取って代わる
 - 量子ドットシステムのscalabilityと信頼性の向上を目指す
###### 先行研究からの改善
- 先行研究として、部分的に自動化されたtuningプロセスをベースとする
	- 課題は、すでにtuning済みの単一量子ドットであることと、人力が残存
- 本研究はこれを複数ドットに拡張、前処理や人力も不要に
###### 技術・手法の肝
- characterization 特性評価
	1. きちんと電流が流れるかどうか
	2. 各ゲートを1D測定して個別評価、応答品質を学習済みの2値分類器で評価
		- 電流応答が良好か悪いか、良好なもののみtuningへ進む
- tuning
	1. ゲート電圧を調整し、量子ドットを定義、必要な電子数を実現
###### 有効性の検証方法
- 大量の実験
###### 議論
###### 重要視している先行研究
- 機械学習を用いた量子ドットのtuning
	- [20]    [20Machine learning techniques for state recognition and auto-tuning in quantum dots](https://www.nature.com/articles/s41534-018-0118-7)
	- [21]    [A machine-learning approach to the charge states in quantum dot experiments](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0205844)
	- [22]    [Auto-tuning of double dot devices in situ with machine learning](https://arxiv.org/abs/1909.08030)
#### [Image Processing Algorithms for Tuning Quantum Devices and Nitrogen-Vacancy Imaging](https://www.osti.gov/biblio/1662017)
	CSDの反交差に着目している点が特徴。
	また、磁界分布の測定手法、magnetic field imagingの話も。
###### どんなもの？
- 量子ドットのauto-tuningとimaging magnetic fields using nitrogen-vacancy(NV) centers in diamonds用の画像処理アルゴリズム
###### 背景/モチベ
- 量子ドットデバイスにおける量子ドット間の複雑な相関等により、その調整は困難を極める
- 近年、ダイアモンド中NV centersを用いたmagnetic field imagingの高解像度・高効率化への関心が高まっている。
###### 先行研究からの改善
###### 技術・手法の肝
- 画像処理により、CSD中の反交差を検知・分析
- 異なる磁界領域間の遷移を検知？するのにcross-correlationを使用
###### 有効性の検証方法
###### 議論
###### 重要視している先行研究
- [Computer-automated tuning procedures for semiconductor quantum dot arrays (2019)](https://arxiv.org/abs/1907.10775)
	- トリプル量子ドット？！

#### [Algorithm for automated tuning of a quantum dot into the single-electron regime](https://journals.aps.org/prb/abstract/10.1103/PhysRevB.102.085301)
	単一量子ドットのtuning
	2重量子ドットのtuningよりも難しいらしい。（データが構造的でない）
###### どんなもの？
- 単一量子ドットを電子数一つの領域にtuning
- charge sensing を使用
- measure => analyze => tune gate voltages を繰り返す
###### 背景/モチベ
- 現状人力で電子数を一つに
- これを自動化しtuningをシンプルにし促進する
###### 先行研究からの改善
- 先行研究・手法
	- 人力の調整にかなり依存
	- より単純な2重量子ドットを想定 <= ?
- 上記手法をベースに、単一量子ドットに対応した完全自動のアルゴリズムを提案
- なぜ単一の方が、単純な2重量子ドットよりむずいか
	- ノイズや構造的でない遷移(ハニカムではないみたいな？)
###### 技術・手法の詳細
- アルゴリズムの構成
	- 信号処理
		- 背景のノイズを除去し、電荷の遷移する電圧を特定
	- 画像解析
		- modified Hough transform and EDLines algorithmを使い、直線を2値画像から抽出
	- 測定の繰り返し
		- 単一電子閉じ込めまで、charge secsorのデータの測定と分析、ゲート電圧調整を繰り返す
###### 有効性の検証方法
- 実験データに適用
- 実際に生じる多様な状況で単一電子領域を特定可能と実証
###### 議論
- 他のautomationとの統合
###### 重要視している先行研究
- [Automated tuning of inter-dot tunnel coupling in double quantum dots (2018)](https://pubs.aip.org/aip/apl/article/113/3/033101/37032/Automated-tuning-of-inter-dot-tunnel-coupling-in)
- [Computer-automated tuning of semiconductor double quantum dots into the single-electron regime (2016)](https://pubs.aip.org/aip/apl/article/108/21/213104/30965/Computer-automated-tuning-of-semiconductor-double)
- [Computer-automated tuning procedures for semiconductor quantum dot arrays (2019)](https://pubs.aip.org/aip/apl/article/115/11/113501/1078186/Computer-automated-tuning-procedures-for)
#### [Computer-automated tuning of semiconductor double quantum dots into the single-electron regime (2016)](https://pubs.aip.org/aip/apl/article/108/21/213104/30965/Computer-automated-tuning-of-semiconductor-double)
	全文読みたい。
	デバイスとして、一直線に並んだ 4 量子ドットデバイスを使用。(3つのdouble dot?)
	(1, 1)への調整に目的を特化している印象。
	古いが、逆に新鮮な手法かも。
###### どんなもの？
- 二重量子ドットを単一電子領域にauto-tuningするアルゴリズムの実証
###### 背景/モチベ
- 従来人力だった量子ドットのtuningを自動化、大規模化を可能に
	- 量子ドット中電子数やトンネル障壁の緻密な制御が不可欠
###### 先行研究からの改善
- 従来は人力、時間コスト高 かつ 複雑さ故の一貫性のなさ が問題
- 自動化アルゴリズムの提案
	- 人間の仕事を減らしつつ、整合性の改善
###### 技術・手法の肝
- 適切なゲート電圧を設定
- 必要な情報は ゲートの配置 および 各ゲートのピンチオフ値 のみ
###### 有効性の検証方法
4量子ドットアレイにおける3つの2量子ドットを個別にtuningすることに成功、実証
各ドットが単一電子領域に入っていることを、CSDの確認 と センシングドットのtuningから保証
###### 議論
- 自動化の効果
	- 大規模な量子ドットアレイを効率的に制御できるように
	- 量子コンピュータ研究の促進
- 異なるセットアップ・構成にも適用可能な汎用性
###### 重要視している先行研究
#### [Visual explanations of machine learning model estimating charge states in quantum dots](https://arxiv.org/abs/2210.15070)
	例の東北大の人の論文
###### どんなもの？
- 量子ドットの電荷状態を推定する機械学習モデルの説明可能性向上を目的
- Grad-CAMを使用、入力のどの部分が出力に重要かを可視化
- モデルのブラックボックス化に一石を投じる
###### 背景/モチベ
- 量子ドットの目的電子状態へのtuningむずいよね、scalingにも非現実的
- これを機械学習で自動化したい
###### 先行研究からの改善
- 機械学習による電荷状態の認識手法が数々実証
	- 学習データに物理モデルに基づくシミュデータ
- 本研究
	- シンプルなシミュモデルと二値化プロセス
	- モデルの学習とデータ生成が、異なる実験環境でも容易に
###### 技術・手法の詳細
- シミュにシンプルな定相互作用モデル
- Grad-CAMで入力で重要な個所を可視化
- 実験データへの性能(ロバスト性)向上のため、学習用シミュデータにノイズ付与
- CNNで電荷状態認識
###### 有効性の検証方法
- 人力のクラスタリング結果と比較
- シミュと実験両方での検証
###### 議論
###### 重要視している先行研究

#### [Machine Learning techniques for state recognition and auto-tuning in quantum dots(2018)](https://arxiv.org/abs/1712.04914)
	全文読みたい。
	機械学習を初めて採用した、当時では先進的な論文であるという印象。
	少し古いが、電流から電荷状態を認識する機械学習もやろうとしてる？
###### どんなもの？
- deepなCNNを量子ドットアレイの電荷状態の認識と最適化に適用
- scalableな量子コンピュータに重要
###### 背景/モチベ
- 量子ドットの調整等には複雑な高次元パラメータ空間が課題
- 先行研究
	- 回帰分析と量子制御理論でtuning
	- しかし、アナログパラメータのfine-tuningしかできず
- 機械学習で解決しよう
###### 先行研究からの改善
- 先行研究の多くは人力か半自動でのtuningばかり
- 本研究はこれを機械学習で解決
- 電荷状態の認識とtuningを自動化、人力の工程なしで量子ドットの構成を決定
###### 技術・手法の詳細
- CNNでパターン認識
- 学習はシミュデータ
- 目的は実験環境でのゲート電圧の推定・最適化
###### 有効性の検証方法
- シミュと実験両方で検証
###### 議論
- 機械学習が有望
- 今後の方向性として、より複雑で高次元のシステムに拡張
- 一方、潜在的な課題として、現実のノイズやvariability(変動性)
###### 重要視している先行研究
#### [The Automated Bias Triangle Feature Extraction Framework(2023)](https://arxiv.org/abs/2312.03110)
	画像処理＋バイアストライアングル
###### どんなもの？
- 量子ドットのバイアストライアングルの特徴抽出を自動化
- 教師なし、セグメンテーションベースの画像処理手法
###### 背景/モチベ
- CSDの解析の複雑さ
- 既存手法
	- 人力
	- 大量のデータを学習に必要
	- これを本手法は教師ナシの自動手法で回避
###### 先行研究からの改善
###### 技術・手法の詳細
2 step
- Segmentation
	- rRDP: relaxed Ramer-Douglas-Peucker algorithm
	- MET: Minimum Enclosing Triangle method
- Feature Extraction & PSB-Detection
	- base linesやangleなどの幾何学的な特徴を抽出
	- これらを使いPauli Spin Blockadeを抽出
		- by comparing pixel intensities within segmented regions across different magnetic fields.
###### 有効性の検証方法
- シミュと実験データ両方で検証
- Dice coefficient や IoUでsegmentationの精度を評価
- PSBの検証はthe precision and recall in classifying PSB occurrences across different datasets
###### 議論
- 多様なデータセットに対する本フレームワークの効率性とロバスト性を主張
###### 重要視している先行研究
特徴抽出手法に関する論文も多く引用
- (6) [Identifying Pauli spin blockade using deep learning (2023).](https://quantum-journal.org/papers/q-2023-08-08-1077/)
- (12) [Colloquium: Advances in automation of quantum dot devices control (2023)](https://arxiv.org/abs/2112.09362)
- (13) [Machine learning techniques for state recognition and auto-tuning in quantum dots(2017)](https://www.nature.com/articles/s41534-018-0118-7)
- (18) [Dataset for ’Identifying Pauli spin blockade using deep learning’ (2023)](https://data.niaid.nih.gov/resources?id=ZENODO_7948851)
	- [元論文](https://arxiv.org/abs/2202.00574)
#### [Fully autonomous tuning of a spin qubit(2024/02)](https://arxiv.org/pdf/2402.03931)
	手法のところだけでも読みたい
	plunger gate voltage / barrier gate voltage ?
###### どんなもの？
- 初の完全自動tuning
- deep learningとベイズ最適化という、複数の機械学習を併用・統合
- 効率的かつ高精度なtuning
- ラビ振動の観測、量子ビットの実現を確認した
###### 背景/モチベ
- tuningにおける大規模化の壁
- パラメータ最適化の困難さ
	- パラメータ空間と製造誤差に起因
###### 先行研究からの改善
- 既存研究は、tuningプロセスのたった一工程を自動化するものばかり
	- 「目的のドット中電子数達成に必要なパラメータ最適化」
	- 完全な制御自動化ではない
- 提案手法
	- end-to-endな自動tuningプロセス
	- 実験者が数週間数カ月で行っていたのを数日に短縮
###### 技術・手法の詳細
提案プロセスは大きく4つの工程からなる
1. Defining the DQD potential
	- ゲート電圧を決定
	- ガウシアンプロセス＋NN
	- 詳細
		- Current measurements are taken along random directions in gate voltage space.
		- Gaussian processes model the hypersurface between conducting and non-conducting regions.
		- Neural networks confirm the presence of DQD features in stability diagrams.
1. Tuning the tunnel barriers
	- 量子ビット操作に必要なsinglet-triplet energy splittingをenhance
		- 一重項-三重項エネルギー分裂
		- bias triangle をsegmentation、その寸法や形状の情報を用いて、 一重項-三重項エネルギー分裂を強化するようトンネル障壁を最適化
	- ベイズ最適化
		- bias triangleの特徴を最適化するゲート電圧を決定する
		- aims to increase the singlet-triplet energy splitting 
		  by reducing current between the triangle baseline and excited states compared to the rest of the bias triangles​
1. Finding Pauli Spin Blockade
	- PSBを示す電荷遷移を発見
	- PSBは初期化と測定に使用
	- 低解像度なCSDと高解像度なCSDを併用
		- 低解像度CSDでPSBがありそうな場所(つまりbias triangle?)に検討をつける
		- その領域で高解像度なCSDを取得、再度SegmentationしてPSBの存在を確認したり、特徴抽出したり
	- NN
2. Finding the readout point
	- 量子ビットの測定や操作に最適なplunger 電圧の決定
		-  driving frequencies and pulse durations(パルス長)に最適化
	- 詳細
		- 磁場をスキャン、エントロピースコアを評価、測定領域を最適化
		- ベイズ最適化は可能性のある電圧を提案、それを評価していく
###### 有効性の検証方法
- 13回の実験(experimental run)にて検証
- 10回ラビ振動観測に成功
	- tuningにかかった時間はトータルで22-80時間
- tuning時間のボトルネック
	- 「the integration times required for DC transport measurements」
	- より高速な測定方法があれば、全体の高速化につながることを示唆
###### 議論
###### 重要視している先行研究
- Moon et al. demonstrated the potential of machine learning for quantum device tuning .
- Severin et al. explored cross-architecture tuning of quantum devices using machine learning .
- Schuff et al. identified PSB using deep learning techniques .
- 超曲面の応用：3次元のバリアゲート電圧空間における、導電/非導電領域の分割に超曲面モデルを使用
	- B. Severin, D. T. Lennon, L. C. Camenzind, F. Vigneau, F. Fedele, D. Jirovec, A. Ballabio, D. Chrastina, G. Isella, M. de Kruijf, et al., “Cross-architecture Tuning of Silicon and SiGe-based Quantum Devices Using Machine Learning,” arXiv preprint arXiv:2107.12975, 2021.
	- H. Moon, D. Lennon, J. Kirkpatrick, N. van Esbroeck, L. Camenzind, L. Yu, F. Vigneau, D. Zumbühl, G. A. D. Briggs, M. Osborne, et al., “Machine learning enables completely automatic tuning of a quantum device faster than human experts,” Nat. Commun., vol. 11, no. 1, pp. 1–10, 2020.
##### 本文読んでみた
- ラビ振動まで完全自動化
	- ラビ振動は量子ビットができていることを確認する工程
	- 人力では週月単位の時間
	- これを3日に短縮
- 検証の一環として、ラビ周波数とgファクタがどのようにゲート電圧に依存するかを解明
- 自動化に関する様々な先行研究
	- defining double quantum dot (DQD) confinement potentials [9, 23– 25], 
	- navigating to specific charge transitions [ 26– 33], 
	- fine-tuning of charge transport features [ 34] 
	- the inter-dot tunnel couplings [35, 36], as well as device characterization [ 37–39]
- 先行研究は機械学習の有用性を示す一方、実用性は不十分、と指摘
- 手法の大まかな流れ　一連の流れや例がFIG. 1
	- 全ゲート電圧=0から開始
	- 全4 stage, 前半2 stageで二重量子ドットのpotentialを定義 by tuning the inter-dot barrier and the reservoir coupling, stage 3で初期化と測定に必要な、PSBを探索, stage 4でゲート電圧を微調整すると同時に量子ビット操作に必要なパルス波の周波数と波長を決定 
	- 各stageで複数の候補(電圧領域)が生成、最も尤もらしいものを次のstageへ、だめなら戻って次の候補を渡す。見つかったらそれ以降は無視(破線)。これら一連の工程がFIG. 1 bにsearch treeとして示されている
- 手法の各stageの詳細 FIG. 2 
	- stage 1
		- その後使用するゲート電圧の範囲を絞る
		- バリアゲート電圧空間は3次元空間、この空間内の点をランダムに選び、電流測定、超曲面をモデル化、二重量子ドットが定義できそうな電圧領域が2-a-2の箱(DQD search region)のように得られる
		- 得られたバリアゲート電圧の領域内で、plunger gate voltageを変化させつつ電流測定、CSDを得る
	- stage 2
		- ゴール：一重項三重項エネルギー分裂を強化するようトンネル障壁を調整すること
		- stage 1で得た範囲内で、ゲート電圧をベイズ最適化
		- そのゲート電圧でCSD測定 2-b-1
		- segmentationでbias triangleの輪郭取得、scoreを計算2-b-2
		- 電荷移動で乱れたbias triangleをNNで最適化対象の領域から除去
		- 課題：大量のCSDの測定に時間がかかりすぎる。→臨機応変かつ効率的な測定アルゴリズムを考案、補足資料に
		- 定量的に有望と評価されたbias triangleを含むような、plunger gate voltageの範囲が絞られる
	- stage 3
		- PSBを見つけたい
		- 3つの分類器で判定
	- stage 4
		- plunger gate voltage＋パルス波を決定
		- 測定に使えそうな領域が2-d-1の白破線
		- 2つのplunger gate voltageと、パルス波の周波数と波長の4次元空間の最適化
#### [Colloquium: Advances in automation of quantum dot devices control](https://arxiv.org/pdf/2112.09362)
メモ：
- tuning
	- bootstrapping
	- coarse tuning
	- establishing controllability
	- charge state tuning
	- fine tuning
- それぞれのphaseに関する先行研究をSec. 3で
- 各phaseの外観　FIG. 3
	- bootstrapping FIG. 2,3
		- initializationとも
		- デバイスを冷却、最適化対象のパラメータをおおよその範囲に調整するpretuning
		- 具体的な研究
			- McJunkin, T W (2021), Heterostructure Modifications, Fabrication Improvements, and Measurements Automation of Si/SiGe Quantum Dots for Quantum Computation, Ph.D. thesis (The University of Wisconsin-Madison, Madison, WI, USA).
				- 未確認
			- [Autonomous tuning and charge state detection of gate defined quantum dots](https://arxiv.org/pdf/1911.10709)
				- 量子ドットの特性評価と理解
				- まとめ済み
			- [Computer-automated tuning of semiconductor double quantum dots into the single-electron regime (2016)](https://pubs.aip.org/aip/apl/article/108/21/213104/30965/Computer-automated-tuning-of-semiconductor-double)
				- まとめ済み
	- coarse tuning FIG. 4 ~ 7
		- 電子をとじこめるのに必要なゲート電圧の大体の範囲を見つける
		- この時点では閉じ込めた電子数不明
		- デバイストポロジーの決定とも解釈できる
		- device topologyの決定に関する具体的な研究
			- 単一量子ドットのトポロジー
				- [Algorithm for automated tuning of a quantum dot into the single-electron regime](https://journals.aps.org/prb/abstract/10.1103/PhysRevB.102.085301)
					- まとめ済み
				- [Miniaturizing neural networks for charge state autotuning in quantum dots](https://arxiv.org/abs/2101.03181v3)
					- まとめ済み
			- 二重量子ドットのトポロジー
				- [Machine Learning techniques for state recognition and auto-tuning in quantum dots(2018)](https://arxiv.org/abs/1712.04914)
					- まとめ済み
				- [Autotuning of Double-Dot Devices _In Situ_ with Machine Learning](https://journals.aps.org/prapplied/abstract/10.1103/PhysRevApplied.13.034075)
					- 未確認
				- [Toward robust autotuning of noisy quantum dot devices]()
					- 未確認
				- [Autonomous tuning and charge state detection of gate defined quantum dots](https://arxiv.org/pdf/1911.10709)
					- まとめ済み
			- Setting topology with rays 
				-  ようわからんので飛ばす
		- 
	- establishing controllability FIG. 8, 9
		- クロストークにより、一つのゲート電圧の変更が別のパラメータに影響
		- これを軽減する一つの方法がvirtual gateの導入
		- これは複数のゲートの線形結合で、ただ一つの電気化学ポテンシャルやトンネル障壁を変更できるよう定義される。
		- これをこのphaseで取得する
		- 具体的な研究
			- 飛ばす
	- charge state tuning　FIG. 10
		- ゴールは特定の数の電子(大抵1~3)を閉じ込めるのに必要な条件を特定する
		- 具体的な取り組み
			- 単一量子ドット (sec. 3-bと同じ)
				- [Algorithm for automated tuning of a quantum dot into the single-electron regime](https://journals.aps.org/prb/abstract/10.1103/PhysRevB.102.085301) 
				- [Miniaturizing neural networks for charge state autotuning in quantum dots](https://arxiv.org/abs/2101.03181v3)
			- 二重量子ドット
				- どこから探索を始めるかの違いかな
				- [Computer-automated tuning of semiconductor double quantum dots into the single-electron regime (2016)](https://pubs.aip.org/aip/apl/article/108/21/213104/30965/Computer-automated-tuning-of-semiconductor-double)
				- [Automated tuning of double quantum dots into specific charge states using neural networks](https://arxiv.org/abs/1912.02777)
					- まとめ済み
				- Ziegler, J, F. Luthi, M. Ramsey, F. Borjans, G. Zheng, and J. P. Zwolak (2022a), “Tuning arrays with rays: Physics-informed tuning of quantum dot charge states,” arXiv:2209.03837 10.48550/arxiv.2209.03837.
				- Hensgens, T (2018), Emulating Fermi-Hubbard physics with quantum dots: from few to more and how to, Ph.D. thesis 
				- [Quantum simulation of a Fermi–Hubbard model using a semiconductor quantum dot array](https://www.nature.com/articles/nature23022)
	- fine tuning
		- この時点で、システムのトポロジーと電子数は達成されている
		- 完全に制御された量子ビットシステムの実現には、極めて高精度な微調整が必要
		- 

---
```
#### []()
###### どんなもの？
###### 背景/モチベ
###### 先行研究からの改善
###### 技術・手法の詳細
###### 有効性の検証方法
###### 議論
###### 重要視している先行研究
```

Prompt
```
この論文を 
- どんなもの？ 
- 研究の背景やモチベーション
- 先行研究からの改善 
- 技術・手法の詳細
- 有効性の検証方法 
- 議論 
- 出発点としている、あるいは複数回引用するなど、重要視している先行研究 
のそれぞれの点について説明してくれ。 
なお、英語を使用すること。 
また、先行研究に言及する際は添付した論文中での引用番号を記載すること。
```