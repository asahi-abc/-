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

# ラベルエンコーディング

    function convert_categorical_to_integer(df::DataFrame, column_name::Symbol)
        # カテゴリカルな列に変換
        df[!, column_name] = categorical(df[!, column_name])
    
        # 各カテゴリに対応する整数のマッピングを作成
        category_to_int = levels(df[!, column_name])
    
        # 列を整数に変換
        df[!, column_name] .= [findfirst(x -> x == category, category_to_int) for category in df[!, column_name]]

        # 整数値から文字列に変換するための辞書を作成
        int_to_category = Dict(enumerate(category_to_int))

        return df, int_to_category
    end
# Any型の処理
Any型では学習できないので、型を統一する必要がある。例:String型とInt型が混在している場合の処理

    function change_int(x)
        if x == "2000㎡以上"
            x = 2000
        end
    
        if isa(x,String15)
            return parse(Int64,x)
        else
            return x
        end
    end
# XGBoostのパラメータ詳細
    dtrain = DMatrix(train_x, label=train_y)

    # Train the XGBoost model
    bst = xgboost(dtrain, num_round = 100, eta = 0.7, max_depth = 6, objective = "reg:squarederror", eval_metric = "rmse")
| パラメータ              | 解説                                                                                     |
|-----------------------|------------------------------------------------------------------------------------------|
| `eta` or `learning_rate` | 学習率（Learning Rate）。各ステップでモデルの重みをどれだけ更新するかを制御します。小さな値は過学習を防ぎますが、収束に時間がかかります。 |
| `max_depth`           | 木の深さの最大値。ツリーの深さを制限し、過学習を防ぎます。                                 |
| `min_child_weight`    | 葉ノードの最小の合計インスタンス重み。ツリーの分岐を制御し、過学習を防ぎます。             |
| `subsample`           | トレーニングデータのサブサンプルの割合。過学習を防ぐために使用されます。                   |
| `colsample_bytree`    | ツリーを構築する際に使用する特徴量の割合。過学習を防ぎ、モデルの多様性を増やします。     |
| `lambda` or `reg_lambda` | L2正則化の強さを制御します。モデルの複雑さを制限し、過学習を防ぎます。               |
| `alpha` or `reg_alpha`   | L1正則化の強さを制御します。特徴量選択を促進し、モデルのスパース性を増やします。   |
| `num_round`           | イテレーションの数。トレーニングプロセスの反復回数を指定します。                         |
| `objective`           | 最適化する目的関数。回帰、分類、ランキングなどのタスクに応じて選択します。               |
| `eval_metric`         | モデルの評価指標。トレーニング中にモデルのパフォーマンスを評価するために使用します。     |

