# GPU Setup
* [Tutorial: CUDA, cuDNN, Anaconda, Jupyter, PyTorch Installation in Windows 10](https://sh-tsang.medium.com/tutorial-cuda-cudnn-anaconda-jupyter-pytorch-installation-in-windows-10-96b2a2f0ac57)
* [Install TensorFlow with pip](https://www.tensorflow.org/install/pip#windows-wsl2)
* https://www.reddit.com/r/tensorflow/comments/jsalkw/rtx_3090_and_tensorflow_for_windows_10_step_by/
* [Installing TensorFlow 2 GPU [Step-by-Step Guide]](https://neptune.ai/blog/installing-tensorflow-2-gpu-guide)


```{warning}
This tutorial is specifically for `Nvidia GPU`. Not sure if the codes will run for other GPUs. 
```

Check your gpu version running `nvidia-smi` from shell. Install the compatible version of libraries. Your pytorch supported version and cudatoolkit version need to match. More on how to get the cuda version is [here](https://stackoverflow.com/questions/9727688/how-to-get-the-cuda-version).

Also reboot the pc after reinstall if needed. For this tutorial, I am using the current latest version.

## PyTorch

Check if torch and cuda is already installed.

```{code-block} python
import torch
torch.cuda.is_available()
```

If False, install using the following ([source](https://pytorch.org/get-started/previous-versions/#v1131)),

```
# using anaconda
conda install pytorch==1.13.1 pytorch-cuda=11.7 -c pytorch -c nvidia
# if you want to install additional libraries
conda install pytorch==1.13.1 torchvision==0.14.1 torchaudio==0.13.1 pytorch-cuda=11.7 -c pytorch -c nvidia

# or using pip
pip install torch==1.13.1+cu117 --extra-index-url https://download.pytorch.org/whl/cu117

# if you want to install additional libraries
pip install torch==1.13.1+cu117 torchvision==0.14.1+cu117 torchaudio==0.13.1 -f https://download.pytorch.org/whl/torch_stable.html
```

This will install PyTorch 1.13.1 and cuda 11.7. Recheck if it is properly installed. Related [issue](https://discuss.pytorch.org/t/pytorch-cannot-find-gpu-2021-version/135701/2).
1. 
2. 

## TensorFlow

Check if it is already installed.

```{code-block} python
import tensorflow as tf
tf.config.list_physical_devices('GPU')
```

If 0, install the library using the following code ([source](https://www.tensorflow.org/install/pip)). Do not install TensorFlow with conda. It may not have the latest stable version. pip is recommended since TensorFlow is only officially released to PyPI. Earlier versions used to come with different library for gpu (`tensorflow-gpu`), release updated of those have been discontinued.

For windows, Tensorflow 2.10.0 is the last version installable by windows native. Later versions have to be installed by wsl. On linus, there is no major change. The following is for windows.

```
pip install tensorflow==2.10.*

conda install -c conda-forge cudnn=8.1.0
set CUDA_VISIBLE_DEVICES=1
```

This installs both cpu and gpu versions. Verify it has been installed. Installing cudnn didn't work with pip.

```
python -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"
```

If this returns an empty list, check the following [issue](https://stackoverflow.com/questions/42326748/tensorflow-on-gpu-no-known-devices-despite-cudas-devicequery-returning-a-pas).

```
pip uninstall protobuf
pip install --upgrade --force-reinstall tensorflow

# on windows set cuda visible if necessary
# for multiple GPUS, set multiple numbers
set CUDA_VISIBLE_DEVICES=1
```