# Sionnaとは
Sionna ™ は、無線および光通信システムの物理層をシミュレートするためのTensorFlowベースのオープンソースライブラリである。

複雑な通信システムアーキテクチャの高速プロトタイピングであっても、 Kerasとして提供される目的の構成要素を接続するだけで行うことが可能となっている。
微分可能なレイヤーを使用すると、システム全体に勾配を逆伝播させることができる。これは、システムの最適化と機械学習、特にニューラルネットワークの統合を可能にする重要な要素となります。


# Sionnaの特徴
Sionna は、NVIDIA によって開発され、5G および 6G 研究を推進するために使用されている。低密度パリティチェック(LDPC)[^1]およびPolar en/decoders[^2]、3GPP channel models[^3]、OFDM(orthogonal frequency-division multiplexing:直交周波数分割多重化)[^4]、チャネル推定[^5]、およびソフトデマッピング[^6]が例として挙げられます。すべての構成要素は独立したモジュールであり、ニーズに応じて簡単にテスト、変更できる。

[^1]: https://en.wikipedia.org/wiki/Low-density_parity-check_code
[^2]: https://en.wikipedia.org/wiki/Polar_code_(coding_theory)
[^3]: https://www.3gpp.org/ftp/Specs/archive/36_series/36.101/
[^4]: https://en.wikipedia.org/wiki/Orthogonal_frequency-division_multiplexing
[^5]: https://en.wikipedia.org/wiki/Channel_state_information
[^6]: https://en.wikipedia.org/wiki/Soft_output_Viterbi_algorithm

# Sionnaの利点
通信分野のほとんどの研究者は、プロトタイプを迅速に作成し、最先端のアルゴリズムと比較してアルゴリズムのベンチマークを得るために、リンクレベルのシミュレーション用のツールを必要としている。しかし、独自のソフトウェアを除けば、広く使用されている共通のオープンソース ツールは存在していなかった。さらに、チャネル推定などの1つの分野の専門家は、現実的なチャネル モデルでの符号化ビット誤り率 (BER) など、End-to-Endのパフォーマンスに関するアルゴリズムを評価するための時間持っているわけではない。

Sionna は、複雑な通信システムをEnd-to-Endで迅速にモデル化するためのアプリケーション プログラミング インターフェイス (API) を提供すると同時に、研究対象の部分を適応させることも可能となっている。これにより、自分の研究に集中できると同時に、その研究をより影響力のあるものにし、他の人が簡単に再現できるようになります。
