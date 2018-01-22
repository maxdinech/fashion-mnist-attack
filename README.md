# Building, attacking and improving classifiers for Fashion-MNIST

The aim of this project is to show the vulnerabilities of classifier networks to adversarial examples.

![Attack result example](docs/images/attack_result_1.png)
![Attack result example](docs/images/attack_result_2.png)

## Getting Started

### 1. Loading the dataset

The Fashion-MNIST dataset is automatically downloaded and divided into three parts :

- `train.pt`, containing 50.000 samples,
- `test.pt` containing 10.000 samples,
- `val.pt` containing 10.000 samples,

### 2. Network training

The networks architectures are defined in `architectures.py`. The following architectures are available:

- AlexNet
- AlexNet_bn (AlexNet with BatchNorm)
- VGG
- VGG_bn (VGG with BatchNorm)

Note that the training parameters (`lr`, `epochs` and `batch_size`) and functions (`loss_fn` and `optimizer`) are included in the class definition of the model: it makes switching between models easier and makes it possible to use a universal training script : `train.py`.

Models are trained by running:

```sh
python train.py MODEL [bool]
```

Where `MODEL` is one of the available architectures, and `bool` decides wether the trained model is saved in `models/`.

#### Networks accuracies:

| Architecture |   acc    | test_acc | val_acc  |
| :----------- | -------- | -------- | -------- |
| VGG          |    -     |    -     |    -     |
| VGG_bn       |    -     |    -     |    -     |
| AlexNet      |    -     |    -     |    -     |
| AlexNet_bn   |    -     |    -     |    -     |


## 3. Network attacks

### Finding a minimal perturbation

Let's call `Prediction` and `Confidence` the functions that respectively gives the digit prediction of the network and the confidence prediction of the network on the real label of the image. To trick a network, we want to determine a slightly modified version of the image , `adv_image`, such that `Prediction(adv_image)` is no longer equal to `Prediction(image)`. This is usually done by using `Confidence` as a loss function: The smaller it gets, the less the networks thinks that the modified image still corresponds to its initial digit.

Attacking a network consists in determining a perturbation <img src="https://rawgit.com/maxdinech/fashion-mnist-attack (fetch/master/svgs/89f2e0d2d24bcf44db73aab8fc03252c.svg?invert_in_darkmode" align=middle width=7.873024500000003pt height=14.155350000000013pt/> such that `model.forward(image + r)` gives a wrong prediction. We want to find a minimal perturbation for a given Euclidian norm.


To approach an acceptable value of <img src="https://rawgit.com/maxdinech/fashion-mnist-attack (fetch/master/svgs/89f2e0d2d24bcf44db73aab8fc03252c.svg?invert_in_darkmode" align=middle width=7.873024500000003pt height=14.155350000000013pt/>, we minimize the following function by a gradient descent.

<p align="center"><img src="https://rawgit.com/maxdinech/fashion-mnist-attack (fetch/master/svgs/e6d8ef86207084f79e2dd4a3aae51b18.svg?invert_in_darkmode" align=middle width=426.86324999999994pt height=69.041775pt/></p>

### Instructions


## Project requirements

- Python 3
- PyTorch
- numpy
- matplotlib
- texlive, ghostscript and dvipng (for a fancy matplotlib <img src="https://rawgit.com/maxdinech/fashion-mnist-attack (fetch/master/svgs/87181ad2b235919e0785dee664166921.svg?invert_in_darkmode" align=middle width=45.697245pt height=22.46574pt/> style prining)
- tqdm
