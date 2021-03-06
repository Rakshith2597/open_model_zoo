# **Deep Neural Network for Chest X-Ray Screening**

## **Use Case and High-Level Description**

<div id="abs">

Chest radiographs are primarily employed for the screening of pulmonary and cardio-thoracic conditions. Being undertaken at primary healthcare centers, they require the presence of an on-premise reporting Radiologist, which is a challenge in low and middle income countries. This has inspired the development of machine learning based automation of the screening process. While recent efforts demonstrate a performance benchmark using an ensemble of deep convolutional neural networks (CNN), our systematic search over multiple standard CNN architectures identified single candidate CNN models whose classification performances were found to be at par with ensembles.

This code is to perform architecture search for models trained to detect multiple comorbid chest diseases in chest X-rays. In this code, we performed experiments to fine-tune the DenseNet-121[[1](#densenet)] CNN architecture for this task [[2](#embc)]. The method in this repository differs from the paper in a few aspects; In the paper, the authors classify an X-ray image into one or more of the 14 classes following a multi-hot encoding on account of co-morbidity of diseases, while in this repository we present the approach to classify a Chest X-ray image into any of the applicable 3 classes. This modified DenseNet-121 is retrained using the publicly available 2018 RSNA Pneumonia Detection Challenge dataset ([link](https://www.rsna.org/education/ai-resources-and-training/ai-image-challenge/rsna-pneumonia-detection-challenge-2018)).
</div>


Few example images from the dataset
<table >
<tr>
<td align='center'> Class 0 (Lung Opacity)</td>
<td align='center'> Class 1 (Normal)</td>
<td align='center'> Class 2 (No Lung Opacity/ Not Normal)</td>
</tr>
<tr>
<td align='center'><img src="./media/class0.jpg" width="250" height="200"></td>
<td align='center'> <img src="./media/class1.jpg" width="250" height="200"></td>
<td align='center'> <img src="./media/class2.jpg" width="250" height="200"> </td>
</tr>
</table>

A systematic search was performed over a set of CNN architectures by scaling the width and depth of the standard DenseNet using the EfficientNet approach[[3](efficientnet)]. The details of our experiments based on EfficientNet is summarized in the image below.

<img src = "./media/efficientnet.png" width=650>

## Network Architecture:

We have used a DenseNet-121 as the base architecture.

![6: A schematic illustration of the DenseNet-121 ...](https://lh6.googleusercontent.com/ziwy55LfUqzzErhcy0Cw418LbeCWpfH_liD3dXNrae8yVnV91rnCWsokLUVO0NVUuUeNHIG6bnkV3J7jNNT5U6DDr2Y78Z60NW-2ACUEuY53k6B7C6x1Q9HFrJ-1yJZNM1vyMPdg)



## **Example**

An example for using the onnx models shared in this repository is available [here](https://drive.google.com/file/d/1FCG9EaslKa_n_OF6FpcPlNy9Q3NNY4Rn/view?usp=sharing).

## **Specification and Performance**

The optimized DenseNet CNN model obtained through EfficientNet based architecture search is provided in this repository. The performance is measured using the average Area under the ROC curves (AUROC) across three classes.

| Type | Classification (Mult-task) |
| --- | --- |
| Source Framework | Pytorch (converted to ONNX) |
| GFLOPs | 6.85 |
| Mparams | 7.13 |
| Avg. AUROC | 0.5138 |


### **Input**

For the optimal DenseNet model in the ONNX format provided in this repository, the input grayscale image is replicated to three channels. Each of the 3 image channels is normalized to match the distribution of the Image-Net dataset. The order of the dimensions for the input to the ONNX model is (B, C, H, W) where B is the batch size, C is the number of channels and H, W is the spatial dimensions of the image.

The details of the image preprocessing steps are detailed in our demo code provided [here](https://drive.google.com/file/d/1FCG9EaslKa_n_OF6FpcPlNy9Q3NNY4Rn/view?usp=sharing).


## **Output**

Outputs the prediction scores in the range of [0,1] for the three disease classes. We pose the Chest radiograph screening as a multi-label classification task where multiple disease classes can co-occur in the same image. The ordering of the disease classes is listed below:

| Class |
| --- |
| Lung Opacity |
| Normal |
| No Lung Opacity/ Not Normal |

## **Acknowledgement**
This work is undertaken as part of Intel India Grand Challenge 2016 Project MIRIAD: Many Incarnations of Screening of Radiology for High Throughput Disease Screening via Multiple Instance Reinforcement Learning with Adversarial Deep Neural Networks, sponsored by Intel Technology India Pvt. Ltd., Bangalore, India.

**Contributor**

The codes/model was contributed to the OpenVINO project by

<a href="https://www.linkedin.com/in/rakshith-sathish/">Rakshith Sathish</a>, </br>
Advanced Technology Development Centre, </br>
Indian Institute of Technology Kharagpur</br>
email: rakshith.sathish@kgpian.iitkgp.ac.in</br>
Github username: Rakshith2597


**Principal Investigators**

<a href="https://www.linkedin.com/in/debdoot/">Dr Debdoot Sheet</a>, <a href="">Dr Nirmalya Ghosh</a>,</br>
Department of Electrical Engineering,</br>
Indian Institute of Technology Kharagpur</br>
email: debdoot@ee.iitkgp.ac.in, nirmalya@ee.iitkgp.ac.in

<a href="https://www.linkedin.com/in/ramanathan-sethuraman-27a12aba/">Dr Ramanathan Sethuraman</a>,</br>
Intel Technology India Pvt. Ltd.
email: ramanathan.sethuraman@intel.com

## **References**

<div id="densenet">
<a href="#abs">[1]</a> Huang, Gao, Zhuang Liu, Laurens Van Der Maaten, and Kilian Q. Weinberger. Densely connected convolutional networks. In Proceedings of the IEEE conference on computer vision and pattern recognition_, pp. 4700-4708. 2017. <a href="https://arxiv.org/pdf/1608.06993.pdf"> (link) </a> 
</div>

<div id="embc">
<a href="#abs">[2]</a> A. Mitra, A. Chakravarty, N. Ghosh, T. Sarkar, R. Sethuraman and D. Sheet, "A Systematic Search over Deep Convolutional Neural Network Architectures for Screening Chest Radiographs," 2020 42nd Annual International Conference of the IEEE Engineering in Medicine & Biology Society (EMBC), Montreal, QC, Canada, 2020, pp. 1225-1228, doi: 10.1109/EMBC44109.2020.9175246. <a href="https://ieeexplore.ieee.org/document/9175246"> (link) </a>

</div>

<div id="efficientnet">
<a href="#abs">[3]</a>  Tan, Mingxing, and Quoc V. Le. &quot;EfficientNet: Rethinking model scaling for convolutional neural networks.&quot; , ICML, pp. 6105-6114. 2019. <a href="http://proceedings.mlr.press/v97/tan19a/tan19a.pdf"> (link) </a>

</div>

<div id="chexpert">
<a href="#abs">[4]</a>  Irvin, Jeremy, Pranav Rajpurkar, Michael Ko, Yifan Yu, Silviana Ciurea-Ilcus, Chris Chute, Henrik Marklund et al. &quot;Chexpert: A large chest radiograph dataset with uncertainty labels and expert comparison.&quot; In _Proceedings of the AAAI Conference on Artificial Intelligence_, vol. 33, pp. 590-597. 2019. <a href="https://arxiv.org/abs/1901.07031"> (link) </a>
</div>

</br> 

## **Legal Information**

The model is distributed under theApache License, Version 2.0

Copyright (c) 2020 Debdoot Sheet, Ramanathan Sethuraman

Licensed under the Apache License, Version 2.0 (the &quot;License&quot;);

you may not use this file except in compliance with the License.

You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software

distributed under the License is distributed on an &quot;AS IS&quot; BASIS,

WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.

See the License for the specific language governing permissions and

limitations under the License.
