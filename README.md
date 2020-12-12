# glSpecializeShader segfaults on merge compute shader

```
$ nvidia-smi
Sat Dec 12 14:35:11 2020       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 455.46.02    Driver Version: 455.46.02    CUDA Version: 11.1     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  GeForce RTX 2060    Off  | 00000000:01:00.0  On |                  N/A |
| 32%   36C    P8    21W / 160W |    309MiB /  5931MiB |     23%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|    0   N/A  N/A    453329      G   /usr/lib/xorg/Xorg                231MiB |
|    0   N/A  N/A    454083      G   ...rojects/mgpu-shaders/test        2MiB |
|    0   N/A  N/A    454285      G   ...AAAAAAAA== --shared-files       52MiB |
+-----------------------------------------------------------------------------+

$ clang++ segfault.cxx -lGL -lgl3w -lglfw -g -o segfault
$ ./segfault
Loading shader _ZN4mgpu16kernel_partitionILj0ENS_14merge_params_tINS_10iterator_tINS_17readonly_access_tIfLi0EEEfEENS_16empty_iterator_tENS2_INS3_IfLi1EEEfEES6_NS2_INS_18writeonly_access_tIfLi2EEERfEES6_St4lessIiEEELi3ELi0EEEvv from test.spv
Segmentation fault (core dumped)
```

Driver segfaults in `glSpecializeShader` for merge partition compute shader. The shader binary passes validation.

I'm porting moderngpu algorithms over, and this is the simplest one. [test.cxx](https://github.com/seanbaxter/mgpu-shaders/blob/master/test.cxx) is the entry point that kicks of all these template specializations and generates the shader binary.