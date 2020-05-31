# project-NII-pytorch-scripts
By Xin Wang, National Institute of Informatics, 2020

I am a new pytorch user. If you have any suggestions or questions on this repostiory, pleas email wangxin at nii dot ac dot jp

---


### General
This repository contains pytorch codes for a few projects:

1. [Cyclic-noise neural source-filter waveform model (NSF)](https://nii-yamagishilab.github.io/samples-nsf/nsf-v4.html)

2. [Harmonic-plus-noise NSF with trainable sinc filter](https://nii-yamagishilab.github.io/samples-nsf/nsf-v3.html) 

3. [Harmonic-plus-noise NSF with fixed FIR filter](https://nii-yamagishilab.github.io/samples-nsf/nsf-v2.html) 

This is the re-implemetation of projects based on [CURRENNT](https://github.com/nii-yamagishilab/project-CURRENNT-public). All the papers published so far used CURRENNT implementation. 

### Requirements
1. python 3 (test on python3.8) 
2. Pytorch (test on pytorch-1.4)
3. numpy (test on  1.18.1)
4. scipy (test on 1.4.1)

I use miniconda to manage python environment. If necessary, you may use [env.yml](./env.yml) to create the environment on our server by *conda env create -f env.yml*

### Usage
Take cyc-noise-nsf as an example:

```
# add $PWD to PYTHONPATH 
# you may also activate conda environment
$: bash env.sh 

# cd into the project and run the script
$: cd project/cyc-noise-nsf-4
$: bash 00_demo.sh
``` 

The above steps will download the CMU-arctic data, and run waveform generation using a pre-trained model, and train a new model (which may take 1 day or more).


### Explanation

Directory | Function
------------ | -------------
./core_scripts | scripts to manage the training process, data io, and so on
./sandbox | directories to test new functions and models
./project | project directories, and each folder correspond to one model for one dataset
./project/*/main.py | script to load data and run training and inference
./project/*/model.py | model definition based on Pytorch APIs
./project/*/config.py | configurations for training/val/test set data

The motivation is to separate the training and inference process, the model definition, and the data configuration. For example:

* To define a new model, change model.py only

* To run on a new database, change config.py only

---
### Differences from CURRENNT implementation
There may be more, but here are the important ones:

1. "Batch-normalization": in CURRENNT, "batch-normalization" is conducted along the length sequence, i.e., assuming each frame as one sample. There is no equivalent implementation on this Pytorch repository;

2. No bias in CNN and FF: due to the 1st point, NSF in this repostory uses bias=false for CNN and feedforward layers in neural filter blocks, which can be helpful to make the hidden signals around 0;

3. STFT framing/padding: in CURRENNT, the first frame starts from the 1st step of a signal; in this Pytorch repository (as Librosa), the first frame is centered around the 1st step of a signal, and the frame is padded with 0;

4. (minor one) STFT backward: in CURRENNT, STFT backward follows the steps in [this paper](https://ieeexplore.ieee.org/document/8915761/); in Pytorch respository, backward over STFT is done by the Pytorch library. 

### Reference

1. Xin Wang and Junichi Yamagishi. 2019. Neural Harmonic-plus-Noise Waveform Model with Trainable Maximum Voice Frequency for Text-to-Speech Synthesis. In Proc. SSW, pages 1–6, ISCA, September. ISCA. http://www.isca-speech.org/archive/SSW_2019/abstracts/SSW10_O_1-1.html

2. Xin Wang, Shinji Takaki, and Junichi Yamagishi. 2020. Neural source-filter waveform models for statistical parametric speech synthesis. IEEE/ACM Transactions on Audio, Speech, and Language Processing, 28:402–415. https://ieeexplore.ieee.org/document/8915761/


