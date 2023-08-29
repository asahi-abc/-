# CSVファイルのヘッダー処理
全角（）が含まれているとエラーが出るため以下の処理により正規化を行う。

train_x= select(df, Not(:取引価格_総額_log))

train_y = df[:, :取引価格_総額_log]
