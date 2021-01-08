### tkDNN
This library uses code from the [tkDNN project](https://github.com/ceccocats/tkDNN). You are required to cite them if you use this to assist in your inference.

tkDNN is a Deep Neural Network library built with cuDNN and tensorRT primitives, specifically thought to work on NVIDIA Jetson Boards. It has been tested on TK1(branch cudnn2), TX1, TX2, AGX Xavier, Nano and several discrete GPUs.
The main goal of this project is to exploit NVIDIA boards as much as possible to obtain the best inference performance. It does not allow training. 


If you use tkDNN in your research, please cite the [following paper](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=9212130&casa_token=sQTJXi7tJNoAAAAA:BguH9xCIY48MxbtDS3LXzIXzO-9sWArm7Hd7y7BwaLmqRuM_Gx8bOYizFPNMNtpo5K0kB-P-). For use in commercial solutions, write at gattifrancesco@hotmail.it and micaela.verucchi@unimore.it or refer to https://hipert.unimore.it/ .

```
@inproceedings{verucchi2020systematic,
  title={A Systematic Assessment of Embedded Neural Networks for Object Detection},
  author={Verucchi, Micaela and Brilli, Gianluca and Sapienza, Davide and Verasani, Mattia and Arena, Marco and Gatti, Francesco and Capotondi, Alessandro and Cavicchioli, Roberto and Bertogna, Marko and Solieri, Marco},
  booktitle={2020 25th IEEE International Conference on Emerging Technologies and Factory Automation (ETFA)},
  volume={1},
  pages={937--944},
  year={2020},
  organization={IEEE}
}
```



## Index
- [tkDNN](#tkdnn)
  - [Index](#index)
  - [Dependencies](#dependencies)
  - [About OpenCV](#about-opencv)
  - [How to compile this repo](#how-to-compile-this-repo)
  - [Workflow](#workflow)




## Dependencies
This branch works on every NVIDIA GPU that supports the dependencies:
* CUDA 10.0
* CUDNN 7.603
* TENSORRT 6.01
* OPENCV 3.4
* yaml-cpp 0.5.2 (sudo apt install libyaml-cpp-dev)

## About OpenCV
To compile and install OpenCV4 with contrib us the script ```install_OpenCV4.sh```. It will download and compile OpenCV in Download folder.
```
bash scripts/install_OpenCV4.sh
```
When using openCV not compiled with contrib, comment the definition of OPENCV_CUDACONTRIBCONTRIB in include/tkDNN/DetectionNN.h. When commented, the preprocessing of the networks is computed on the CPU, otherwise on the GPU. In the latter case some milliseconds are saved in the end-to-end latency. 

## How to compile this repo
Build with cmake. If using Ubuntu 18.04 a new version of cmake is needed (3.15 or above). 
```
git clone https://github.com/nightduck/keras2trt
cd keras2trt
mkdir build
cd build
cmake .. 
make
```

## Workflow
Steps needed to do inference on tkDNN with a custom neural network. 
* Build and train a NN model with keras
* Export weights as hdf5 file
* Copy paste model definition into keras2trt.py, and set name of weights file
* Run script to generate TensorRT engine
* Declare NetworkRT engine in your C++ program, giving it the TensorRT engine path
* call output = netRT->infer(data)
* For pre and post processing, you're on your own


## References

1. Redmon, Joseph, and Ali Farhadi. "YOLO9000: better, faster, stronger." Proceedings of the IEEE conference on computer vision and pattern recognition. 2017.
2. Redmon, Joseph, and Ali Farhadi. "Yolov3: An incremental improvement." arXiv preprint arXiv:1804.02767 (2018).
3. Yu, Fisher, et al. "Deep layer aggregation." Proceedings of the IEEE conference on computer vision and pattern recognition. 2018.
4. Zhou, Xingyi, Dequan Wang, and Philipp Krähenbühl. "Objects as points." arXiv preprint arXiv:1904.07850 (2019).
5. Sandler, Mark, et al. "Mobilenetv2: Inverted residuals and linear bottlenecks." Proceedings of the IEEE conference on computer vision and pattern recognition. 2018.
6. He, Kaiming, et al. "Deep residual learning for image recognition." Proceedings of the IEEE conference on computer vision and pattern recognition. 2016.
7. Wang, Chien-Yao, et al. "CSPNet: A New Backbone that can Enhance Learning Capability of CNN." arXiv preprint arXiv:1911.11929 (2019).
8. Bochkovskiy, Alexey, Chien-Yao Wang, and Hong-Yuan Mark Liao. "YOLOv4: Optimal Speed and Accuracy of Object Detection." arXiv preprint arXiv:2004.10934 (2020).
9. Bochkovskiy, Alexey, "Yolo v4, v3 and v2 for Windows and Linux" (https://github.com/AlexeyAB/darknet)
