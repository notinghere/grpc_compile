# grpc 编译相关

## 参考
- 官方文档
> https://github.com/grpc/grpc/blob/master/BUILDING.md

> https://blog.towind.fun/2021/04/21/linux-docker-install-grpc/#/%E5%AE%89%E8%A3%85%E5%9F%BA%E6%9C%AC%E4%BE%9D%E8%B5%96

## 编译

- 下载依赖

```sh
 $ [sudo] apt-get install build-essential autoconf libtool pkg-config
```

- 下载代码和三方库（submodule update 这一步容易卡住）
 ```sh
 $ git clone -b v1.50.1 https://github.com/grpc/grpc
 $ cd grpc
 $ git submodule update --init
```

- 编译
```sh
$ mkdir -p cmake/build
$ cd cmake/build
$ cmake ../..
$ make
```

- 安装
```sh
# 安装到 $HOME/.local 中
export MY_INSTALL_DIR=$HOME/.local
# 确保目录存在
mkdir -p $MY_INSTALL_DIR
# 添加该路径下的 bin 目录到环境变量
export PATH="$PATH:$MY_INSTALL_DIR/bin"

# 创建存放编译 gRPC 结果的目录
mkdir -p cmake/build
# 进入到该目录
pushd cmake/build
# 生成编译 gRPC 的 Makefile 文件
# 其中 DCMAKE_INSTALL_PREFIX 指定了 gRPC 的安装路径
cmake -DgRPC_INSTALL=ON \
    -DgRPC_BUILD_TESTS=OFF \
    -DCMAKE_INSTALL_PREFIX=$MY_INSTALL_DIR \
    ../..
# 执行编译
# ${JOBS_NUM} 为同时执行的线程数，应替换为数字，下同
make -j ${JOBS_NUM}
# 安装 gRPC
make install
```

- 需要生成动态库
```sh
# 生成编译 gRPC 的 Makefile 文件
cmake -DBUILD_SHARED_LIBS=ON ../..
```
