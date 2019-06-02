# EfficientNets in Keras
Keras implementation of EfficientNets from the paper [EfficientNet: Rethinking Model Scaling for Convolutional Neural Networks](https://arxiv.org/abs/1905.11946).

Contains code to build the EfficientNets B0-B7 from the paper, and includes weights for configurations B0-B3. B4-B7 weights will be ported when made available from the Tensorflow repository.

Supports building any other configuration model of efficient nets as well, other than the B0-B7 variants.

# Efficient Nets and Compound Coefficeint Scaling 
The core idea about Efficient Nets is the use of compound scaling - using a weighted scale of three inter-connected hyper parameters of the model - Resolution of the input, Depth of the Network and Width of the Network.

<p align="center">
<img src="https://latex.codecogs.com/png.latex?\inline&space;\dpi{300}&space;\bg_white&space;\begin{align*}&space;depth:&&space;d&space;=&space;\alpha&space;^&space;\phi&space;\\&space;width:&&space;w&space;=&space;\beta&space;^&space;\phi&space;\\&space;resolution:&&space;r&space;=&space;\gamma&space;^&space;\phi&space;\end{align*}" title="\begin{align*} depth:& d = \alpha ^ \phi \\ width:& w = \beta ^ \phi \\ resolution:& r = \gamma ^ \phi \end{align*}" height=25% width=25%/>
</p>

When `phi`, the compound coefficient, is initially set to 1, we get the base configuration - in this case `EfficientNetB0`. We then use this configuration in a grid search to find the coefficients `alpha`, `beta` and `gamma` which optimize the following objective under the constraint:

<p align="center">
<img src="https://latex.codecogs.com/png.latex?\inline&space;\dpi{300}&space;\begin{align*}&space;\alpha&space;\cdot&space;\beta&space;^&space;2&space;\cdot&space;\gamma&space;^&space;2&space;&\approx&space;2&space;\\&space;\alpha&space;\ge&space;1,&space;\beta&space;\ge&space;&1,&space;\gamma&space;\ge&space;1&space;\end{align*}" title="\begin{align*} \alpha \cdot \beta ^ 2 \cdot \gamma ^ 2 &\approx 2 \\ \alpha \ge 1, \beta \ge &1, \gamma \ge 1 \end{align*}" height=25% width=25%/>
</p>

Once these coefficients for `alpha`, `beta` and `gamma` are found, then simply scale `phi`, the compound coeffieints by different amounts to get a family of models with more capacity and possibly better performance.

-----

In doing so, and using Neural Architecture Search to get the base configuration as well as great coefficients for the above, the paper generates EfficientNets, which outperform much larger and much deeper models while using less resources during both training and evaluation.

<img src="https://raw.githubusercontent.com/tensorflow/tpu/master/models/official/efficientnet/g3doc/params.png" height=100% width=49%> <img src="https://raw.githubusercontent.com/tensorflow/tpu/master/models/official/efficientnet/g3doc/flops.png" height=100% width=49%>

# Usage
Simply import `efficientnet.py` and call either the model builder `EfficientNet` or the pre-built versions `EfficientNetBX` where `X` ranger from 0 to 7.

```python
from efficientnet import EfficientNetB0

model = EfficientNetBXinput_size, classes=1000, include_top=True, weights='imagenet')
```

# Requirements
- Tensorflow 1.13+
- Keras 2.2.4+

# References
```
[1] Mingxing Tan and Quoc V. Le. EfficientNet: Rethinking Model Scaling for Convolutional Neural Networks. ICML 2019. Arxiv link: https://arxiv.org/abs/1905.11946.
```