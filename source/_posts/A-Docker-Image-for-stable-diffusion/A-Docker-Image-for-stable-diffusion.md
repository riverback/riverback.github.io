---
title: a easy stable diffusion script
date: 2022-09-02 23:49:47
tags:
---

## Usage

Here is the pipeline to create your own stable-diffusion image.

make a folder for your generated images, then create a python script like this:

```python
"""
Args you need to specify:
    cuda_idx
    image info

"""
import os
import pynvml as nv
from getpass import getuser

### set gpu idx
cuda_idx = 6


### set image info
"""
Args:
    prompt: str, A description of required images
    height: int, height must be divided by 8
    width: int, width must be divided by 8
    guidance_scale: int, higher guidance_scale may lead to more closely matched images, but at the expense of image quality 
    seed: int
    num_images: int, generate `num_images` images for the same prompt
"""
prompt = ""
height = 512
width = 512
guidance_scale = 512
seed = 123456789
num_images = 10
img_size = height * width # img_size should be samller than 1280*640 pixles if you are using RTX3090


### check gpu status
GPU_MEMORY_512x512 = 11245 # 11245M
os.environ['CUDA_VISIBLE_DEVICES'] = str(cuda_idx)

nv.nvmlInit()
handle = nv.nvmlDeviceGetHandleByIndex(cuda_idx)
info = nv.nvmlDeviceGetMemoryInfo(handle)
gpu_total = info.total / 1024**2
gpu_free = info.free / 1024**2
gpu_used = info.used / 1024**2


if img_size / 512**2 * GPU_MEMORY_512x512 > gpu_free or img_size > 1208*640:
    print("gpu memory total: {:d}M".format(int(gpu_total)))
    print("gpu memory free: {:d}M".format(int(gpu_free)))
    print("gpu memory used: {:d}M".format(int(gpu_used)))
    raise ValueError("the free gpu memory cannot handle the required image size")



### run stable-diffusion docker application
image_name = 'zhuang_pku/sd:latest'
container_name = '{}-stable_diffusion'.format(getuser())
pwd = os.getcwd()
image_folder = os.path.join(pwd, 'images')
if not os.path.exists(image_folder):
    os.makedirs(image_folder)

command_line = f'docker run -ti --gpus="{cuda_idx}" \
-v  {image_folder}:/mydir/images \
--name {container_name} {image_name} \
python3 /mydir/generate.py \
--prompt {prompt} \
--height {height} \
--width {width} \
--guidance_scale {guidance_scale} \
--seed {seed} \
--num_images {num_images}'

print(command_line)

os.system(command_line)
os.system(f"docker rm {container_name}")

```

or you can simply git clone the prepared code:

```bash
git clone git@github.com:riverback/easy-use-stable-diffusion.git
```

modify the arguments and generate your images freely!

## Requirements

1. Nvidia Docker

   test with Nvidia Docker 2.11.0.

   please refer to https://github.com/NVIDIA/nvidia-docker to install.

2. Python package

   ```
   pip install pynvml
   ```

3. GPU Memory

   a 512*512 images need a GPU memory larger than 10G, a RTX3090 can not generate a image larger than 1280×640 pixels.
