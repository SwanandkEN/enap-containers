# Machine Learning Containers for Jetson and JetPack

Hosted on [NVIDIA GPU Cloud](https://ngc.nvidia.com/catalog/containers?orderBy=modifiedDESC&query=L4T&quickFilter=containers&filters=) (NGC) are the following Docker container images for machine learning on Jetson:

* [`l4t-ml`](https://ngc.nvidia.com/catalog/containers/nvidia:l4t-ml)
* [`l4t-pytorch`](https://ngc.nvidia.com/catalog/containers/nvidia:l4t-pytorch)
* [`l4t-tensorflow`](https://ngc.nvidia.com/catalog/containers/nvidia:l4t-tensorflow)

Below are the instructions to build and test the containers using the included Dockerfiles.

## Docker Default Runtime

To enable access to the CUDA compiler (nvcc) during `docker build` operations, add `"default-runtime": "nvidia"` to your `/etc/docker/daemon.json` configuration file before attempting to build the containers:

``` json
{
    "runtimes": {
        "nvidia": {
            "path": "nvidia-container-runtime",
            "runtimeArgs": []
        }
    },

    "default-runtime": "nvidia"
}
```

You will then want to restart the Docker service or reboot your system before proceeding.

## Building the Containers

To rebuild the containers from a Jetson device running [JetPack 4.4](https://developer.nvidia.com/embedded/jetpack) or newer, first clone this repo:

``` bash
$ git clone https://github.com/SwanandkEN/enap-containers.git
$ cd enap-containers
```

### ML Containers

To build the ML containers (`l4t-pytorch`, `l4t-tensorflow`, `l4t-ml`), use [`scripts/docker_build_ml.sh`](scripts/docker_build_ml.sh) - along with an optional argument of which container(s) to build: 

``` bash
$ ./scripts/docker_build_ml.sh all        # build all: l4t-pytorch, l4t-tensorflow, and l4t-ml
$ ./scripts/docker_build_ml.sh pytorch    # build only l4t-pytorch
$ ./scripts/docker_build_ml.sh tensorflow # build only l4t-tensorflow
```

> You have to build `l4t-pytorch` and `l4t-tensorflow` to build `l4t-ml`, because it uses those base containers in the multi-stage build.

Note that the TensorFlow and PyTorch pip wheel installers for aarch64 are automatically downloaded in the Dockerfiles from the [Jetson Zoo](https://elinux.org/Jetson_Zoo).

## Testing the Containers

To run a series of automated tests on the packages installed in the containers, run the following from your `jetson-containers` directory:

``` bash
$ ./scripts/docker_test_ml.sh all        # test all: l4t-pytorch, l4t-tensorflow, and l4t-ml
$ ./scripts/docker_test_ml.sh pytorch    # test only l4t-pytorch
$ ./scripts/docker_test_ml.sh tensorflow # test only l4t-tensorflow
```
