# Deep Hierarchical Semantic Segmentation

This repo contains the unoffical supported code and configuration files to reproduce semantic segmentaion results of [HieraSeg](https://arxiv.org/abs/2203.14335). It is based on [mmsegmentaion](https://github.com/open-mmlab/mmsegmentation).

## Results and Models

| Dataset | Backbone | Crop Size | mIoU (single scale) | mIoU (multi scale w/ flipping) | config |
| :---: | :---: | :---: | :---: | :---: | :---: |
| Pascal-Person-Part | ResNet101 | 480x480 | 73.44 | 75.4 | [config](configs/deeplabv3plus/deeplabv3plus_r101-d8_480x480_60k_pascal_person_part_hiera_triplet.py) |


## Usage

### Installation 

Please refer to [get_started.md](https://github.com/open-mmlab/mmsegmentation/blob/master/docs/en/get_started.md#installation) for installation and dataset preparation.

### Requirement

Pytorch >= 1.8.0 & torchvision >= 0.9.0

### Inference
```
# single-gpu testing
# python tools/test.py <CONFIG_FILE> <SEG_CHECKPOINT_FILE> --eval mIoU
python tools/test.py configs/deeplabv3plus/deeplabv3plus_r101-d8_480x480_60k_pascal_person_part_hiera_triplet.py /mnt/server14_hard0/msson/HSSN_pytorch/output_iter60k+-res101/iter_6000-74.13.pth --eval mIoU --show-dir ./visualization

# multi-gpu testing
tools/dist_test.sh <CONFIG_FILE> <SEG_CHECKPOINT_FILE> <GPU_NUM> --eval mIoU

# multi-gpu, multi-scale testing
tools/dist_test.sh <CONFIG_FILE> <SEG_CHECKPOINT_FILE> <GPU_NUM> --aug-test --eval mIoU
tools/dist_test.sh configs/deeplabv3plus/deeplabv3plus_r101-d8_480x480_60k_pascal_person_part_hiera_triplet.py /mnt/server14_hard0/msson/HSSN_pytorch/output_iter60k+-res101/iter_6000-74.13.pth 4 --aug-test --eval mIoU

```

### Training

To train with pre-trained models, run:
```
# single-gpu training
python tools/train.py <CONFIG_FILE> --options model.pretrained=<PRETRAIN_MODEL> [model.backbone.use_checkpoint=True] [other optional arguments]

# multi-gpu training
tools/dist_train.sh <CONFIG_FILE> <GPU_NUM> --options model.pretrained=<PRETRAIN_MODEL> [model.backbone.use_checkpoint=True] [other optional arguments] 
```
For example, to train on Cityscapes with a `ResNet-101` backbone and 4 gpus, run:
```
tools/dist_train.sh configs/deeplabv3plus/deeplabv3plus_r101-d8_512x1024_80k_cityscapes_hiera_triplet.py.py 4 
```


## Citing HieraSeg
```BibTeX
@article{li2022deep,
  title={Deep Hierarchical Semantic Segmentation},
  author={Li, Liulei and Zhou, Tianfei and Wang, Wenguan and Li, Jianwu and Yang, Yi},
  journal={CVPR},
  year={2022}
}
```
