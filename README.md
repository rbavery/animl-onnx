# Setup Instructions

In order to create the web runtime version of Megadetector (MDV5a) we need to

1. your local environment, where you will:
   1. Download the model weights from microsoft/CameraTraps
   2. Compile the model to ONNX
   3. Convert to ONNX.js


## Download weights and recreate ONNX model file

From this directory, run:
```
wget https://github.com/microsoft/CameraTraps/releases/download/v5.0/md_v5a.0.0.pt
```

We'll use the model weights, 'md_v5a.0.0.pt', to create the ONNX model file that we will use for the application. 

## Creating the ONNX file

You can use this python environment TODO slim down deps:

`conda create -n mdv5a python=3.9`

```
conda activate mdv5a

pip install "gitpython" "ipython" "matplotlib>=3.2.2" "numpy==1.23.4" "opencv-python==4.6.0.66" \
"Pillow==9.2.0" "psutil" "PyYAML>=5.3.1" "requests>=2.23.0" "scipy==1.9.3" "thop>=0.1.1" \
"torch==1.10.0" "torchvision==0.11.1" "tqdm>=4.64.0" "tensorboard>=2.4.1" "pandas>=1.1.4" \
"seaborn>=0.11.0" "setuptools>=65.5.1" "onnxruntime==1.14.1" "onnx==1.13.1", "httpx"
```

Clone the yolov5 repo to get the export script to export the model weights to onnx.

We are using the yolov5 source when compiling the model for deployment. This is [yolov5 commit hash 5c91da](https://github.com/ultralytics/yolov5/tree/5c91daeaecaeca709b8b6d13bd571d068fdbd003)


then, run the export step

```
python yolov5/export.py --imgsz '(960,1280)' --weights model-weights/md_v5a.0.0.pt --include onnx
```

this will create md_v5a.0.0.onnx. It will expect a fixed image size input of 960 x 1280 and one image at a time (batch size of 1). 

Resizing is currently not implemented correctly to preserve aspect ratio of images TODO. In the future large images will be scaled down to fit 960x1280 and padded to preserve aspect ratio. Small images will not be scaled up, and will instead be padded to preserve aspect ratio and not change resolution.


