# CSVファイルのヘッダー処理
全角（）が含まれているとエラーが出るため以下の処理により正規化を行う。

train_x= select(df, Not(:取引価格_総額_log))

train_y = df[:, :取引価格_総額_log]

# 複数CSVファイルをまとめる処理
欠損値があるとappendできないため、

1.データのクリーニング,2.Missing型から適切な型への変換,3プロモートのどれかを行う。

insert!(df, :地区名, new_value, promote=true)

とすることで、型を一致させてくれる。
