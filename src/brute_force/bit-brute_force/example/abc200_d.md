# ABC200 D - Happy Birthday! 2

[https://atcoder.jp/contests/abc200/tasks/abc200_d](https://atcoder.jp/contests/abc200/tasks/abc200_d)

実行時間制限: 2 sec / メモリ制限: 1024 MB

## 問題文

\\(N \\)個の正整数からなる数列\\(A=(A_1,A_2,...,A_N) \\)が与えられる．以下の条件を全て満たす2つの数列\\(B=(B_1,B_2,...,B_x) \\)，\\(C=(C_1,C_2,...,C_y) \\)が存在するか判定し，存在する場合はひとつ出力せよ．

- \\( 1 \leq x,y \leq N \\)
- \\( 1 \leq B_1 < B_2 < ... < B_x \leq N \\)
- \\( 1 \leq C_1 < C_2 < ... < C_y \leq N \\)
- \\(B \\)と\\(C \\)は，異なる数列である．
    - \\(x \neq y \\)のとき，または，ある整数\\(1 \leq i \leq \mathrm{min}(x,y) \\)が存在して\\(B_i \neq C_i \\)であるとき，\\(B \\)と\\(C \\)は異なるものとする．
- \\(A_{B_1}+A_{B_2}+...+A_{B_x} \\)を\\(200 \\)で割った余りと\\(A_{C_1}+A_{C_2}+...+A_{C_y} \\)を\\(200 \\)で割った余りが等しい．

## 制約

- 入力は全て整数
- \\(2 \leq N \leq 200 \\)
- \\(1 \leq A_i \leq {10}^9 \\)

## 解法

問題文をよりわかりやすく言い換える．

数列\\(A \\)の任意長の部分列の集合の中に，総和を\\(200 \\)で割った余りが等しくなるものが存在するか，存在する場合はその2つの部分列を出力せよ．

各\\(A_i (1 \leq i \leq N)\\)を含めるか含めないかの組合せを全探索すれば答えを得られるので，単純なbit全探索の問題に見える．

bit全探索の計算量を確かめてみよう．\\(2 \leq N \leq 200 \\)なので，組合せの総数は最大で\\(2^{200}>{10}^{50} \\)通りである．したがって，計算量は\\(O(2^N N) \\)となり，この解法の実行時間は制限を大きく上回ることがわかる．そのため，より深い考察が必要である．

今求めたいのは，それぞれの組合せにおける，総和を\\(200 \\)で割った余りに，重複があるかないかである．\\(200 \\)で割った余りは\\(0 \\)~\\(199 \\)の\\(200 \\)通りであるため，組合せを\\(201 \\)通り調べた時点で，必ず1つ以上の重複が生じることがわかる．

つまり，組合せの総数は最大\\(2^{200}-1>{10}^{50} \\)通りだが，最初の\\(201 \\)通りで必ず答えが見つかる．そのため，実は，上記アルゴリズムの計算量は見つかった時点でプログラムを終了すれば\\(O(201) \\)であり，十分高速なのである．

他の解法として，上の考察から，そもそも数列\\(A \\)の長さは\\(8 \\)あれば組合せ数が\\(2^8-1=255 \\)となり\\(201 \\)を超えているため，そもそも最初から数列\\(A \\)の長さが\\(\mathrm{min}(N,8) \\)であるという前提でプログラムを書いても正解となる．この場合は，全探索のループを最後まで行っても計算量は\\(O(255) \\)であり，十分高速である．

## 実装

```py
n = int(input())
a = [*map(int, input().split())]

mod = [[] for _ in range(200)]
for bits in range(1, 1<<n):
  combi = [i+1 for i in range(n) if (bits>>i)&1]
  m = sum([a[i-1] for i in combi])%200
  mod[m].append(combi)
  if len(mod[m]) > 1:
    print('Yes')
    [print(len(e), *e) for e in mod[m]]
    exit()

print('No')
```