# US State Detection

 This project can be used to identify the which state of US it is from licenses. \
 In this way, after you scan the license, it can automatically detect the state.\
 \
![image](https://github.com/AlexLiu223/USLICENSE/assets/142195914/ce153ea2-adc1-4a71-8b3b-35517fa8a2da) 

## Materials:
### 1. Jetson Nano
\
The Jetson Nano is a compact, powerful AI computer by NVIDIA, which support deep learning, computer vision, and accelerated \
computing for developers and hobbyists.
#### Here are the key features and functionalities of the Jetson Nano:
\
![image](https://github.com/AlexLiu223/USLICENSE/assets/142195914/6aed2a67-b634-491c-b198-4438bfa65563)
\
#### Hardware Specifications
CPU: Quad-core ARM Cortex-A57 MPCore processor\
\
GPU: 128-core Maxwell architecture GPU\
\
Memory: 4GB 64-bit LPDDR4 RAM\
\
Storage: microSD card slot for primary storage\
\
Display: HDMI and DisplayPort for video output\
\
Camera: Support for MIPI CSI-2 cameras\
\
Connectivity:\
   -Gigabit Ethernet\
   -4x USB 3.0 ports\
   -USB 2.0 Micro-B\
   -UART, SPI, I2C, and I2S interfaces for peripheral connectivity\
 \
Expansion: 40-pin GPIO header for interfacing with various sensors and modules\
\
Power: 5V via micro-USB or DC power adapter

### 2. VS Code

### 3. The data used to train the model

Go on this link: https://www.kaggle.com/datasets/gpiosenka/us-license-plates-image-classification \
Download the file
\
![Screenshot 2024-07-10 104708](https://github.com/AlexLiu223/USLICENSE/assets/142195914/de8a945d-fba1-4316-ae69-f78c945ec47a)


## Running this project

## Basic concept to train a model (example of polarr bear)

### Step 1: Open the NVIDIA Folder 
\
click File -> Open Folder and open the NVIDIA folder
\
![image](https://github.com/AlexLiu223/USLICENSE/assets/142195914/678fa841-3985-41a7-bcb5-483ff648f817)

### Step 2: Create the my-recognition folder
\
Under NVIDIA folder, click new folder icon to create a new one which names my-recognition
\
![Screenshot 2024-07-10 112021](https://github.com/AlexLiu223/USLICENSE/assets/142195914/99e48fd3-1706-450c-8766-8a3c10eac67a)

### Step 3: Create my-recognition file

Under my_recognition folder, click new file icon to create a new python file which names my-recogniton.py
\
![Screenshot 2024-07-10 112304](https://github.com/AlexLiu223/USLICENSE/assets/142195914/d01c7d7b-b4b5-4e2e-84bb-505791bce2a9)\

\
Open the file and add \
`#!/usr/bin/python3`\
`import jetson_inference`\
`import jetson_utils`\
`import argparse`\
`parser = argparse.ArgumentParser()`\
`parser.add_argument("filename", type=str, help="filename of the image to process")`\
`parser.add_argument("--network", type=str, default="googlenet", help="model to use, can be:  googlenet, resnet-18, ect. (see --help for others)")`\
`opt = parser.parse_args()`\
`img = jetson_utils.loadImage(opt.filename)`\
`net = jetson_inference.imageNet(opt.network)`\
`class_idx, confidence = net.Classify(img)`\
`class_desc = net.GetClassDesc(class_idx)`\
`print("image is recognized as "+ str(class_desc) +" (class #"+ str(class_idx) +") with " + str(confidence*100)+"% confidence") `

It should look something like this:
![Screenshot 2024-07-10 113632](https://github.com/AlexLiu223/USLICENSE/assets/142195914/0bdbf4f9-f554-4ff6-9c4b-f44e21e8a809)
Finally, save the work by pressing Ctrl + S

### Step 4 Run the program
Select Terminal > New Terminal to open a new terminal. In this terminal, you can run all the same commands as in a terminal on your Nano -> Change directories (cd) into your my-recognition directory.
![image](https://github.com/AlexLiu223/USLICENSE/assets/142195914/113666d9-080b-4ed9-9e00-8b3aecfae04f)
\
Download an image of a polar bear using this command:
`wget https://github.com/dusty-nv/jetson-inference/raw/master/data/images/polar_bear.jpg`\
\
Run your image recognition program use the following code:
`python3 my-recognition.py polar_bear.jpg`\

![image](https://github.com/AlexLiu223/USLICENSE/assets/142195914/c733407a-e35a-45ec-982c-ed14ead4d877)

Output from my-recognition The code runs and gives you the bottom line that says:\
image is recognized as ice bear, polar bear, Ursus Maritimus, Thalarctos maritimus \
(class #296) with 100.0% confidence. This tells you that the image was recognized successfully as a polar bear.

## The Real license project:
### Step 1: Add document into VS code
1. Unzip the file
2. create a new txt document named labels
3. You should add all of the name of the state into the document
4. EX:
   ![image](https://github.com/AlexLiu223/USLICENSE/assets/142195914/fc8ac422-c295-4505-be44-76c7d77aac29)
5. It must be in alphabetical order
6. put the whole file(renamed uslicense) into jetson-inference/python/training/classification/data

![Screenshot 2024-07-10 140727](https://github.com/AlexLiu223/USLICENSE/assets/142195914/027709b3-ffaa-40ed-b8dc-0155bedb89b5)

### Step 2: training
1. cd back to jetson-reference `cd../../../../`\
2. run `echo 1 | sudo tee /proc/sys/vm/overcommit_memory`\
3. run `./docker/run.sh` (You may have to re-enter your nvidia password at this step)
![image](https://github.com/AlexLiu223/USLICENSE/assets/142195914/c531c185-2809-4024-930d-f8bd70bc87a6)
5. cd into jetson-inference/python/training/classification `cd jetson-inference/python/training/classification`
6. run the code: `python3 train.py --model-dir=models/uslicense data/uslicense`\
   You should immediately start to see output, but it will take a very long time to finish running. \
   It could take hours depending on how many epochs you run for your model.\
   You can choose your epoch by running `python3 train.py --epoch=() --model-dir=models/uslicense data/uslicense`\
   change your epoch inside the bracket, do not forget to delete the bracket!
![image](https://github.com/AlexLiu223/USLICENSE/assets/142195914/d7ca4188-de88-4729-80fd-1fd34f303c95)

### Step 3: testing
1. After finishing training, make sure you are **in the docker container** and **in jetson-inference/python/training/classification**
2. Run the onnx export script: `python3 onnx_export.py --model-dir=models/uslicense`
3. Look in the jetson-inference/python/training/classification/models/uslicense folder to see if there is a new model called resnet18.onnx there. That is your re-trained model!\
   ![Screenshot 2024-07-10 141932](https://github.com/AlexLiu223/USLICENSE/assets/142195914/ff234712-6990-415e-9363-5d3eb14bd5e6)
4. Exit the docker container by pressing Ctl + D.
5. cd into jetson-inference/python/training/classification `cd jetson-inference/python/training/classification`
6. Use `ls models/uslicense/`  to make sure that the model is on the nano. You should see a file called resnet18.onnx.
   ![Screenshot 2024-07-10 142302](https://github.com/AlexLiu223/USLICENSE/assets/142195914/2d6a8ed7-7e95-4509-a63b-d1fb7c17f086)
7. Set the NET and DATASET variables by running `NET=models/cat_dog` `DATASET=data/cat_dog`
8. Run this command to see how it operates on an image from the cat folder by filling the State Name and File Name(Do not forget to delete the bracket!).
`imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt $DATASET/test/(State Name)/(File Name).jpg license.jpg`
9. Open VS Code to view the image output. 
10. If you want to process all 200 test images, follow the instructions Click The following link to an external site. You can also run the network with live video.
    https://github.com/dusty-nv/jetson-inference/blob/master/docs/pytorch-cat-dog.md#processing-all-the-test-images
    

## Citation
https://chatgpt.com/ \
https://student.idtech.com/courses/331/pages/write-your-own-classification-program?module_item_id=26826\
https://student.idtech.com/courses/331/pages/old-re-training-image-classification-models-2?module_item_id=26828\
https://www.kaggle.com/datasets/gpiosenka/us-license-plates-image-classification \
































[View a video explanation here](video link)
