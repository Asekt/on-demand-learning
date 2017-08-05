## On-Demand Learning for Deep Image Restoration
[[Project Page]](http://vision.cs.utexas.edu/projects/on_demand_learning/)    [[arXiv]](https://arxiv.org/abs/1612.01380)<br/>

This repository contains the training code for our [ICCV 2017 paper on All-Rounders for Image Restoration](http://vision.cs.utexas.edu/projects/on_demand_learning/gao-iccv2017.pdf). We propose an on-demand learning algorithm for training image restoration models with deep convolutional neural networks that can generalize across difficulty levels. This repo contains our code for four image restoration tasks---image inpainting, pixel interpolation, image deblurring, and image denoising. The code is adapted from [Soumith's DCGAN Implementation](https://github.com/soumith/dcgan.torch) and [Deepak Pathak's Context Encoder Implementation](https://github.com/pathak22/context-encoder).

If you find our code or project useful in your research, please cite:

        @inproceedings{gao2017on-demand,
          title={On-Demand Learning for Deep Image Restoration},
          author={Gao, Ruohan and Grauman, Kristen},
          booktitle={ICCV},
          year={2017}
        }

Please also consider citing:

        @inproceedings{radford2016unsupervised,
          title={Unsupervised representation learning with deep convolutional generative adversarial networks},
          author={Radford, Alec and Metz, Luke and Chintala, Soumith},
          booktitle={ICLR},
          year={2016}
        }
        
        @inproceedings{pathak2016context,
          title={Context Encoders: Feature Learning by Inpainting},
          author={Pathak, Deepak and Krahenbuhl, Philipp and Donahue, Jeff and Darrell, Trevor and Efros, Alexei A},
           booktitle={CVPR},
          year={2016}
        }
### Contents
0. [Preparation](#0-preparation)
1. [Image Inpainting](#1-image-inpainting)
2. [Pixel Interpolation](#2-pixel-interpolation)
3. [Image Deblurring](#3-image-deblurring)
4. [Image Denoising](#4-image-denoising)
5. [Image Denoising of Arbitrary Sizes](#5-image-denoising-anysize)
6. [Sample Code for All Training Schemes](#5-sample-code-for-all-training-schemes)
### 1) Preparation

1. Install Torch: http://torch.ch/docs/getting-started.html

2. Install torch-opencv: https://github.com/VisionLabs/torch-opencv/wiki/Installation

3. Clone the repository
  ```Shell
  git clone https://github.com/rhgao/on-demand-learning.git
  ```
  
4. Download [CelebA](http://mmlab.ie.cuhk.edu.hk/projects/CelebA.html) or [SUN397](http://vision.cs.princeton.edu/projects/2010/SUN/) dataset, or prepare your own dataset. 
  ```Shell
  # put all training images inside my_train_set/images/
  mkdir -p /your_path/my_train_set/images/
  # put all validation images inside my_val_set/images/
  mkdir -p /your_path/my_val_set/images/
  # put all testing images inside my_test_set/images/
  mkdir -p /your_path/my_test_test/images/
  cd on-demand-learning/
  ln -sf /your_path dataset
  ```

5. [Optional] If you want to run a quick demo for the four image restoration tasks, please download our pre-trained models using the following script.
  ```Shell
  cd models
  #download image inpainting model
  bash download_model.sh inpainting
  #download pixel interpolation model
  bash download_model.sh pixelInterpolation
  #download image deblurring model
  bash download_model.sh deblurring
  #download image denoising model
  bash download_model.sh denoising
  #download our image denoising model equipped to denoise images of any size
  bash download_model.sh denoise_anysize
  ```
  
6. [Optional] Install the Display Package, which enables you to track the training progress. If you don't want to install it, please set `display=0` in `train.lua`.
  ```Shell
  luarocks install https://raw.githubusercontent.com/szym/display/master/display-scm-0.rockspec
  #start the display server
  th -ldisplay.start 8000
  # on client side, open in browser: http://localhost:8000/
  # You can then see the training progress in your browser window.
  ```
        
### 2) Image Inpainting
1. Demo
  ```Shell
  # Test the image inpainting model on various corruption levels
  cd inpainting
  DATA_ROOT=../dataset/my_test_set name=inpaint_demo net=../models/inpainting_net_G.t7 manualSeed=333 gpu=1 display=1 th demo.lua
  # Demo results saved as inpaint_demo.png
  ```
  
2. Train the model
  ```Shell
  # Train your own image inpainting model
  cd inpainting
  DATA_ROOT=../dataset/my_train_set name=inpaint niter=250 loadSize=96 fineSize=64 display=1 display_iter=50 gpu=1 th train.lua
  ```
  
### 3) Pixel Interpolation
1. Demo
  ```Shell
  # Test the pixel interpolation model on various corruption levels
  cd pixelInterpolation
  DATA_ROOT=../dataset/my_test_set name=pixelInter_demo net=../models/pixelInterpolation_net_G.t7 manualSeed=333 gpu=1 display=1 th demo.lua
  # Demo results saved as pixelInter_demo.png
  ```
  
2. Train the model
  ```Shell
  # Train your own pixel interpolation model
  cd pixelInterpolation
  DATA_ROOT=../dataset/my_train_set name=pixel niter=250 loadSize=96 fineSize=64 display=1 display_iter=50 gpu=1 th train.lua
  ```
  
### 4) Image Deblurring
1. Demo
  ```Shell
  # Test the image deblurring model on various corruption levels
  cd deblurring
  DATA_ROOT=../dataset/my_test_set name=deblur_demo net=../models/deblurring_net_G.t7 manualSeed=333 gpu=1 display=1 th demo.lua
  # Demo results saved as deblur_demo.png
  ```
  
2. Train the model
  ```Shell
  # Train your own image deblurring model
  cd deblurring
  DATA_ROOT=../dataset/my_train_set name=deblur niter=250 loadSize=96 fineSize=64 display=1 display_iter=50 gpu=1 th train.lua
  ```

### 5) Image Denoising
1. Demo
  ```Shell
  # Test the image denoising model on various corruption levels
  cd deblurring
  DATA_ROOT=../dataset/my_test_set name=denoise_demo net=../models/denoising_net_G.t7 sigma=25 manualSeed=333 gpu=1 display=1 th demo.lua
  # Demo results saved as denoise_demo.png
  ```
  
2. Train the model
  ```Shell
  # Train your own image denoising model
  cd denoising
  DATA_ROOT=../dataset/my_train_set name=denoise niter=1500 loadSize=96 fineSize=64 display=1 display_iter=50 gpu=1 th train.lua
  ```

### 6) Image Denoising of Arbitrary Sizes
Denoising/DB11 contains 11 classic images commonly used to evaluate image denoising algorithms. Because the input of our network is of size 64 x 64, given an image of arbitrary size (assuming larger than 64 x 64), we use a sliding-window approach to denoise each patch separately, then average outputs at overlapping pixels.
  ```Shell
  # Denoise classic image Lena from DB11 dataset
  cd denoise_anysize
  img_path=DB11/Lena.png name=denoise net=../models/denoise_anysize_net_G.t7 sigma=25 stepSize=3 gpu=1 th denoise.lua
  # Denoising results saved as denoise.png
  ```

### 7) Sample Code for Training Schemes
training_schemes contains a sample script that can be adapted for different training schemes attempted in the paper, including on-demand learning, rigid-joint learning, staged (anti-)curriculum learning and cumulative (anti-)curriculum learning.
