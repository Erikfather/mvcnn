# Multi-view CNN (MVCNN) for shape recognition

The goal of the project is to learn a general purpose descriptor for shape recognition. To do this we train discriminative models for shape recognition using convolutional neural networks (CNNs) where view-based shape representations are the only cues. Examples include line-drawings, clip art images where color is removed, or **renderings of 3D models** where there is little or no texture information present. 

If you use any part of the code from this project, please cite:

Multi-view Convolutional Neural Networks for 3D Shape Recognition
Hang Su, Subhransu Maji, Evangelos Kalogerakis, Erik Learned-Miller
Proceedings of the IEEE International Conference on Computer Vision 2015 (ICCV 2015)

## Installation

* install dependencies
``` 
#!bash
git submodule init
git submodule update
```

* compile

(1) compile for CPU
``` 
#!bash
MEX=<MATLAB_ROOT>/bin/mex matlab -nodisplay -r "setup(true);exit;"
```
(2) compile for GPU: 
``` 
#!bash
MEX=<MATLAB_ROOT>/bin/mex matlab -nodisplay -r "setup(true,struct('enableGpu',true));exit;"
```
(3) compile with cuDNN support: 
``` 
#!bash
MEX=<MATLAB_ROOT>/bin/mex matlab -nodisplay -r "setup(true,struct('enableGpu',true,
'cudaRoot',<CUDA_ROOT>,'cudaMethod','nvcc','enableCudnn',true,'cudnnRoot',<CUDNN_ROOT>));exit;"
```
(note) You can alternatively run directly the scripts from the Matlab command window e.g. for Windows installations:
setup(true,struct('enableGpu',true,'cudaRoot','C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v7.0','cudaMethod','nvcc'));
You may also need to add Visual Studio's cl.exe in your PATH environment (e.g., C:\Program Files (x86)\Microsoft Visual Studio 12.0\VC\bin\amd64)

## Usage

* extract descriptor for a shape (off/obj mesh) - the descriptor will be saved in a txt file (bunny_descriptor.txt) [assumes upright orientation]

```
shape_compute_descriptor('bunny.off');
```

* extract descriptor for all shapes in a folder (off/obj meshes),  the descriptors will be saved in txt files in the same folder [assumes upright orientation]

```
shape_compute_descriptor('my_mesh_folder/');
```

* extract descriptor for all shapes in a folder (off/obj meshes), post-process descriptor with learned metric, and use the model that *does not assume* upright orientation [*-v2 models do not assume upright orientations]

```
shape_compute_descriptor('my_mesh_folder/', 'cnn_model', 'cnn-modelnet40-v2.mat', 'metric_model', 'metric-relu7-v2.mat','post_process_desriptor_metric',true);
```

* download datasets for training/evaluation

```
#!bash
#ModelNet40 (v1 - 12 views w/ upright assumption) (4.8G)
cd data
wget http://maxwell.cs.umass.edu/deep-shape-data/modelnet40-v1.tar
tar xf modelnet40-v1.tar

#ModelNet40 (v2 - 80 views w/o upright assumption) (7.2G)
cd data
wget http://maxwell.cs.umass.edu/deep-shape-data/modelnet40-v2.tar
tar xf modelnet40-v2.tar

#sketch (211M)
cd data
wget http://pegasus.cs.umass.edu/deep-shape-data/sketch160.tar
tar xf sketch160.tar

#clipart (701M)
cd data
wget http://pegasus.cs.umass.edu/deep-shape-data/clipart100.tar
tar xf clipart100.tar
```
* run experiments in the paper (see run_experiments.m for options and other details)
```
#!bath
LD_LIBRARY_PATH=<CUDA_ROOT>/lib64:<CUDNN_ROOT> matlab -nodisplay -r "run_experiments;exit;"
```
*LD_LIBRARY_PATH* may not be necessary depending on your installation, e.g. whether includes cuDNN support. 
*LD_LIBRARY_PATH* may not be necessary depending on your installation, e.g. whether includes cuDNN support. 