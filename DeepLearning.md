
# DeepLearning

## H2O

### Install

```
brew install jenv
jenv add /Library/Java/JavaVirtualMachines/jdk1.8.0_45.jdk/Contents/Home
jenv add /Library/Java/JavaVirtualMachines/jdk1.7.0_79.jdk/Contents/Home
# java1.8 node pythonに依存
git clone https://github.com/h2oai/h2o-3
cd h2o-3
jenv local 1.7
```

# chainer
・CUDAドライバのアップデート
http://www.nvidia.co.jp/object/macosx-cuda-7.5.22-driver-jp.html

・CUDAツールキットのインストール
https://developer.nvidia.com/cuda-downloads

・環境変数の設定
# CUDA Toolkit
export PATH=/Developer/NVIDIA/CUDA-7.5/bin:$PATH
export DYLD_LIBRARY_PATH=/Developer/NVIDIA/CUDA-7.5/lib:$DYLD_LIBRARY_PATH

・cuda環境を反映させるためchainerを再インストール
LDFLAGS="-rpath /usr/local/cuda/lib/ -F/Library/Frameworks -framework CUDA" pip install --no-cache-dir chaine
