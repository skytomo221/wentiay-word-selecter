# wentiay-word-selecter

wentiay-word-selecterは、W000の単語を生成します。

## Algorithm

1. 生成する単語の長さ（`c_len`）を求める。
   1. `c_len`はそれぞれの親言語の正規重み×親単語の長さの和で求める。
2. 候補となる音素の一覧を求める。
   1. `n`文字目の音素の候補を以下のように求める。
   2. すべての親単語に対して以下のことを行う。
      1. 任意の親単語の長さを`len`とする。
      2. `len = c_len`のとき、親単語の`n`文字目を候補となる音素の一覧（`cps`）に追加する。
      3. `len > c_len`のとき、親単語の`n`文字目から`n + len - c_len + 1`文字目までに含まれる音素を`cps`に追加する。
      4. `len < c_len`のとき、親単語の`max(0, n - (c_len - len))`文字目から`min(len, n + 1)`文字目までに含まれる音素を`cps`に追加する。
3. 候補となる単語の一覧を求める。
   1. 候補となる音素の一覧に含まれる音素の組み合わせから単語を作る。
   2. W000の音素配列の規則に適合している単語のみを抽出する。
      1. 同じ母音の連結は禁止（W202）
      2. 同じ子音の連結は禁止（W203）
      3. 有声子音と無声子音との連結は禁止（W204）
      4. `x, h, y, w`と子音の連結は禁止（W205）
      5. `c, j, s, z` は互いに連結できない（W206）
      6. 三重子音は`ts, tc, dz, dj`と子音の連結のみである。（W207）
      7. 最初の音素は`y`を除いた子音（W208）
      8. 最後の音素は子音（W209）
      9. 二重子音は`ts, tc, dz, dj`のみが語頭に立つ（W210）
      10. 三重母音は禁止（W211）
4. 候補となる単語のスコアを求める。
   1. すべての親単語に対して以下のことを行う。
      1. 候補となる単語`word`が任意の親単語`loanword`と一致した文字数を`i`とする。
      2. `i > 1`のとき、`i`と親言語の正規重みの積をスコアに加算にする。
      3. `i = 1`のとき、`i`と親言語の正規重みの積の1000分の1をスコアに加算にする。
5. 最もスコアの高い単語がW000の単語として選ばれる。
   1. スコアが同点の場合は、アルファベット順で小さい方が優先される。

## Word list

Click [here](./export/word-list.md).
