# ddp-resnet101-cifar10

- ResNet101をCIFAR10で分散学習するためのコード

## 使い方

- Pythonの環境には[UV](https://docs.astral.sh/uv/)を推奨しておきます

- venvやcondaなどその他の環境を使っても 必要なパッケージさえインストールできれば問題ないです

  - condaを環境に使用している場合、uvとは競合するため絶対にuvのインストールを行わないでください

- エディタ&実行環境はVSCodeを想定しています それ以外のエディタは知りません

### リポジトリのクローン

```bash
git clone https://github.com/rits-menglab/ddp-resnet101-cifar10.git
```

### CUDAバージョンの確認方法

```bash
nvcc -V
```

- 出力例1

```bash
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2024 NVIDIA Corporation
Built on Thu_Jun__6_02:18:23_PDT_2024
Cuda compilation tools, release 12.5, V12.5.82
Build cuda_12.5.r12.5/compiler.34385749_0
```

この場合はCUDA 12.5がインストールされているため、[12.4↑](#cuda-124)の環境を構築してください。

- 出力例2

```bash
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2023 NVIDIA Corporation
Built on Wed_Nov_22_10:17:15_PST_2023
Cuda compilation tools, release 12.3, V12.3.107
Build cuda_12.3.r12.3/compiler.33567101_0
```

この場合はCUDA 12.3がインストールされています。  
CUDAは下位互換のため、[12.1～](#cuda-121)の環境を構築してください。  
11.8～の環境でも問題ないですが、近いバージョンの環境を構築が推奨されます。

- 出力例3

```bash
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2022 NVIDIA Corporation
Built on Tue_Mar__8_18:18:20_PST_2022
Cuda compilation tools, release 11.6, V11.6.124
Build cuda_11.6.r11.6/compiler.31057947_0
```

現在のPytorchの最低バージョンよりも低い状態です。  
まず最初にCUDAのバージョンアップを検討してください。  
バージョンアップが行えない場合、[最も近い上位バージョンの環境](#cuda-118)を構築してください。

### UVでの仮想環境作成

<a id="cuda-124"></a>

#### CUDA 12.4↑

```bash
uv sync --dev
```

<a id="cuda-121"></a>

#### CUDA 12.1～

```bash
uv sync --dev --extra cu121
```

<a id="cuda-118"></a>

#### CUDA 11.8～

```bash
uv sync --dev --extra cu118
```

#### CPU

```bash
uv sync --dev --extra cpu
```

### 実行

以下のコマンドを実行(GPUが1台のマシンに2台搭載されている場合)

- プロセス0

```bash
python3 train.py --master-addr 127.0.0.1 --master-port 62001 --world-size 2 --local-rank 0 --dir ./ --bsz 32 --epoch 300
```

```bash
python3 train.py --master-addr 127.0.0.1 --master-port 62001 --world-size 2 --local-rank 1 --dir ./ --bsz 32 --epoch 300
```
