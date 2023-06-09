# GPU on remote servers
* [Tutorial: CUDA, cuDNN, Anaconda, Jupyter, PyTorch Installation in Windows 10](https://sh-tsang.medium.com/tutorial-cuda-cudnn-anaconda-jupyter-pytorch-installation-in-windows-10-96b2a2f0ac57)
* [Install TensorFlow with pip](https://www.tensorflow.org/install/pip#windows-wsl2)
* https://www.reddit.com/r/tensorflow/comments/jsalkw/rtx_3090_and_tensorflow_for_windows_10_step_by/
* [Installing TensorFlow 2 GPU [Step-by-Step Guide]](https://neptune.ai/blog/installing-tensorflow-2-gpu-guide)
```
import torch
torch.cuda.is_available()

import tensorflow as tf
tf.config.list_physical_devices('GPU')
```

Make sure to load cuda toolkit, cudnn modules. Check the installed versions of your environment, both local and anaconda. Check the versions in your gpu `nvidia-smi`. You pytorch supported version and cudatoolkit version needs to match. Also reboot the pc after reinstall.

https://stackoverflow.com/questions/9727688/how-to-get-the-cuda-version
https://discuss.pytorch.org/t/pytorch-cannot-find-gpu-2021-version/135701/2
https://stackoverflow.com/questions/42326748/tensorflow-on-gpu-no-known-devices-despite-cudas-devicequery-returning-a-pas
```
# the current version of cudatoolkit is 11.7, but torch supports upto 11.3
# also conda-forge has the latest version of the libraries.
conda install -c conda-forge cudatoolkit=11.3 cudnn=8.2.1

# install pytorch using pip: https://pytorch.org/get-started/locally
# force reinstall using --force-reinstall if you want a clean install
# installing with conda might not install the cuda library for pytorch, 
# hence torch will be unable to detech the GPU. https://stackoverflow.com/questions/72445866/gpu-available-in-tensorflow-but-not-in-torch
pip install torch==1.11.0+cu113 -f https://download.pytorch.org/whl/torch_stable.html

# or optionally the torch libraies for vision and audio
pip install torch==1.11.0+cu113 torchvision==0.12.0+cu113 torchaudio==0.11.0+cu113 -f https://download.pytorch.org/whl/torch_stable.html


# if you already have cpu version installed, remove it and also protobuf
# don't install tensorflow with conda, rather use pip, since conda will install old tf

pip uninstall protobuf
pip install --upgrade --force-reinstall tensorflow

# on windows set cuda visible if necessary
# for multiple GPUS, set multiple numbers
set CUDA_VISIBLE_DEVICES=1
```

* [NotImplementedError: Cannot convert a symbolic Tensor (2nd_target:0) to a numpy array](https://stackoverflow.com/questions/58479556/notimplementederror-cannot-convert-a-symbolic-tensor-2nd-target0-to-a-numpy)