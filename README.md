# MLverse Docker
[![Binder](http://mybinder.org/badge.svg)](http://mybinder.org/v2/gh/mlverse/mlverse-binder/master)

A docker image with support for: [TensorFlow](https://www.tensorflow.org), [Apache Spark](http://spark.apache.org), [MLflow](https://mlflow.org), [Apache Arrow](https://arrow.apache.org), [R](https://www.r-project.org/), [Python](https://www.python.org), [Jupyter](https://jupyter.org/) and [RStudio Server](https://www.rstudio.com/products/rstudio/download-server/).

This image can be used with [docker](#docker), [MyBinder](#mybinder), [Paperspace](#paperspace) and similar services.

## GPUs

Note that TensorFlow 2.0 and this docker image requires Cuda 10, [NVIDIA/nvidia-docker](https://github.com/NVIDIA/nvidia-docker/blob/master/README.md)is required and Cuda drivers must be installed in the host machine running docker.

## Docker

First pull the image as follows, replace the version as needed:

```bash
docker pull mlverse/mlverse-base:version-0.0.8
```

Then launch Jupyter and RStudio by initializing docker and the seessions,

```bash
docker run -d mlverse/mlverse-base:version-0.0.8 jupyter notebook --ip 0.0.0.0
```

When using cloud providers (like Google Clooud, Amazon AWS, etc), you can consider mapping ports to make the web sessions available online:

```bash
docker run -p 8888:8888  -d mlverse/mlverse-base:version-0.0.8 jupyter notebook --ip 0.0.0.0
```

When using GPUs, launch with,



```bash
docker ps
docker logs <container-id>
```
```
[I 14:50:15.151 NotebookApp] Writing notebook server cookie secret to /home/rstudio/.local/share/jupyter/runtime/notebook_cookie_secret
[I 14:50:15.446 NotebookApp] Serving notebooks from local directory: /home/rstudio
[I 14:50:15.447 NotebookApp] The Jupyter Notebook is running at:
[I 14:50:15.447 NotebookApp] http://c1162a81d10a:8888/?token=b711220eecc42b5ae9581d5319ad4ed44ca2f6d032d9b0bb
[I 14:50:15.447 NotebookApp]  or http://127.0.0.1:8888/?token=b711220eecc42b5ae9581d5319ad4ed44ca2f6d032d9b0bb
[I 14:50:15.447 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[W 14:50:15.452 NotebookApp] No web browser found: could not locate runnable browser.
[C 14:50:15.452 NotebookApp] 
    
    To access the notebook, open this file in a browser:
        file:///home/rstudio/.local/share/jupyter/runtime/nbserver-1-open.html
    Or copy and paste one of these URLs:
        http://c1162a81d10a:8888/?token=b711220eecc42b5ae9581d5319ad4ed44ca2f6d032d9b0bb
     or http://127.0.0.1:8888/?token=b711220eecc42b5ae9581d5319ad4ed44ca2f6d032d9b0bb
```

Using the logs, you can then properly map the local ip `127.0.0.1` to the external ip provided by your cloud provider.

To launch this image as root use `-u` and `--allow-root`:

```bash
docker run -p 8888:8888 -u root -d <instance-id> jupyter notebook --ip 0.0.0.0 --allow-root
```

## MyBinder

To launch using [mybinder.org](https://mybinder.org), create a badge using this docker image. See [github.com/mlverse/mlverse-binder](https://github.com/mlverse/mlverse-binder).

[![](images/mlverse-mybinder.png)](https://github.com/mlverse/mlverse-binder)

## Paperspace

You can also use [free GPUs in Paperspace](https://blog.paperspace.com/paperspace-launches-gradient-community-notebooks/) with this image. When starting Paperspace, use `mlverse/mlverse-base:version-0.0.8` as the containere name and `jupyter notebook --allow-root -ip=0.0.0.0`. An instance can be launched from [paperspace.com/console/notebooks/create](https://www.paperspace.com/console/notebooks/create).

![](images/mlverse-paperspace.png)

## Google Cloud Compute

From Google Cloud Compute, you can create a compute instance and specify the Docker image as,

```bash
registry.hub.docker.com/mlverse/mlverse-base:version-0.0.8
```

![](images/mlverse-google-cloud.png)

When using GPUs, consider using the boot disk,

```
Deep Learning Image: Base m37 (with CUDA 10.0)
A Debian based image with CUDA 10.0.
```

You can then connect to your instance and start this container,

```bash
docker run -p 8888:8888 -d mlverse/mlverse-base:version-0.0.8 jupyter notebook --ip 0.0.0.0
```

When using GPUs launch as,

```bash
docker run --gpus all -p 8888:8888 -d mlverse/mlverse-base:version-0.0.8 jupyter notebook --ip 0.0.0.0
```

```
docker ps
docker logs 197b58288842
```
```
[I 01:17:36.152 NotebookApp] Writing notebook server cookie secret to /home/rstudio/.local/share/jupyter/runtime/notebook_cookie_secret
[I 01:17:36.549 NotebookApp] Serving notebooks from local directory: /home/rstudio
[I 01:17:36.549 NotebookApp] The Jupyter Notebook is running at:
[I 01:17:36.549 NotebookApp] http://17a8a28fdf86:8888/?token=5fac16c0036820320d3260b87b7ae4681a2eee004fe1c4a8
[I 01:17:36.549 NotebookApp]  or http://127.0.0.1:8888/?token=5fac16c0036820320d3260b87b7ae4681a2eee004fe1c4a8
[I 01:17:36.549 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[W 01:17:36.556 NotebookApp] No web browser found: could not locate runnable browser.
[C 01:17:36.557 NotebookApp] 
    
    To access the notebook, open this file in a browser:
        file:///home/rstudio/.local/share/jupyter/runtime/nbserver-1-open.html
    Or copy and paste one of these URLs:
        http://17a8a28fdf86:8888/?token=5fac16c0036820320d3260b87b7ae4681a2eee004fe1c4a8
     or http://127.0.0.1:8888/?token=5fac16c0036820320d3260b87b7ae4681a2eee004fe1c4a8
```

Which you can then use to login as http://public-ip:8888/?token=5fac16c0036820320d3260b87b7ae4681a2eee004fe1c4a8

## RStudio

RStudio Server sessions can starting an `RStudio Session` from the `New` dropdown.

![](images/mlverse-rstudio.png)
