# 仮想環境の構築からTensorFlowのインストールまで
環境構築の際に、いくつかの点で詰まったので解決策を記載する。
## 仮想環境の構築
仮想環境の構築には、Anacondaを使用する。Anacondaを起動した後、EnvironmentsタブからOpen Terminalを選択する。

### ターミナル上で、仮想環境を作成する。

    conda create --name tf python=3.9

### 仮想環境を有効化する。
    
    conda activate tf

### GPUのセットアップ

    conda install -c conda-forge cudatoolkit=11.2 cudnn=8.1.0


### TensorFlowのインストール

Anything above 2.10 is not supported on the GPU on Windows Native

    pip install "tensorflow<2.11" 

### インストールの確認

    python -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"

## 詰まったポイントと解決策
### condaとpipの共存
ここが一番の鬼門だった。


結論として、condaでCUDAとcuDNNをインストールし、pipでTensorFlowをインストールすることで、共存を実現した。理由は、pipでインストールするライブラリがTensorFlowを勝手にインストールしてしまうからである。

condaとpipは互いのライブラリを把握していないため、二重にインストールが発生する場合がある。また、GPUを活用したいと考えているので、CUDAとcuDNN、TensorFlowのバージョンを正しく指定する必要があった。

しかし、機械学習に利用したいライブラリがpipのみで提供されているため、condaとpipの共存を考える必要がある。もしくは最初からvenvを使用する方法が考えられるが、以下の理由からcondaを採用した。

### condaの利点
GPUの活用において、CUDAとcuDNNのバージョン管理やパスの設定は初心者にとってネックとなる部分だと思うが、condaを使用することで、自動でインストールされるため、初心者にとっては、手間が省ける。

今回は使用していないが、

    conda install tensorflow=2.8.*=gpu_*

とすることで、Tensorflow、CUDA、cuDNNが自動でインストールされる。


### TensorFlowのバージョン
TensorFlowのバージョンは、2.10以上はWindows NativeでGPUを活用できないため、2.10以下を指定する必要がある。

## モデルの学習
上記の方法で、GPUを活用した環境構築ができたので、モデルの学習を行う。

現在の状況として、TensorFlowとPythonはGPUを認識している。

    from tensorflow.python.client import　device_lib
    device_lib.list_local_devices()

上記のコードを実行すると、以下のような結果が得られる。

    [name: "/device:CPU:0"
    device_type: "CPU"
    memory_limit: 268435456
    locality {
    }
    incarnation: 12142248063336448492
    xla_global_id: -1
    , name: "/device:GPU:0"
    device_type: "GPU"
    memory_limit: 10849157120
    locality {
     bus_id: 1
      links {
      }
    }
    ncarnation: 17153162280940221000
    physical_device_desc: "device: 0, name: NVIDIA GeForce GTX 1080 Ti, pci bus id: 0000:01:00.0, compute capability: 6.1"
    xla_global_id: 416903419
    ]

しかし、モデルの学習を実行しようとした際に、

    libdevice not found at ./libdevice.10.bc

というエラーが発生した。明確な理由はわかっていないが、存在しているファイルを認識できていないようだ。
Linux用の解決策は多数見つかったが、Windowsでは解決しなかったので別の方法を探した。

### 解決策

    import os
    os.environ["XLA_FLAGS"]='--xla_gpu_cuda_data_dir="%s"'%os.environ["CUDA_PATH"].replace('\\','/')

上記のコードをプログラムの先頭に追加することで、解決した。
コンパイル時のCUDAフォルダと別の場所に現在のCUDAがあるため、このエラーが出ていると考えられる。