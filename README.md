# Super Fast and Accurate 3D Object Detection based on 3D LiDAR Point Clouds (SFA3D)

[![python-image]][python-url]
[![pytorch-image]][pytorch-url]

---

## Features
- [x] Super fast and accurate 3D object detection based on LiDAR
- [x] Fast training, fast inference
- [x] An Anchor-free approach
- [x] No Non-Max-Suppression
- [x] Support [distributed data parallel training](https://github.com/pytorch/examples/tree/master/distributed/ddp)
- [x] Release pre-trained models 

## Demonstration (on a single RTX 3080Ti)

[![demo](https://img.youtube.com/vi/fcce6t55grg/0.jpg)](https://youtu.be/fcce6t55grg)


**[Youtube link](https://youtu.be/fcce6t55grg)**

## 2. Getting Started
### 2.1. Requirement

The instructions for setting up a virtual environment is [here](https://github.com/maudzung/virtual_environment_python3).


### 2.2. Data Preparation
Download the 3D KITTI detection dataset from [here](http://www.cvlibs.net/datasets/kitti/eval_object.php?obj_benchmark=3d).

The downloaded data includes:

- Velodyne point clouds _**(29 GB)**_
- Training labels of object data set _**(5 MB)**_
- Camera calibration matrices of object data set _**(16 MB)**_
- **Left color images** of object data set _**(12 GB)**_ (For visualization purpose only)


Please make sure that you construct the source code & dataset directories structure as below.

### 2.3. How to run

#### 2.3.1. Visualize the dataset 

To visualize 3D point clouds with 3D boxes, let's execute:

```shell script
cd sfa/data_process/
python kitti_dataset.py
```


#### 2.3.2. Inference

The pre-trained model was pushed to this repo.

```
python test.py --gpu_idx 0 --peak_thresh 0.2
```

#### 2.3.3. Making demonstration

```
python demo_2_sides.py --gpu_idx 0 --peak_thresh 0.2
```

The data for the demonstration will be automatically downloaded by executing the above command.


#### 2.3.4. Training

##### 2.3.4.1. Single machine, single gpu

```shell script
python train.py --gpu_idx 0
```

##### 2.3.4.2. Distributed Data Parallel Training
- **Single machine (node), multiple GPUs**

```
python train.py --multiprocessing-distributed --world-size 1 --rank 0 --batch_size 64 --num_workers 8
```

- **Two machines (two nodes), multiple GPUs**

   - _**First machine**_
    ```
    python train.py --dist-url 'tcp://IP_OF_NODE1:FREEPORT' --multiprocessing-distributed --world-size 2 --rank 0 --batch_size 64 --num_workers 8
    ```

   - _**Second machine**_
    ```
    python train.py --dist-url 'tcp://IP_OF_NODE2:FREEPORT' --multiprocessing-distributed --world-size 2 --rank 1 --batch_size 64 --num_workers 8
    ```

#### Tensorboard

- To track the training progress, go to the `logs/` folder and 

```shell script
cd logs/<saved_fn>/tensorboard/
tensorboard --logdir=./
```

- Then go to [http://localhost:6006/](http://localhost:6006/)


## References

[1] CenterNet: [Objects as Points paper](https://arxiv.org/abs/1904.07850), [PyTorch Implementation](https://github.com/xingyizhou/CenterNet) <br>
[2] RTM3D: [PyTorch Implementation](https://github.com/maudzung/RTM3D) <br>
[3] Libra_R-CNN: [PyTorch Implementation](https://github.com/OceanPang/Libra_R-CNN)

_The YOLO-based models with the same BEV maps input:_ <br>
[4] Complex-YOLO: [v4](https://github.com/maudzung/Complex-YOLOv4-Pytorch), [v3](https://github.com/ghimiredhikura/Complex-YOLOv3), [v2](https://github.com/AI-liu/Complex-YOLO)

*3D LiDAR Point pre-processing:* <br>
[5] VoxelNet: [PyTorch Implementation](https://github.com/skyhehe123/VoxelNet-pytorch)

## Folder structure

```
${ROOT}
└── checkpoints/
    ├── fpn_resnet_18/    
        ├── fpn_resnet_18_epoch_300.pth
└── dataset/    
    └── kitti/
        ├──ImageSets/
        │   ├── test.txt
        │   ├── train.txt
        │   └── val.txt
        ├── training/
        │   ├── image_2/ (left color camera)
        │   ├── calib/
        │   ├── label_2/
        │   └── velodyne/
        └── testing/  
        │   ├── image_2/ (left color camera)
        │   ├── calib/
        │   └── velodyne/
        └── classes_names.txt
└── sfa/
    ├── config/
    │   ├── train_config.py
    │   └── kitti_config.py
    ├── data_process/
    │   ├── kitti_dataloader.py
    │   ├── kitti_dataset.py
    │   └── kitti_data_utils.py
    ├── models/
    │   ├── fpn_resnet.py
    │   ├── resnet.py
    │   └── model_utils.py
    └── utils/
    │   ├── demo_utils.py
    │   ├── evaluation_utils.py
    │   ├── logger.py
    │   ├── misc.py
    │   ├── torch_utils.py
    │   ├── train_utils.py
    │   └── visualization_utils.py
    ├── demo_2_sides.py
    ├── demo_front.py
    ├── test.py
    └── train.py
├── README.md 
└── requirements.txt
```



[python-image]: https://img.shields.io/badge/Python-3.6-ff69b4.svg
[python-url]: https://www.python.org/
[pytorch-image]: https://img.shields.io/badge/PyTorch-1.5-2BAF2B.svg
[pytorch-url]: https://pytorch.org/
