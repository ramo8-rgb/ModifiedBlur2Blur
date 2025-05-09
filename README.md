# Official Pytorch Implementation of "ModifiedBlur2Blur: Blur Conversion for Unsupervised Image Deblurring on Unknown Domains" [(CVPR'24)](https://cvpr.thecvf.com/)

[![arXiv](https://img.shields.io/badge/arXiv-2403.16205-red?logo=arxiv&logoColor=red)](https://arxiv.org/abs/2403.16205)
[![homepage](https://img.shields.io/badge/project-Homepage-blue?logo=Homepage&logoColor=blue)](https://zero1778.github.io/blur2blur/)
[![youtube](https://img.shields.io/badge/video-Youtube-white?logo=youtube&logoColor=red)](https://www.youtube.com/watch?v=CMjxtElp9g4)


<div align="center">
  <a href="https://zero1778.github.io" target="_blank">Bang-Dang&nbsp;Pham</a> &emsp; <b>&middot;</b> &emsp;
  <a href="https://p0lyfish.github.io/portfolio/" target="_blank">Phong&nbsp;Tran</a> &emsp; 
  <b>&middot;</b> &emsp;
  <a href="https://scholar.google.com/citations?user=FYZ5ODQAAAAJ&hl=en" target="_blank">Anh&nbsp;Tran</a> &emsp; 
  <b>&middot;</b> &emsp;
  <a href="https://sites.google.com/view/cuongpham/home" target="_blank">Cuong&nbsp;Pham</a> &emsp; 
  <b>&middot;</b> &emsp;
  <a href="https://rangnguyen.github.io/" target="_blank">Rang&nbsp;Nguyen</a> &emsp; 
  <b>&middot;</b> &emsp;
  <a href="https://gshoai.github.io" target="_blank">Minh&nbsp;Hoai</a> &emsp; 
  <br> <br>
  <a href="https://www.vinai.io/">VinAI Research, Vietnam</a>
</div>

<br>
<div align="center">
    <img width="1100" alt="teaser" src="assets/teaser.png"/>
</div>

<b>TL;DR</b>: ModifiedBlur2Blur converts images from unknown blur into a known blur. This version retains the original content while applying a different blur kernel that has been effectively trained and captured by supervision deblurring models.


> **Abstract**: This paper presents Modified Blur2Blur, an innovative framework for training camera-specific deblurring algorithms. Our key insight is to transform challenging blur patterns into more tractable ones before deblurring. Using unpaired blurry ($\mathcal{B}$) and sharp ($\mathcal{S}$) images from a target camera, we learn a mapping $G$ from the camera's unknown blur domain $C$ to a known domain $C'$. A critical modification replaces traditional LeakyReLU activations with SiLU (Swish) in both generator and discriminator networks, improving gradient flow and detail preservation. Experiments across multiple benchmarks show our method outperforms state-of-the-art approaches by up to 2.91 dB PSNR when combined with pre-trained models. The framework is practical for real-world applications, requiring only simple data collection from the target camera.Image deblurring has been extensively studied through both classical non-learning methods and modern machine learning approaches. Contemporary learning-based techniques can be classified according to their training data requirements: paired, synthetic, or unpaired data. We review significant contributions from each category below.



Details of the model architecture and experimental results can be found in [our paper](https://arxiv.org/abs/2403.16205):

```bibtex
@inproceedings{pham2024blur2blur,
 author={Pham, Bang-Dang and Tran, Phong and Tran, Anh and Pham, Cuong and Nguyen, Rang and Hoai, Minh},
 booktitle={Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)},
 title={ModifiedBlur2Blur: Blur Conversion for Unsupervised Image Deblurring on Unknown Domains},
 year={2024}
}
```
**Please CITE** our paper whenever this repository is used to help produce published results or incorporated into other software.



## Table of contents
1. [Getting Started](#getting-started)
2. [Datasets](#datasets-floppy_disk)
3. [ModifiedBlur2Blur](#blur2blur-rocket)
4. [Results](#results-trophy)
5. [Acknowledgments](#acknowledgments)
6. [Contacts](#contacts-mailbox_with_mail)

## Getting Started :sparkles:

### Prerequisites
- Python >= 3.7
- Pytorch >= 1.9.0
- CUDA >= 10.0


### Installation
Install dependencies:
```shell
git clone https://github.com/VinAIResearch/ModifiedBlur2Blur
cd ModifiedBlur2Blur

conda create -n blur2blur python=3.9  
conda activate blur2blur  
pip install -r requirements.txt  
```

## Datasets :floppy_disk:

### Data Preperation
You can download our proposed RB2V dataset by following this script:
```
chmod +x ./dataset/download_RB2V.sh
bash ./dataset/download_RB2V.sh
``` 
Download datasets [REDS](https://seungjunnah.github.io/Datasets/reds.html), [GoPro](https://seungjunnah.github.io/Datasets/gopro.html) and [RSBlur](https://cg.postech.ac.kr/research/rsblur/) then unzip to folder `./dataset` and organize following this format:
<pre>
dataset
├── Name of Unknown-Known dataset e.g. RB2V-GoPro
    ├── trainA
    ├──── <i>(Train) Blurry set of Unknown Blur</i>
    ├──── ...
    ├── trainB
    ├──── <i>(Train) Sharp set of Unknown Blur</i>
    ├──── ...
    ├── trainC
    ├──── <i>(Train) Blurry set of Known Blur</i>
    ├──── ...
    ├── trainD
    ├──── <i>(Train) Sharp set of Known Blur</i>
    ├──── ...
    ├── testA
    ├──── <i>(Test) Blurry set of Unknown Blur</i>
    ├──── ...

</pre>
where:
* <b>trainA, trainB</b>: Blur/Sharp images from the Unknown Blur dataset for blur kernel conversion.
* <b>trainC, trainD</b>: Blur/Sharp images from the Known Blur dataset for the target blur domain.
* <b>testA</b>: Blurry images from the `test-set` of the Unknown Blur dataset.

## ModifiedBlur2Blur :rocket:

### Training
To train the model:
```.bash
python train.py --dataroot path/to/dataset \
                --name exp_name \
                --model blur2blur --netG mimounet \
                --batch_size 1 \
                --dataset_mode unaligned \
                --norm instance --pool_size 0 \
                --display_id -1
```
or 
```.bash
bash ./scripts/train.sh
```

### Using ModifiedBlur2Blur
To evaluate the model:
```.bash
python test.py --dataroot datasets/GoPro/b2b_exp/RB2V_GOPRO_filter \
                --name exp_name \
                --eval \
                --model blur2blur --netG mimounet \
                --checkpoints_dir ckpts/ \
                --dataset_mode unaligned \
                --norm instance \
```
or 
```.bash
bash ./scripts/test.sh
```


## Results :trophy:
For more interactive results, you can take a look at my project page: https://zero1778.github.io/blur2blur/

## Acknowledgments
We would like to extend our gratitude to the following implementations for their contributions to the development of ModifiedBlur2Blur:

- [pix2pix](https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix) for providing the foundational code model.
- [uvcgan2](https://github.com/LS4GAN/uvcgan2) for helping to enhance the baseline code.
- [MIMO-UNet](https://github.com/chosj95/MIMO-UNet) and [Blur Kernel Extractor](https://github.com/VinAIResearch/blur-kernel-space-exploring/tree/main) for creating the backbone and submodules integral to our project.

## Contacts :mailbox_with_mail:
If you have any questions or suggestions about this repo, please feel free to contact me (bangdang2000@gmail.com).
