# YuukouhaiFuyouhaiCalculator
麻雀においてシャンテン数および有効牌・不要牌を計算する

## 使用方法
1. ヘッダファイル"calsht_dw.hpp"をインクルードする。
2. クラス"CalshtDW"をインスタンス化する。
3. 手牌を表す長さ34のint型配列を用意する。

- n番目の要素がn番目の牌の枚数を格納する。(n=0:1m, ..., n=8:9m, n=9:1p, ..., n=17:9p, n=18:1s, .. , n=26:9s, n=27:東, ...,n=30:北, n=31:白, n=32:発, n=33:中)

4. Calshtクラスのメソッドを呼び、シャンテン数と有効牌・不要牌を計算する。

- 各メソッドはシャンテン数+1の値を返す。
- ある牌が有効牌(または不要牌)であることは、64bit整数(unsigned long long int型変数)のi番目のbitが1であることに対応する。

## メソッド一覧
- (面前部分が)n面子一雀頭形のシャンテン数を求める。
  - int calc_lh(int* [手牌を表す配列], int n, unsigned long long& [有効牌の集合を表す変数], unsigned long long& [不要の集合を表す変数])
- 七対子のシャンテン数を求める。
  - int calc_sp(int* [手牌を表す配列], unsigned long long& [有効牌の集合を表す変数], unsigned long long& [不要の集合を表す変数])
- 国士無双のシャンテン数を求める。
  - int calc_to(int* [手牌を表す配列], unsigned long long& [有効牌の集合を表す変数], unsigned long long& [不要の集合を表す変数])
- 一般形のシャンテン数(上記のシャンテン数の最小値)を求める。
  - int operator()(int* [手牌を表す配列], int n, int& [モードを表す変数], unsigned long long& [有効牌の集合を表す変数], unsigned long long& [不要の集合を表す変数])
  - モードとはどの上がりパターンでシャンテン数をとるかを表す値です。n面子一雀頭形には1、七対子には2、国士無双には4の各値が割り当てられています。複数の上がりパターンでシャンテン数をとる場合は、それらの和がモードとなります。

## サンプルプログラムについて
このプログラムでは一人麻雀シミュレーションを行います。各巡目で、シャンテン数を不変に保ちかつ有効牌の枚数が最大となるように打牌を行います。
シャンテン数を計算すると同時に有効牌(ツモったときにシャンテン数が低下する牌)と不要牌(捨てたときにシャンテン数を不変に保つ牌)を計算するため高速に計算することが可能です。

## サンプルプログラムの使用方法
1. インデックスファイルを解凍
```
$ gunzip index_dw_h.txt.gz index_dw_s.txt.gz
```

2. ビルド
```
$ make
```

3. 実行
```
$ ./prob.out [局数]
```

4. 出力ファイルを確認

以下の出力は局数を100万とした場合の結果です。乱数のシードは実行ごとに変わるので出力結果は毎回変化します。
```
$ cat result.txt
Number of Tiles         14
Total                   1000000
Time (msec.)            106499

Turn    Shanten Number (-1 - 6)                                         Hora    Tempai  Exp.
0       4       689     23224   194623  439207  285376  55254   1623    4e-06   0.000693        3.1576
1       36      3139    60055   312301  436206  169174  18856   233     3.6e-05 0.003175        2.76561
2       165     9550    118653  409619  368079  87961   5939    34      0.000165        0.009715        2.42371
3       575     22236   193284  461052  278799  42243   1805    6       0.000575        0.022811        2.12924
4       1585    43455   270484  467989  196448  19497   539     3       0.001585        0.04504 1.87492
5       3557    73558   341680  439988  132134  8916    167     0       0.003557        0.077115        1.651
6       7014    111731  397628  393033  86425   4115    54      0       0.007014        0.118745        1.45269
7       12342   155439  437312  337357  55610   1918    22      0       0.012342        0.167781        1.2743
8       19769   202505  460160  281105  35463   991     7       0       0.019769        0.222274        1.11299
9       29749   250970  466746  229544  22484   502     5       0       0.029749        0.280719        0.96557
10      42291   298561  460318  184296  14284   247     3       0       0.042291        0.340852        0.830474
11      57229   343412  444338  145635  9257    128     1       0       0.057229        0.400641        0.706667
12      74977   383949  420945  114180  5875    74      0       0       0.074977        0.458926        0.592249
13      94703   420040  392750  88629   3832    46      0       0       0.094703        0.514743        0.486985
14      116313  450680  362048  68448   2487    24      0       0       0.116313        0.566993        0.390188
15      140202  475546  330180  52435   1622    15      0       0       0.140202        0.615748        0.299774
16      165530  494708  298847  39820   1087    8       0       0       0.16553 0.660238        0.21625
17      192032  508775  268240  30169   780     4       0       0       0.192032        0.700807        0.138902
```

1行目は手牌の枚数、2行目は局数、3行目は実行時間(ミリ秒)を示しています。
6行目以降は、左から順に巡目、各シャンテン数の割合(-1から6)、和了率、聴牌率、シャンテン数期待値を示しています。
特に最終行に注目すると一人麻雀の和了率が約19%であることがわかります。各巡目に対するシャンテン数の割合と期待値を図示すると以下のようになります。

![シャンテン数の割合](ratio.png "シャンテン数の割合")

![シャンテン数の期待値](expected_value.png "シャンテン数の期待値")

## 注意事項
- C++11以上に対応したコンパイラが必要です。
