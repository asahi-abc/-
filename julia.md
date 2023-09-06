# CSVファイルのヘッダー処理
全角（）が含まれているとエラーが出るため以下の処理により正規化を行う。

data = CSV.File("train/01.csv", normalizenames=true) |> DataFrame

# 目的関数の分割処理
train_x= select(df, Not(:取引価格_総額_log))

train_y = df[:, :取引価格_総額_log]

# 複数CSVファイルをまとめる処理
欠損値があるとappendできないため、

1.データのクリーニング,2.Missing型から適切な型への変換,3プロモートのどれかを行う。

insert!(df, :地区名, new_value, promote=true)

とすることで、型を一致させてくれる。

# カテゴリカル変数の扱い
カテゴリカルな値はモデルが扱えないので、整数値に変換が必要。

1.ラベルエンコーディング：決定木の場合のみ使用可能

2.One-Hotエンコーディング:水準値が多くなるとデータ数が大きくなる。次元の呪いが発生しやすい

CategoricalArraysを用いて、整数値に変換できる。
