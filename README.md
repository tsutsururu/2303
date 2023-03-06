# 2303

# 223E  Σ[k=0..10^100]floor(X／10^k)

解答遷移 TLE WA TLE AC

計　52：48 + 15:00

➀ 思考

まず、10^(10^100) >> X より、X + X[:-1] + .... +X[0] が答え。素直にこれを求めても O(桁数) なので十分高速である。 今回は、Σ int(X[i] × (桁数-i)) でこれをオシャレに求めてみた。⇒ TLE / 四則演算は O(桁数) なので、これでは全体で O(桁数^2) となって全く間に合わない。⇒　i桁目は 桁の総和 - (i-1)桁目の数 の最終桁で求められる。つまり、繰り上がりを高速に求めることができれば、O(桁数) で間に合う。⇒ i 桁目が高々 6 桁であることに注目すると、各桁を管理するテーブルを作成し、該当位置を操作する処理で全桁を求められそうである。ただし最終的に答えをテーブルから復元する際、最初の理由で足し合わせることはできないので 文字列の結合で対処する。

➁ 解法

未決定値を保存しておき、各桁をこれに足し合わせてその最終桁をその桁の数とする。そして、2桁目以前を次に持ち越す。

③ 文字列の結合

X += T vs ""join() は計算量が異なる。前者は演算ごとに O(桁数) かかるので、全体で O( 桁数^2 ) かかるのに対して、後者は O(桁数) で済む。

