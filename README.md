# Web Texture Nets
reference https://github.com/DmitryUlyanov/texture_nets  https://github.com/nagadomi/waifu2x

# Prerequisites
Torch7 + loadcaffe 
NVIDIA CUDA + LuaRocks packages: turbo
cudnn + torch.cudnn (optionally)
display (optionally)

Download VGG-19.

cd data/pretrained && bash download_models.sh && cd ../..

# process
## train
Preparing image dataset

You can use an image dataset of any kind. For my experiments I tried Imagenet and MS COCO datasets. The structure of the folders should be the following:

dataset/train
dataset/train/dummy
dataset/val/
dataset/val/dummy
The dummy folders should contain images. The dataloader is based on one used infb.resnet.torch.

Here is a quick example for MSCOCO:

wget http://msvocds.blob.core.windows.net/coco2014/train2014.zip
wget http://msvocds.blob.core.windows.net/coco2014/val2014.zip
unzip train2014.zip
unzip val2014.zip
mkdir -p dataset/train
mkdir -p dataset/val
ln -s `pwd`/val2014 dataset/val/dummy
ln -s `pwd`/train2014 dataset/train/dummy

th train.lua -data <path to any image dataset> -style_image path/to/img.jpg -style_size 600 -image_size 512 -model johnson -batch_size 4 -learning_rate 1e-2 -style_weight 10 -style_layers relu1_2,relu2_2,relu3_2,relu4_2 -content_layers relu4_2

## test
th test.lua -input_image path/to/image.jpg -model_t7 data/checkpoints/model.t7

# Web Application
th stylize.lua
