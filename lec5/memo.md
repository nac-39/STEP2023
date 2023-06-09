# ５, 6: TSP

## 実行方法

```bash
cd lec5
python3 solver_mine.py
# pypyで高速化する場合
docker run pypy:3.9-7.3.12-bullseye --rm -v ".:/workspace" bash
# cd workspace
# pypy solver_mine.py
```

## 方針

### その１：とりあえず貪欲にがんばる？

- 一つだけポツンと残らないようにしたいが、方法が思いつかなかった…

### その２：よく知られているアルゴリズムを使ってみる

- 2-opt
- 焼きなまし法
- [採用] 遺伝的アルゴリズム ← スコアボードでパッと見つかってる人がいなさそうだからやってみたい

#### 遺伝的アルゴリズム

都市を回る順番を個体（セールスマン）の遺伝子として、遺伝的アルゴリズムを適用する。

以下、[遺伝的アルゴリズムで巡回セールスマン問題を解いてみる(理論編)](https://qiita.com/masaru/items/729a0a0e2d7f305e8e90)という記事の抜粋。

- 個体：セールスマン。個体は道を回る順番を染色体として持つ。

  - 適応度：通る道の長さの合計を適応度の計測に使う
  - 染色体：道を回る順番

- 選択

  - エリート選択：適応度が高い順に選択
  - ルーレット選択：適応度から計算された確立に基づいて選択

- 経路の表現方法

  - 順列コーディング：点の番号をそのまま並べる方法
  - 序数コーディング（参考記事の独自用語）：通過した点を除外した点列の序数を使う

- 交雑の方法

  - 順列コーディング
    - 循環交叉：二つの染色体から、点の組と位置の組が等しいグループを探し、グループ同士を交換する, [参考: 遺伝的アルゴリズムの循環交叉について](https://qiita.com/python6051/items/b5f37c353bf71ca07cc4)

- 突然変異
  - 交換：ランダムに選択した２区間について、遺伝子を区間ごと交換する


### パラメータ関連

- `mutation_rage`は0.1ぐらいだと永遠に解が変わらなくなる…？

## 閲覧した記事

[AHC006 初心者向け解説 ～貪欲だけで順位表 2 ページ目を目指す～](https://www.terry-u16.net/entry/ahc006-for-beginners)という記事を参考に、貪欲法で行けるところまで頑張ってみる

[巡回セールスマン問題をいろんな近似解法で解く（その 2: 局所探索法と 2-opt 近傍|Qiita](https://qiita.com/take314/items/33843f980c4cbab140ac)

[巡回セールスマン問題(TSP)を遺伝的アルゴリズム(GA)を用いて解くプログラム](https://gist.github.com/TonyMooori/dd77a0f7d42ff26c2ea2)

[遺伝的アルゴリズムで巡回セールスマン問題を解いてみる(理論編)](https://qiita.com/masaru/items/729a0a0e2d7f305e8e90)

[順序問題への GA の適用](http://ono-t.d.dooo.jp/GA/GA-order.html)

## 参考にしたコード

`is_crossing()`: [https://github.com/ss-cosmos-hue/step23dev/blob/main/5/solver_simulated_annealing.py#L35](https://github.com/ss-cosmos-hue/step23dev/blob/main/5/solver_simulated_annealing.py#L35)

## 他の人のコード見て雑感

- 都市が少ない時は全探索
- 2-optか焼きなまし法を使ってる人が多い

## 高速化のアイデア

（高速化してイテレーションの回数をふやせば良いのでは！という理由から）

- 距離を求める関数について、キャッシュを残しておく
- 並列化する
- pypyとnumbaを併用する？
- CやRustで実装する（Rustは挫折）
