---
title: A-Docker-Image-for-stable-diffusion
date: 2022-09-02 23:49:47
tags:
---

# A Docker Image for stable diffusion

Here is the pipeline to create your own stable-diffusion image.

```bash
docker pull huggingface/transformers
```



```dockerfile
FROM huggingface/transformers

RUN pip install diffusers


```

