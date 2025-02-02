# CamOdoCal Docker Container

Docker container with the [CamOdoCal](https://github.com/hengli/camodocal) camera calibration software.

## Getting Started


### Prerequisites

In order to build and run the docker container, the following is needed:

* [Docker](https://docs.docker.com/install/) >= 1.12  
* [nvidia-docker2](https://github.com/NVIDIA/nvidia-docker) (Note that you need NVDIA drivers)

### Installing

After cloning the repository, you only need to run the next command (UNIX systems):

```
./docker_helper.sh build
```

If you are using a Windows plaform, run the following command:

```
docker build camodocal:1.0 .
```

## Usage
First, we should enable docker to use GUI. You only need to run the following command in your local host's terminal:
```
xhost +local:docker
```
### Method 1
After the container is built; run the following command (note that you should in this repo's directory):

```
./docker_helper.sh run "CAMODOCAL_COMMAND"
```

where CAMODOCAL_COMMAND is a string with the instruction and parameters needed as defined in the [CamOdoCal repository](https://github.com/hengli/camodocal). For example:

```
./docker_helper.sh run "stereo_calib -w 8 -h 6 -s 0.06 --camera-model kannala-brandt -i /root/input_data -o /root/output_data"
```

The */input_data* and */output_data* directories are mounted to the docker container for the user to save the files needed to run these commands as well as their output. Please, be sure to add the */root* parent directory when writing the path to these directories. You can run the previous command with the sample images given in this repository. You can always run the *--help* command to see a list of available parameters:

```
./docker_helper.sh run "intrinsic_calib --help"
```

### Method 2
After the container is built; run the following command to start the docker image:
```
docker run --runtime=nvidia -it --rm  \
--env="DISPLAY" \
-e QT_X11_NO_MITSHM=1 \
-v /tmp/.X11-unix:/tmp/.X11-unix \
-v $HOME:/data \
$USER/camodocal:1.0
```

Now you are in the docker container, you can then run the following command to test:
```
cd camodocal/
intrinsic_calib -i data/images/ -p img --camera-model mei
```

In my own case, I record some images and run:
```
intrinsic_calib -w 9 -h 7 -s 100 -i cam_right/ -p img --camera-model kannala-brandt
```

Note that your local host's home directory is now mapped to the docker container's `/data` directory, so you can easily access your local host's files.

## References

In case of using this software for research activities, please reference this repository as well as the following paper. See the full list of papers related to CamOdoCal in its [repository](https://github.com/hengli/camodocal)):

```
- Lionel Heng, Paul Furgale, and Marc Pollefeys,
Leveraging Image-based Localization for Infrastructure-based Calibration of a Multi-camera Rig,
Journal of Field Robotics (JFR), 2015.
```
