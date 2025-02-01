- ABC348
	- [D問題](https://atcoder.jp/contests/abc348/tasks/abc348_d)：
	  2次元格子点を途中エネルギーを補給しつつスタートからゴールまで到達できるか。
	  発想：
	  エネルギーのあるN個の地点間それぞれの間を、エネルギー補給せずに到達可能かをN頂点有向グラフで表し、BFS
- ABC 349
	- [D問題](https://atcoder.jp/contests/abc349/editorial/9772)：
	　![[スクリーンショット 2024-04-14 142243.png]]
	　セグメント木を左から貪欲に確保していく
- ABC 352
	- `set`平衡二分探索木で実装
		  `*st.rbegin() : 最大値の取得`
		  `*st.begin() : 最小値の取得`
- ABC 353
	- n個の整数列の任意の2組のうち一定値より大きいものの数
		- set(=>vector)等で大きさ順で保持
			- set => vectorで昇順のvector得られる
		- left / right 2つのpointerを用意
		- left + rightが一定値を下回るまでright--, 下回ったらleft++
- ABC 354
```python
# structのソート
struct Card {　
	int a;
    int c;
    int index;
};
~
cards[i] = {a, c, i};
~
// C の昇順にソート
sort(cards.begin(), cards.end(), [&](const auto &l, const auto &r) {
	return l.c < r.c;
});

```
	

