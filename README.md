# Point cloud ensemble

Here one can find code snippets used in the paper: **3D Object Recognition with Ensemble Learning—A Study of Point Cloud-Based Deep Learning Models** [springer](https://link.springer.com/chapter/10.1007/978-3-030-33723-0_9), [arxiv](https://arxiv.org/abs/1904.08159)

We focus on the direct point cloud processing because such architectures can be adapted to the analysis of different kinds of sets. Seven architectures are used:
- DeepSets [1]
- PointNet [2]
- PointNet++ [3]
- SO-Net [4]
- KCNet [5]
- DGCNN [6]
- PointCNN [7]. 

---

# Deep Learning on Jetson TX2

As a part of the project, we set up all above mentioned DL methods on Jetson TX2, which is a reasonable choice for energy efficient mobile robotic applications. We need to set up three different DL frameworks, since PointNet, PointNet++, DGCNN, and PointCNN are using Tensorflow, DeepSets and SO-Net are using PyTorch and KCNet is using Caffe. Here are some tips, how we did it (pls_help).

### Tensorflow and Jetpack 3.2

Jetson TX2 need to be flushed with Jetpack 3.2, this results with CUDA v9.0 and cuDNN v7.0.5. Please do apt update & upgrade afterwards. Here are all dependencies listed:

```sh
$ sudo apt-get install python-pip
$ sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev \
                       libhdf5-dev libhdf5-serial-dev protobuf-compiler
$ sudo apt-get install --no-install-recommends libboost-all-dev
$ sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev
$ sudo apt-get install libatlas-base-dev libopenblas-dev
```

### How to build Tensorflow 1.5 on Jetson TX2

Daniel, PLS, find it on nvidia forum.

### How to build PyTorch on Jetson TX2

First we need to build LLVM from source, I choose the LLVM 7.0.0 version from [here](http://releases.llvm.org/download.html).

```sh
$ cd llvm-7.0.1.src
$ mkdir build
$ cd build

#Configure to build only the Release version and only for the ARM, x86 and AArch64 architectures
$ cmake .. -DCMAKE_BUILD_TYPE=Release -DLLVM_TARGETS_TO_BUILD="ARM;X86;AArch64"

#Start the build from the build directory
$ cmake -j4 --build .

#install it from the build directory:
$ sudo cmake --build . --target install
```

Next, we can build pytorch:
```sh
# clone pyTorch repo
$ git clone http://github.com/pytorch/pytorch
$ cd pytorch
$ git submodule update --init

# install prereqs
$ sudo pip install -U setuptools
$ sudo pip install -r requirements.txt

# Develop Mode:
$ python setup.py build_deps
$ sudo python setup.py develop
```

And veryfy the installation:

```sh
# Verify CUDA (from python interactive terminal)
# import torch
# print(torch.__version__)
# print(torch.cuda.is_available())
# a = torch.cuda.FloatTensor(2)
# print(a)
# b = torch.randn(2).cuda()
# print(b)
# c = a + b
# print(c)
```

### How to build caffe (for KCNet)

KCNet authors prepared cloned version of caffe for KCNet repository. Here's the build procedure:

---

# Authors

This work was done with collaboration with Łukasz Chechliński [LinkedIn](https://www.linkedin.com/in/%C5%82ukasz-chechli%C5%84ski-38b469185) and Tarek El-Gaaly [LinkedIn](https://www.linkedin.com/in/tarekelgaaly/). 

---

# References

[1] M.  Zaheer,  S.  Kottur,  S.  Ravanbakhsh,  B.  Poczos,  R.  R. Salakhutdinov, and A. J. Smola.  Deep sets.  In Advances in Neural Information Processing Systems, pages 3391–3401, 2017
- [paper](https://arxiv.org/abs/1703.06114)
- [original_repo](https://github.com/manzilzaheer/DeepSets)
- [forked_repo](https://github.com/dkoguciuk/DeepSets)

[2] C. R. Qi, H. Su, K. Mo, and L. J. Guibas.  Pointnet:  Deep learning on point sets for 3d classification and segmentation. Proc.  Computer  Vision  and  Pattern  Recognition  (CVPR), IEEE, 1(2):4, 2017
- [paper](https://arxiv.org/abs/1612.00593)
- [original_repo](https://github.com/charlesq34/pointnet)
- [forked_repo](https://github.com/dkoguciuk/pointnet)

[3] C. R. Qi, L. Yi, H. Su, and L. J. Guibas. Pointnet++: Deep hierarchical feature learning on point sets in a metric space. In Advances in Neural Information Processing Systems, pages 5099–5108, 2017
- [paper](https://arxiv.org/abs/1706.02413)
- [original_repo](https://github.com/charlesq34/pointnet2)
- [forked_repo](https://github.com/dkoguciuk/pointnet2)

[4] J. Li, B. M. Chen, and G. H. Lee.  So-net:  Self-organizing network  for  point  cloud  analysis. CoRR,  abs/1803.04249, 2018
- [paper](https://arxiv.org/abs/1803.04249)
- [original_repo](https://github.com/lijx10/SO-Net)
- [forked_repo](https://github.com/dkoguciuk/so-net)

[5] Y. Shen, C. Feng, Y. Yang, and D. Tian.  Mining point cloud local structures by kernel correlation and graph pooling.  In Proceedings  of  the  IEEE  Conference  on  Computer  Vision and Pattern Recognition, volume 4, 2018
- [paper](https://arxiv.org/abs/1712.06760)
- [project_page](http://www.merl.com/research/?research=license-request&sw=KCNet)

[6] Y. Wang, Y. Sun, Z. Liu, S. E. Sarma, M. M. Bronstein, and J. M. Solomon. Dynamic graph cnn for learning on point clouds.arXiv preprint arXiv:1801.07829, 2018
- [paper](https://arxiv.org/abs/1801.07829)
- [original_repo](https://github.com/WangYueFt/dgcnn)
- [forked_repo](https://github.com/dkoguciuk/dgcnn)

[7] Y. Li, R. Bu, M. Sun, and B. Chen. Pointcnn. arXiv preprint arXiv:1801.07791, 2018
- [paper](https://arxiv.org/abs/1801.07791)
- [original_repo](https://github.com/yangyanli/PointCNN)
- [forked_repo](https://github.com/dkoguciuk/pointcnn)
