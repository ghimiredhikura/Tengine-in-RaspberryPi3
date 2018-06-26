# Tengine-in-RaspberryPi3

## Introduction

Tengine, developed by OPEN AI LAB, is a lite, high-performance, and modular inference engine for embedded device. Currently it supports Caffe/onnx/MXNet/Tenserfolo models. 
More detail about the library can be found in [tengine official git repository](https://github.com/OAID/Tengine). 

## Installing Tengine

For official installation guide please refer to this [install](https://github.com/OAID/Tengine/blob/master/doc/install.md) page. 

#### 1. Tengine Installation in AMD64 PC
#### 2. Tengine Installation in ARMv7-RaspberryPi3

This guide gives instructions on how to build and test Tengine in RaspberryPi3 device. 

##### 2.1 Download Source code
```buildoutcfg
git clone https://github.com/OAID/tengine/ 
```
##### 2.2 Install Depency Libraries
```buildoutcfg
 sudo apt install libprotobuf-dev protobuf-compiler libboost-all-dev libgoogle-glog-dev
```
```buildoutcfg
 sudo apt install libopencv-dev
```
##### 2.3 Prepare config files 
- copy config example file
```commandline
cd ~/tengine
cp makefile.config.example makefile.config
```
- edit ```makefile.config```
If you are using ARMv8 on in your RaspberryPi3 model, you can probably build with ```CONFIG_ARCH_ARM64``` valid (not yet tested). But if your model is ARMv7 you better to build with Openblas. Therefore install openblas first. 
```buildoutcfg
sudo apt-get install libopenblas-dev
```
Now enable openblas option, make sure default arm64 option is set off. 
```buildoutcfg
CONFIG_ARCH_BLAS=y
```
By default caffe model serializer option is valid. If you want to support other platforms uncomment them. <br />
Finally the makefile.config file should look like [makefile.config.RaspberryPi3](./makefile.config.RaspberryPi3)

##### 2.4 Build library
```buildoutcfg
cd ~/tengine
make
make install
```

##### 2.5 Test (Run Benchmark Examples)
```buildoutcfg
cd ~/tengine/examples/benchmark
mkdir build
cd build
cmake -DTENGINE_DIR=/path/to/your/tengine/dir .. 
make
```
If you want to limit the available thread or want to assign specific thread follow this official [benchmark](https://github.com/OAID/Tengine/blob/master/doc/benchmark.md) page.
- assign different cpu cores using these two methods. 
```buildoutcfg
 $ export TENGINE_CPU_LIST=0,1,2,3
 $ ./tests/bin/bench_sqz –p 0,1,2,3
```

Example: Assign single core 
```buildoutcfg
$ export TENGINE_CPU_LIST=0
```

Test model timing: <br />

Copy ```bench_sphereface06.cpp, models, tests``` inside ```tengine/examples/benchmark``` <br />

Rebuild benchmark examples <br />
```buildoutcfg
$ ./bench_sphereface06 -p 0
```