参考：[Python Speed](https://sites.google.com/a/peignot.net/www/python-speed)

# 211  chokudai 2回目  diff 559

解答遷移 AC

計 10:11

➀ 思考

i 文字目を見て、例えば "o" だったら、i-1 文字目までで作成できる "ch" の個数だけ "cho" が作成できるので、dp[ 生成文字列の最後の文字 ] : 生成文字列の総数 で状態を定義すればよいと判断。一点更新なので、i 番目 の情報は与える必要はない。


# 0301

# 重複を許した DP

例えば、要素 S を重複を許して選んだ総和を M にする選び方の総数を求めることを考える。この時、dp[i][W] : i番目の要素までで総和 W を成す選び方の総数 とすると、普通であれば dp[i][W] += dp[i-1][W-S[i]]　で遷移を考えるだろう。しかし、これでは S[i] を一度選ぶような遷移しか考えることができない。そこで、dp[i][W] += dp[i][W-S[i]] とする。これによって、S[i] を既に選んだ場合も含めた状態からの遷移を考えることができるようになった。

![image](https://user-images.githubusercontent.com/109026838/222096719-75ca8d65-840d-4644-b507-86e3173eea61.png)


また、このように更新後の状態からの遷移のみを考えたいので、in-place 化することが可能。

# ☆  153E Crested Ibis vs Monster  diff 1015

降参

➀ 思考 

A/B 効率の貪欲法で解けそうだと思ったが、 (2,2),(6,4),(23,15) H=24 の場合無理。  ⇒　dp では O( Σ k/B × N = N×H^2 ) で無理だし、代案もなく降参 

➁ 思考

重複を許した dp で解く。

参考 : https://drken1215.hatenablog.com/entry/2020/01/26/225000

③ 感想

重複を許した dp を 手札に加えることができた。ナップサックを数か月ぶりに復習できた。次からは思いつける

# 178 Redistribution  2回目  diff 875

解答遷移 AC

➀ 思考

・dp

dp[i] = Σ dp[i-3] で遷移可能。更新後の情報が欲しいので in-place化

・メモ化

f(i) = Σ f(i-3) , f(0)=1 , f(1)=f(2)=0 で求められる。


# ☆ 099 Strange Bank  diff  1101

降参

➀ 解法

dp[i][M] : i番目まで使って、M円引き出すための最小回数　で状態を定義すると、**i番目のお金は重複を許して引き出せるので**、dp[i][M] = dp[i][M-m] +1 で遷移可能。したがって、N 以下の 6^i,9^i を順番で紐づけて、dp すればよい。in-place 化可能。

参考 : https://qiita.com/drken/items/ace3142967c4f01d42e9


# ☆ アルゴ式 特集 問題 7 

➀ 解法

各 A の重複数に制限のある 重複を許したdp である。したがって、dp[i][M] : 総数を M とするために選ぶ Ai の数 で状態を定義し、dp[i][M] =min(dp[i-1][M] , dp[i][M-A[1]] +1) と遷移する。更新後状態からの遷移を考えるので、in-place化可能。ただし個数制限は各要素で独立なので、Ai を変更するにあたって、既に選べる状態を 0 で初期化する必要がある。



# ☆ 197E  Traveler  diff 1379

初見：分け赤ランクなって降参　⇒ その後しばらく休憩とって時間かけて AC 

➀ 思考

同じ色のボールをすべてとり終わった位置を考える。これは右端か左端に限られる。なぜなら、途中でとり終わるとり方は必ず余計な道のりであり最適でないから。⇒ したがって、昇順に各端から各端へ進むことのみを考えた場合の最短距離を求めればよい。例えば、abs(新右端 - 旧右端) + abs(新左端 - 新右端)で 旧右端 → 新左端へ行く場合の距離を求められる。

➀' 初見

右端、左端で回収し終えることは理解できていたものの、旧右端 → 旧左端 → 新左端 → 新右端　のような遷移しかしないと考えたり、なぜか原点に戻ることを考えたりと、思い返すと意味不明な思考をしていた。


# 0302

# ☆ 151E Max-Min Sums  diff 1344

解答遷移 AC

計 1:36:13

➀ 思考

dp するには、必要な情報が多く状態が爆発するので無理そう( 1,何個 , 2, i  , 3, min , 4, max : 個数)  ⇒ 情報をまとめる方法を考えると、例えば K=3 で、(A2,A0) (A3,A1) .... を最大、最小とするペアはそれぞれ 2CK-2 個存在し、これらの和は(AA[N-1] - AA[3-2+i] - AA[N-3-i]) との積で表現できるので　O(1) で求められる。⇒ したがって、昇順累積和、NCr をあらかじめ求めておき、i : 0 → N-K までこれを計上すればよいと判断した。

![image](https://user-images.githubusercontent.com/109026838/222395337-455f293e-740f-4758-8cd9-d34967aa7f94.png)


➀'

K=3 では (A2,A0) , (A3,A1) , (A4,A2) , (A5,A3) .... と続き、途中の項が次々に相殺さるれるので最終的に 1,(AN-1 + AN-2 + ... ) - 2,(... + A1 + A0)) との積を求めればよいと考えた。しかしペアの数は単調に減少するため次第に削除できる項数も減少していき、途中で削除できないようになる。よってこれを式化するのは難しく、素直に全範囲計上する方が遥かに簡単である。

また、累積和計上に気づいた後もindex 番号で戸惑ったが、始点終点がどこで、どのようにこれらがスライドするか考えると簡単だった。

➁ 解法

ans = (最大値 - 最小値) + (最大値 - 最小値) + ... = Σ最大値 - Σ最小値　と分割して考える。

③ 

削除の考えをどこかでやっていた記憶があったが、これは記憶違いだった。

186D , 194C が類題だが、全く削除など行っていなかった。

# ☆ 151E Summer Vacation  diff 1313

解答遷移 AC

計 1:14:44

➀ 思考

報酬がより高価な仕事から行いたい。(A,B) の仕事をするとして M-A 日目以前のどこでこの仕事をするのか考えなくてはいけない。⇒ SegmentTree で (0,A+1)　区間を調べる方法では、A日目までの仕事の埋まり状況がわからない。⇒ そこで union-find で**その日以前に仕事可能な最終日を管理して**、uf.find(A) 日目にこの仕事をすればよいと考えた。

A 日目まで全埋まり(仕事ができない状態)は仕事可能最終日が 0日目であることで定義した。これに伴い、日にちは 1-indexed としている。

![image](https://user-images.githubusercontent.com/109026838/222422969-4c2ac100-b173-4788-acfd-f180e56a1b3e.png)


➀' アクセスポイントを親とする union-find 類題

228D Linear Probing


➁ 別解

A 日目 に行うべき仕事は A ～ M 日目までに入れられてまだ確定していない仕事の内、報酬が最大のものである。したがって、A を M まで全探索しその日に入れられる仕事を ヒープに格納し、その日はそのうち最大の仕事を行う。という処理を繰り返せばよい。

# 0303

# 135D Digits Parade  diff 1334

解答遷移 AC

計 10:50

➀ 思考

dp[i][j] : i桁目までで生成でき余りが j　のもので状態を管理して、? ならば +0 ～ +9 の状態へ遷移させればよい。計算量は O(N × 13 × 10 ) で十分高速

# 229E Graph Destruction diff 1015

解答遷移 AC

計 11:32

➀ 思考

破壊はしたくないので、逆順に構築していく方針を考える。⇒ 頂点 i と連結する i 以降の頂点と同じ連結成分でなければ union して現在の非連結成分数を -1 する処理を行えばよいと判断


# ☆ 200D Happy Birthday! 2  2回目

降参 ( dp は思いついたが、これは過去に実装した記憶があたので今回は再解答する経緯となる別解を探すことに専念。見つからず。。)

➀ 解法

鳩ノ巣理論。全く思いつかなかった。

➁ 感想

dp も死ぬほどややこしくて、次正確に実装できるか不安。また再解答必須


# 257C  Robot Takahashi　n回目

解答遷移  WA WA  WA AC

計 29:32 + 15:00

➀ 思考

W 昇順にソート、前から順に見ていき、1なら -1 , 0 なら +1 していくことで、判定数を求めることができると考えた。⇒ WA / 同じ数は一つずつ計上できないので、01 の並びをそのまま計上してしまうと不当に高く評価してしまうことに気づいて、次の値が異なれば計上するように修正して AC

➀' 重複工夫

10 の並びなら不当に高く評価することがないので、この並びでソートすれば重複を考慮せず計上可能

➁ 解法

X = W となったときの変化量を管理しておけばよい。これにより、変化が生じるタイミングのみでの更新が可能。

# 256E Takahashi's Anguish  diff 1224

解答遷移 AC

計 50:46

➀ 思考

i　← X と辺を張ったグラフを構築して、頂点v を取り除くことで生じる不満度の低い順に探索する順番を求めれば良さそう。⇒ 出次不満度を管理し、トポロジカルソート + ダイクストラ の要領で探索することでこれを求めた。

➀'

解法でも述べるが、このグラフは全頂点が必ず別の頂点に連結しているので、出次数 0 の状況にはならない。この場合を想定した余計な処理は必要なかったのだ。 

➁ 解法

Functional Graph である。このグラフは出次数が必ず 1 なので、閉路を検出した際始点から素直に探索を行うと、必ず 1本道で閉路をたどり始点に戻ることができる。この性質を利用すれば、union-find で閉路を検出し、始点から閉路を成す頂点を全探索して最小の不満度を採用するような処理を考えることができる。


# 241D Sequence Query  diff 1177

解答遷移 AC

計 46:59

➀ 思考

優先度付きキューで k 番目まで取り出せたらその位置の値を出力すれば良さそうト思ったが、この問題では x を起点に取り出す必要があるので、端から取り出すことしかできない優先度付きキューでは計算量が爆発する。⇒ もう常にソートしている配列を作ること以外で打開する方法がなさそうなので SortedSet に頼ることにした。Counter で個数を管理し、k個になるまで、x 以下で最大/以上で最小 の要素をそれぞれ S.lt(x+1)/S.gt(x-1) で調ベればよいと判断。計算量も O(Q√Q) でギリギリ十分高速

# ☆ 187E Through Path  diff 1358

降参

➀ 思考

加算方向の有向グラフを構築してトポロジカル探索で計上していく処理を考えたが、これには辺の向きが一方通行のグラフ (ex. 1→2→...→ or 1←2← ... ) でなくてはならない。サンプル1 では入力がたまたま偶然このグラフを構築できる順番になっているが、サンプル2 のような場合では 一方通行のグラフを作れず、探索が破綻する。 ⇒ 他の案も思いつかず降参。

➁ 解法

部分列の根に変化量を記憶する。

![image](https://user-images.githubusercontent.com/109026838/222802391-bd080f24-9d1a-4d39-86e5-28d3a702f21e.png)

パターン2 の場合がやっかいだが、これは全体(根)を + して、対象部分木の根　を - することで表現できる。

③ 学び

まず、**木の問題は根を定めること** が大切なことがわかった。根が定まっているか否かで見通しが全く違うことが理解できた。これまで問題側で根を決めてくれていたから解けていた問題も多かったのだろう。

また、このような親の情報を子へ受け渡していくアルゴリズムは　木 dp というらしい

# 0304

# 070D Transit Tree Path   diff 1182

解答遷移 WA AC

計 12:09

➀ 思考

木なので根を設定する　⇒　K を根にすれば dist[x] + dist[y] で求められると考えた。


# 133E  Virus Tree 2  diff 1505

解答遷移 RE × 4  WA AC

➀ 思考

親とその親で使わなかった K-2 色を 子に与えていく。つまり、(K-2)×(K-2C子の数) 個塗り方が倍になることがわかる。このように、次数最大の頂点を親として全探索し、塗り方を求めていけばよいと判断した。ただし、根は親を持たないので k-1 とする必要がある。

➀`

RE は N=1 のとき、根を定義できていないことで生じていた。(WA はこれを修正した後 K でなく 0 を出力してしまったことが原因)

また、実は次数最大の頂点ではなく任意の頂点を根にできる。

# 134E Sequence Decomposing   diff 1320

解答遷移 AC

計 41:24

➀ 思考

転倒数 or ISL みたいだから Segtree で解けそうだな　⇒ i は i 未満で最大の j と同じ色で塗るのが最適であり、これは Segtree に index 番号を格納した [0,i) 区間の最大値を取得することで O(logN) で求められる。⇒ したがって、座圧して Aを木に乗せられるようにして、対象区間で index 番号を取得できなければ +1 , 取得できればその位置を未登場に更新する ( その位置の色と同色で塗るための条件は i< に変更したから ) 。最後に位置 i が登場したことを記憶すればよいと考えた。

ただし、A には同じ値が重複するので同じ値を同じ色で塗らないように、後ろに登場するほど若い index 番号を与えるような座圧にする調整を施した。

➀' 

A の index 番号を 1-indexed にしないと、[0,x) 区間取得で [0,0) という存在しない区間にアクセスしてしまうのが良くなさそうだと考えたが、prod(0,0) は e を返すので全く問題なかった。なぜなら prod(0,0) では 0　の位置の初期値 -1 を求めるための操作であるからである。

➁ 別解

A を前から順にテーブルに格納すると、Ai 未満で最大の位置は二分探索で求められる。この位置を書き換えればよい。もし、そのような位置がなければ先頭に Ai を追加することになるので、テーブルは deque で管理する。

この操作中、常にテーブルの要素は昇順にならんでいるので二分探索が可能になっている

➁' bisect

対象イテラブルが空であっても、0 を返してくれることを覚えておく。


# 0305 

# ☆ 292E  Transitivity  diff 1272

降参(本番)

![image](https://user-images.githubusercontent.com/109026838/222923012-c834de15-aa2a-4d3b-8e76-9cfe8d84fa7c.png)

➀ 解法

最終状態を調べると、最初の状態から到達可能な全頂点と辺を構築することがわかる。したがって、これを計上して M を引いた値が答え

➁ 感想

本番では、トポロジカル的に終点から辺を張る頂点を記憶する処理を考えたが、トポロジカルできない場合で詰まった(サンプル2)。

アルゴリズムを考えることも大切だが、一度どうするかは置いといて最終的にどうなるか(どうなるのが理想か) 考えてみることの大切さを
学んだ。



# 0306 

# 242E (∀x∀) diff 1365

解答遷移 AC

計 53:01

➀ 思考

桁 dp の要領で確定回文と、未確定回文を分けて計上することを考えたが、O(N × 26^2) で怪しい ⇒ わざわざテーブルを作成しなくても i 文字目で回文が確定した場合、26^(N-i) 個の回文ができることに気がつく。したがって、(S[i]-A) 個できる回文の先頭を ans にインクリメントし、i+1 でこれをまとめて 26倍することで計上できると判断。最後に Sの前半部分を折り返して生成できる回文が条件を満たすかを判定すればよいと考えた。


# 119E Synthetic Kadomatsu  diff 1346

解答遷移 AC

計 18:29

➀ 思考

制約を見た瞬間 bit 全探索の問題っぽいなと思った。問題を見てみると確かに 8つの竹それぞれについて、A,B,C　で使うか、使わないか 全探索すれば良さそう。O(4^8) は十分高速だし。⇒ AC



















