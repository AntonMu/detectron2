## Installation

Our [Colab Notebook](https://colab.research.google.com/drive/16jcaJoc6bCFAQ96jDe2HwtXj7BMD_-m5)
has step-by-step instructions that install detectron2.
The [Dockerfile](https://github.com/facebookresearch/detectron2/blob/master/docker/Dockerfile)
also installs detectron2 with a few simple commands.

### Requirements
- Linux or macOS
- Python >= 3.6
- PyTorch 1.3
- [torchvision](https://github.com/pytorch/vision/) that matches the PyTorch installation.
	You can install them together at [pytorch.org](https://pytorch.org) to make sure of this.
- OpenCV, needed by demo and visualization
- [fvcore](https://github.com/facebookresearch/fvcore/): `pip install -U 'git+https://github.com/facebookresearch/fvcore'`
- pycocotools: `pip install cython; pip install 'git+https://github.com/cocodataset/cocoapi.git#subdirectory=PythonAPI'`
- GCC >= 4.9

### Individual Steps
1. Fork repo to your local github account and clone it to your machine:
	```
	git clone git@github.com:<username>/detectron2.git
	```
2. Go into the `detectron2` directory and create and activate a virtual environment:
	```
	cd detectron2
	python3 -m venv env
	source env/bin/activate
	pip install --upgrade pip
	```
3. Find the pytorch installation that matches your system at [pytorch.org/get-started/locally/](https://pytorch.org/get-started/locally/). For instance, if you use Linux with python3.6.9, pip and CUDA 9.2 run:
	```
	pip3 install torch==1.3.1+cu92 torchvision==0.4.2+cu92 -f https://download.pytorch.org/whl/torch_stable.html
	```

	```
	pip3 install torch torchvision
	```

	If you used CUDA, you can [verify if your GPU is recognized](https://stackoverflow.com/questions/48152674/how-to-check-if-pytorch-is-using-the-gpu/53374933) by running:
	```
	python
	import torch
	torch.cuda.is_available() 
	torch.version.cuda
	```

	For Cuda 10.0 try:
	```
	pip3 install torch==1.3.1+cu100 torchvision==0.4.2+cu100 -f https://download.pytorch.org/whl/torch_stable.html
	```

3. Install the remaining requirements by running:
	```
	pip install opencv-python
	pip install -U 'git+https://github.com/facebookresearch/fvcore'
	pip install cython; pip install 'git+https://github.com/cocodataset/cocoapi.git#subdirectory=PythonAPI'
	```

### Build Detectron2

After having the above dependencies, run:
```
python setup.py build develop
```

If you are on macOS
```
MACOSX_DEPLOYMENT_TARGET=10.9 CC=clang CXX=clang++ python setup.py build develop
```

Note: you may need to rebuild detectron2 after reinstalling a different build of PyTorch.

### Common Installation Issues

+ Undefined torch/aten symbols, or segmentation fault immediately when running the library.
  This may be caused by the following reasons:

	* detectron2 or torchvision is not compiled with the version of PyTorch you're running.

		If you use a pre-built torchvision, uninstall torchvision & pytorch, and reinstall them
		following [pytorch.org](http://pytorch.org).
		If you manually build detectron2 or torchvision, remove the files you built (`build/`, `**/*.so`)
		and rebuild them.

	* detectron2 or torchvision is not compiled using gcc >= 4.9.

	  You'll see a warning message during compilation in this case. Please remove the files you build,
		and rebuild them.
		Technically, you need the identical compiler that's used to build pytorch to guarantee
		compatibility. But in practice, gcc >= 4.9 should work OK.

+ Undefined cuda symbols. The version of NVCC you use to build detectron2 or torchvision does
	not match the version of cuda you are running with.
	This happens sometimes when using anaconda.

+ "Not compiled with GPU support": make sure
	```
	python -c 'import torch; from torch.utils.cpp_extension import CUDA_HOME; print(torch.cuda.is_available(), CUDA_HOME)'
	```
	print valid outputs at the time you build detectron2.

+ "invalid device function" or "no kernel image is available for execution": two possibilities:
  * You build detectron2 with one version of CUDA but run it with a different version.
  * Detectron2 is not built with the correct compute compability for the GPU model.
    The compute compability defaults to match the GPU found on the machine during building,
    and can be controlled by `TORCH_CUDA_ARCH_LIST` environment variable during installation.

+ `libSM.so.6` not found
	```
	sudo apt-get update
	sudo apt-get install -y libsm6 libxext6 libxrender-dev
	pip install opencv-python
	```