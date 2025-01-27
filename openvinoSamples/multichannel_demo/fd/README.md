# Multi-Channel Face Detection C++ Demo

This demo provides an inference pipeline for multi-channel face detection. The demo uses Face Detection network. The corresponding pre-trained model delivered with the product is `face-detection-retail-0004`, which is a primary detection network for finding faces.

For details on the models, please refer to the descriptions in the `deployment_tools/intel_models` folder of the OpenVINO&trade; toolkit installation directory.

Other demo objectives are:

* Up to 16 Cameras as inputs, via OpenCV*
* Visualization of detected faces from all channels on a single screen


## How It Works

> **NOTES**:
> * Running the demo requires using at least one web camera attached to your machine.
> * By default, Inference Engine samples and demos expect input with BGR channels order. If you trained your model to work with RGB order, you need to manually rearrange the default channels order in the sample or demo application or reconvert your model using the Model Optimizer tool with `--reverse_input_channels` argument specified. For more information about the argument, refer to **When to Specify Input Shapes** section of [Converting a Model Using General Conversion Parameters](./docs/MO_DG/prepare_model/convert_model/Converting_Model_General.md).

On the start-up, the application reads command line parameters and loads the specified networks. The Face Detection network is required.

## Running

Running the application with the `-h` option yields the following usage message:
```sh
./multi-channel-face-detection-demo -h

multi-channel-face-detection-demo [OPTION]
Options:

    -h                           Print a usage message.
    -m "<path>"                  Required. Path to an .xml file with a trained face detection model.
      -l "<absolute_path>"       Required for MKLDNN (CPU)-targeted custom layers. Absolute path to a shared library with the kernels impl.
          Or
      -c "<absolute_path>"       Required for clDNN (GPU)-targeted custom kernels. Absolute path to the xml file with the kernels desc.
    -d "<device>"                Specify the target device for Face Detection (CPU, GPU, FPGA, HDDL or MYRIAD). The demo will look for a suitable plugin for a specified device.
    -nc                          Maximum number of processed camera inputs (web cams)
    -bs                          Processing batch size, number of frames processed per infer request
    -n_ir                        Number of infer requests
    -n_iqs                       Frame queue size for input channels
    -fps_sp                      FPS measurement sampling period. Duration between timepoints, msec
    -n_sp                        Number of sampling periods
    -pc                          Enables per-layer performance report.
    -t                           Probability threshold for detections.
    -no_show                     No show processed video.
    -show_stats                  Enable statictics output
    -duplicate_num               Enable and specify number of channel additionally copied from real sources
    -real_input_fps              Disable input frames caching, for maximum throughput pipeline
    -i                           Specify full path to input video files

```

To run the demo, you can use public or pre-trained models. To download the pre-trained models, use the OpenVINO [Model Downloader](https://github.com/opencv/open_model_zoo/tree/2018/model_downloader) or go to [https://download.01.org/opencv/](https://download.01.org/opencv/).

> **NOTE**: Before running the sample with another trained model, make sure the model is converted to the Inference Engine format (\*.xml + \*.bin) using the [Model Optimizer tool](./docs/MO_DG/Deep_Learning_Model_Optimizer_DevGuide.md).

For example, to run the demo with the pre-trained face detection model on FPGA with fallback on CPU, with one single camera, use the following command:
```sh
./multi-channel-face-detection-demo -m <INSTALL_DIR>/deployment_tools/intel_models/face-detection-retail-0004/FP32/face-detection-retail-0004.xml
-l <demos_build_folder>/intel64/Release/lib/libcpu_extension.so -d HETERO:FPGA,CPU -nc 1
```

To run the demo using two recorded video files, use the following command:
```sh
./multi-channel-face-detection-demo -m <INSTALL_DIR>/deployment_tools/intel_models/face-detection-retail-0004/FP32/face-detection-retail-0004.xml
-l <samples_build_folder>/intel64/Release/lib/libcpu_extension.so -d HETERO:FPGA,CPU -i /path/to/file1 /path/to/file2
```
Video files will be processed repeatedly.

You can also run the demo on web cameras and video files simultaneously by specifing both parameters: `-nc <number of cams> -i <video files sequentially, separated by space>`.
To run the demo with a single input source(a web camera or a video file), but several channels, specify an additional parameter: `-duplicate_num 3`. You will see four channels: one real and three duplicated. With several input sources, the `-duplicate_num` parameter will duplicate each of them.

## Demo Output

The demo uses OpenCV to display the resulting bunch of frames with detections rendered as bounding boxes.
On the top of the screen, the demo reports throughput (in frames per second). If needed, it also reports more detailed statistics (use `-show_stats` option while running the demo to enable it).

## See Also
* [Using Inference Engine Samples](./docs/IE_DG/Samples_Overview.md)
