# An Empirical Study of Ground Segmentation for 3D Object Detection

This is the official implementation of ***Ground Point Remover*** , a simple yet effective ground segmentation algorithm for 3D detection tasks. 

[<a href="https://ieeexplore.ieee.org/document/10858601">paper</a>]

<img width="2383" height="1112" alt="image" src="https://github.com/user-attachments/assets/da232b31-8645-4cbe-a289-3c7751efae13" />

## Getting Started
### Installation

a. Clone this repository
```shell
git clone https://github.com/yhc2021/GPR && cd GPR
```
b. Configure the environment

We have tested this project with the following environments:
* Ubuntu18.04/20.04
* Python >= 3.7
* PyTorch = 1.10
* CUDA = 11.3
* CMake >= 3.20


*You are encouraged to try to install higher versions above, please refer to the [official github repository](https://github.com/open-mmlab/OpenPCDet) for more information. **Note that the maximum number of parallel frames during inference might be slightly decrease due to the larger initial GPU memory footprint with updated `Pytorch` version.**

c. Install `pcdet` toolbox.
```shell
pip install -r requirements.txt
python setup.py develop
```

d. Prepare the datasets. 

Download the official KITTI with [road planes](https://drive.google.com/file/d/1d5mq0RXRnvHPVeKx6Q612z0YRO1t2wAp/view?usp=sharing) and Waymo datasets, then organize the unzipped files as follows:
```
GPR
├── data
│   ├── kitti
│   │   ├── ImageSets
│   │   ├── training
│   │   │   ├──calib & velodyne & label_2 & image_2 & (optional: planes)
│   │   ├── testing
│   │   ├── calib & velodyne & image_2
│   ├── waymo
│   │   │── ImageSets
│   │   │── raw_data
│   │   │   │── segment-xxxxxxxx.tfrecord
|   |   |   |── ...
|   |   |── waymo_processed_data_v0_5_0
│   │   │   │── segment-xxxxxxxx/
|   |   |   |── ...
│   │   │── waymo_processed_data_v0_5_0_gt_database_train_sampled_1/
│   │   │── waymo_processed_data_v0_5_0_waymo_dbinfos_train_sampled_1.pkl
│   │   │── waymo_processed_data_v0_5_0_gt_database_train_sampled_1_global.npy (optional)
│   │   │── waymo_processed_data_v0_5_0_infos_train.pkl (optional)
│   │   │── waymo_processed_data_v0_5_0_infos_val.pkl (optional)
├── pcdet
├── tools
```
Generate the data infos by running the following commands:
```python 
# KITTI dataset
python -m pcdet.datasets.kitti.kitti_dataset create_kitti_infos tools/cfgs/dataset_configs/kitti_dataset.yaml

# Waymo dataset
python -m pcdet.datasets.waymo.waymo_dataset --func create_waymo_infos \
    --cfg_file tools/cfgs/dataset_configs/waymo_dataset.yaml
```




### Training
The configuration files are in ```tools/cfgs/kitti_models/GPR.yaml``` and ```tools/cfgs/waymo_models/GPR.yaml```, and the training scripts are in ```tools/scripts```.

Train with single or multiple GPUs: (e.g., KITTI dataset)
```shell
python train.py --cfg_file cfgs/kitti_models/GPR.yaml

# or 

sh scripts/dist_train.sh ${NUM_GPUS} --cfg_file cfgs/kitti_models/GPR.yaml
```


### Evaluation

Evaluate with single or multiple GPUs: (e.g., KITTI dataset)
```shell
python test.py --cfg_file cfgs/kitti_models/GPR.yaml  --batch_size ${BATCH_SIZE} --ckpt ${PTH_FILE}

# or

sh scripts/dist_test.sh ${NUM_GPUS} \
    --cfg_file cfgs/kitti_models/GPR.yaml --batch_size ${BATCH_SIZE} --ckpt ${PTH_FILE}
```


## Acknowledgement
-  This work is built upon the `OpenPCDet` (version `0.5`), an open source toolbox for LiDAR-based 3D scene perception. Please refer to the [official github repository](https://github.com/open-mmlab/OpenPCDet) for more information.

-  Parts of our Code refer to <a href="https://github.com/yifanzhang713/IA-SSD.git">IA-SSD</a> and  <a href="https://github.com/url-kaist/patchwork-plusplus.git">Patchwork++</a>.


## TODO List

-  Release pre-trained model of GPR on KITTI dataset
-  Release pre-trained model of GPR on Waymo dataset
-  Release gpu code of GPR for Waymo dataset
-  Update the patchwork++ compilation documentation


## License

This project is released under the [Apache 2.0 license](LICENSE).



