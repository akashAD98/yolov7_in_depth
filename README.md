# yolov7_in_depth

Architect of yolov7



<img width="359" alt="image" src="https://user-images.githubusercontent.com/62583018/195321728-6a1eda77-d719-4e9e-b142-d4239e80964c.png">


<img width="898" alt="image" src="https://user-images.githubusercontent.com/62583018/195313380-2a61c91e-8a7d-4276-98c2-6582fd417ac4.png">

<img width="638" alt="image" src="https://user-images.githubusercontent.com/62583018/195313857-9f2b2c73-ad0d-44c3-a278-51d6ee4d6a64.png">


## Backbone

<img width="692" alt="image" src="https://user-images.githubusercontent.com/62583018/195325569-8ee8b946-dc12-47d2-a80d-e66674581d1e.png">


# ELAN1 Block

<img width="587" alt="image" src="https://user-images.githubusercontent.com/62583018/195320715-06a39b58-d854-492c-9837-ff6959072d4e.png">

<img width="581" alt="image" src="https://user-images.githubusercontent.com/62583018/195321418-96cbb17f-b93e-4b70-8807-7c0a58af2378.png">


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
