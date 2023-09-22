# 仮想環境の構築からTensorFlowのインストールまで
環境構築の際に、いくつかの点で詰まったので解決策を記載する。
## 仮想環境の構築
仮想環境の構築には、Anacondaを使用する。Anacondaを起動した後、EnvironmentsタブからOpen Terminalを選択する。

ターミナル上で、仮想環境を作成する。

    conda create --name tf python=3.9

仮想環境を有効化する。
    
        conda activate tf

GPUのセットアップ
    conda install -c conda-forge cudatoolkit=11.2 cudnn=8.1.0


TensorFlowのインストール
    # Anything above 2.10 is not supported on the GPU on Windows Native
    pip install "tensorflow<2.11" 

インストールの確認
    python -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"

