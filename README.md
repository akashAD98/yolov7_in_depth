# yolov7_in_depth

Architect of yolov7



<img width="359" alt="image" src="https://user-images.githubusercontent.com/62583018/195321728-6a1eda77-d719-4e9e-b142-d4239e80964c.png">


<img width="898" alt="image" src="https://user-images.githubusercontent.com/62583018/195313380-2a61c91e-8a7d-4276-98c2-6582fd417ac4.png">

<img width="638" alt="image" src="https://user-images.githubusercontent.com/62583018/195313857-9f2b2c73-ad0d-44c3-a278-51d6ee4d6a64.png">


Let's take a look at YOLOV7 as a whole. First, resize the input image to a size of 640x640, input it into the backbone network, and then output three layers of feature maps of different sizes through the head layer network, and output the prediction result through Rep and conv. Here, coco As an example, the output is 80 categories, and then each output (x, y, w, h, o) is the coordinate position and the background before and after, 3 refers to the number of anchors, so the output of each layer is (80+5) x3 = 255 multiplied by the size of the feature map is the final output


## Backbone

<img width="692" alt="image" src="https://user-images.githubusercontent.com/62583018/195325569-8ee8b946-dc12-47d2-a80d-e66674581d1e.png">




There are 50 layers in total, and I marked the key layers with black numbers in the picture above.
The first is to go through 4 layers of convolution layers, as shown in the figure below, CBS is mainly composed of Conv + BN + SiLU , I use different colors to represent different sizes and strides in the figure, such as (3, 2) means that the size of the convolution kernel is 3 , with a step size of 2. The configuration in config is shown in the figure.

<img width="413" alt="image" src="https://user-images.githubusercontent.com/62583018/195329013-dd48c3f5-832a-49d5-a4e1-1e49a1ac0b0a.png">

After 4 CBS, the feature map becomes 160*160*128 size. Afterwards, the ELAN module proposed in the paper will go through. The ELAN is composed of multiple CBSs, and the input and output feature sizes remain unchanged. The number of channels will change in the first two CBSs, and the following input channels are kept with the output channels. Consistently, after the last CBS output is the desired channel.

# ELAN1 Block

<img width="587" alt="image" src="https://user-images.githubusercontent.com/62583018/195320715-06a39b58-d854-492c-9837-ff6959072d4e.png">

<img width="581" alt="image" src="https://user-images.githubusercontent.com/62583018/195321418-96cbb17f-b93e-4b70-8807-7c0a58af2378.png">




<img width="367" alt="image" src="https://user-images.githubusercontent.com/62583018/195331166-8f319451-3c2c-4676-a825-39011426f8b2.png">

<img width="332" alt="image" src="https://user-images.githubusercontent.com/62583018/195331246-d30c26a8-e56f-429e-a56b-cb2ac20e6e47.png">

## the mp layer

<img width="369" alt="image" src="https://user-images.githubusercontent.com/62583018/195331374-55397688-8a2a-4141-96dd-cdaa12432cd2.png">


## head 

<img width="378" alt="image" src="https://user-images.githubusercontent.com/62583018/195331496-597fcca5-e93d-4199-a75c-f7254079a0f5.png">

The YOLOV7 head is actually a pafpn structure, the same as the previous YOLOV4 and YOLOV5. First, for the 32-fold down-sampled feature map C5 finally output by the backbone, and then through SPPCSP, the number of channels is changed from 1024 to 512. First, fuse with C4 and C3 according to top down to get P3, P4 and P5; then press bottom-up to fuse with P4 and P5


<img width="314" alt="image" src="https://user-images.githubusercontent.com/62583018/195331688-512fa518-f070-4b96-9fa3-0a805c98f0bd.png">

<img width="239" alt="image" src="https://user-images.githubusercontent.com/62583018/195331773-60b02853-e2d4-4805-86cf-e9177ec10a88.png">


It is basically the same as YOLOV5 here , the difference is that the CSP module in YOLOV5 is replaced by the ELAN-H module, and the downsampling is changed to the MP2 layer. The ELAN-H module is named by myself, and it is slightly different from the ELAN in the backbone that the number of cats is different.

<img width="439" alt="image" src="https://user-images.githubusercontent.com/62583018/195332160-412dd0e6-21d5-452c-8927-e8f38cba4918.png">

<img width="432" alt="image" src="https://user-images.githubusercontent.com/62583018/195332219-48f5b27a-c4d2-4f4c-bc08-e7295136bf95.png">




### CFg 

# Elan2 

<img width="650" alt="image" src="https://user-images.githubusercontent.com/62583018/195322202-2c97bc12-e7fe-4edf-b579-a21b6ba664c1.png">


<img width="542" alt="image" src="https://user-images.githubusercontent.com/62583018/195322307-b7b30706-714d-4786-a0a1-09ccc0cf5159.png">


MP CONV

<img width="593" alt="image" src="https://user-images.githubusercontent.com/62583018/195322365-8984bb4c-7368-41c9-9def-a4accfadc2d0.png">

<img width="661" alt="image" src="https://user-images.githubusercontent.com/62583018/195322514-fa0bb971-92c9-4445-a7ba-b0dc1ee2a9e3.png">


<img width="674" alt="image" src="https://user-images.githubusercontent.com/62583018/195322562-0a87e485-99f3-4bde-a8b6-bf7795a7f14d.png">


# sppcspc
<img width="669" alt="image" src="https://user-images.githubusercontent.com/62583018/195322651-c9c39732-a53b-4ee4-b55d-248580e8ddab.png">

common.pyCorresponding part in:

<img width="655" alt="image" src="https://user-images.githubusercontent.com/62583018/195322869-5a5a6d50-a5db-4084-b8ac-c71dac8a5407.png">



## RepConv

<img width="625" alt="image" src="https://user-images.githubusercontent.com/62583018/195323264-52e5ca84-d9d9-4351-a881-20f6801b8ea6.png">


<img width="666" alt="image" src="https://user-images.githubusercontent.com/62583018/195323356-4a32f8c5-ce03-4a33-b16c-002cbf94a82c.png">

## ImpConv


<img width="658" alt="image" src="https://user-images.githubusercontent.com/62583018/195323604-4a0c527e-559e-4679-aa8f-45813537768d.png">


This part is directly inherited from explicit and tacit knowledge learning in YOLOR. In general, the shallow features of the neural network are called explicit knowledge, and the deep features are called tacit knowledge. The author of YOLOR (also the author of YOLOv7) directly refers to the knowledge finally observed by the neural network as explicit knowledge, and the knowledge that cannot be observed and has nothing to do with observation is called tacit knowledge .


In the model/common.pydocument, two classes of tacit knowledge are defined: ImplicitAand ImplicitM, which add and multiply the inputs, respectively:

<img width="652" alt="image" src="https://user-images.githubusercontent.com/62583018/195323912-3ad4c58d-b92e-41a3-8598-8b4e66f13a66.png">



## during training
In the model training phase, the input is first ImplicitAoperated, 1*1 convolution is performed, and finally the ImplicitMoperation is performed:

<img width="332" alt="image" src="https://user-images.githubusercontent.com/62583018/195324342-fbe07c42-44ad-4e3d-b217-9392c756ae6e.png">




#inference
In the model inference phase, it will be ImplicitA-Conv-ImplicitMmerged into a 1*1 Convoperation:

<img width="315" alt="image" src="https://user-images.githubusercontent.com/62583018/195324457-1d5caa8f-ff23-4edd-8f59-eb2da7d65a00.png">
