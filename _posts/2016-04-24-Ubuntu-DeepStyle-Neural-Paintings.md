
---
layout: post
title: Ubuntu DeepStyle "Neural Paintings"
---

source: http://www.makeuseof.com/tag/create-neural-paintings-deepstyle-ubuntu/

    sudo apt-get install git lua5.2 luarocks luajit libprotobuf-dev protobuf-compiler
    curl -s https://raw.githubusercontent.com/torch/ezinstall/master/install-all | bash

    test torch:
    luajit -ltorch

install loadcaffe:

    sudo luarocks install loadcaffe

if you have an old version of gcc you have to (source):

    git clone https://github.com/szagoruyko/loadcaffe
    cd loadcaffe
    nano CMakeLists.txt
    change the line: add_definitions(-std=c++11) to: add_definitions(-std=c++0x)
    luarocks make

then:

    sudo luarocks install image
    sudo luarocks install nn

to install cuda support:

    sudo luarocks install cutorch
    sudo luarocks install cunn

but since the max allocable memory is the max memory of your gpu it is best to use cudnn, you have to register here: https://developer.nvidia.com/cudnn, then download the package and install (source):

    tar -xvzf cudnn-7.0-linux-x64-v4.0-prod.tgz
    sudo cp lib64/* /usr/local/cuda/lib64/
    sudo cp include/cudnn.h /usr/local/cuda/include/
    sudo luarocks install cudnn

and test it by running:

    th neural_style.lua -gpu 0 -backend cudnn

if you have problems running it and it gives the error (source):

    [CUT] 'libcudnn (R4) not found in library path.
    Please install CuDNN from https://developer.nvidia.com/cuDNN
    Then make sure files named as libcudnn.so.4 or libcudnn.4.dylib are placed in your library load path (for example /usr/local/lib , or manually add a path to LD_LIBRARY_PATH)

you have to set the library path correctly:

    export LD_LIBRARY_PATH=/usr/local/cuda-7.0/lib64:$LD_LIBRARY_PATH

now we can create our work folder and clone the git repo of the neuralnetwork,
it's nice to do that were we a bit of space since it will use about ~600MB:

    sudo git clone https://github.com/jcjohnson/neural-style.git
    cd neural-style

now we can download the model, it will take a bit:

    sudo sh models/download_models.sh

Images generated with simpler nerual network (nin_imagenet_conv):

nn_15

nn_15

nn_15

Images generated with neural-style:

ex_32

ex_33

ex_34

ex_35

ex_36

ex_37

ex_38

ex_39

ex_39_opti1

ex_40_opti2

ex_42_opti2


Notes:
using a smaller network: http://liipetti.net/erratic/2016/03/21/using-nin-imagenet-conv-in-neural-style/
ram discussion: https://github.com/jcjohnson/neural-style/issues/150
main repo: https://github.com/jcjohnson/neural-style
NIN_network: https://drive.google.com/folderview?id=0B0IedYUunOQINEFtUi1QNWVhVVU&usp=drive_web
cudnn: https://github.com/soumith/cudnn.torch
cunn issues: https://github.com/torch/cunn/issues/80
other issues: https://github.com/hughperkins/clnn/issues/18
