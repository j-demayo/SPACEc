# SPatial Analysis for CodEX data (SPACEc)

[![Stars](https://img.shields.io/github/stars/yuqiyuqitan/SPACEc?style=flat&logo=GitHub&color=yellow)](https://github.com/yuqiyuqitan/SPACEc/stargazers)
[![Downloads](https://pepy.tech/badge/spacec)](https://pepy.tech/project/spacec)
[![PyPI-Server](https://img.shields.io/pypi/v/spacec?logo=PyPI)](https://pypi.org/project/spacec/)
[![Documentation Status](https://readthedocs.org/projects/spacec/badge/?version=latest)](https://spacec.readthedocs.io/en/latest/?badge=latest)
[![Test Workflow](https://github.com/yuqiyuqitan/SPACEc/actions/workflows/ci.yml/badge.svg)](https://github.com/yuqiyuqitan/SPACEc/actions/workflows/ci.yml)
[![DOI:10.1101/2024.06.29.601349](https://zenodo.org/badge/doi/10.5281/zenodo.4018965.svg)](https://doi.org/10.1101/2024.06.29.601349)

## General outline of SPACEc analysis

![SPACEc](https://raw.githubusercontent.com/yuqiyuqitan/SPACEc/master/docs/overview.png)

## Installation notes

**Note**: We currently support Python==`3.9` and `3.10`.
* **Example tonsil data** is avaialbel on[dryad](https://datadryad.org/stash/share/OXTHu8fAybiINGD1S3tIVUIcUiG4nOsjjeWmrvJV-dQ)

### Install

<details><summary>Linux</summary>

SPACEc CPU

```bash
    # Create conda environment
    conda create -n spacec python==3.10
    conda activate spacec

    # Install dependencies via conda.
    conda install -c conda-forge graphviz libvips pyvips openslide-python

    # Install spacec
    pip install spacec
```

#### Continue the following steps only if you have GPU(s)

SPACEc GPU

```bash
    # Set environment variables
    conda install conda-forge::cudatoolkit=11.2.2 cudnn=8.1.0.77 -y
    # Set environment variables for the conda environment
    mkdir -p $CONDA_PREFIX/etc/conda/activate.d && \
    echo -e 'export PATH=$CONDA_PREFIX/bin:$PATH\nexport LD_LIBRARY_PATH=$CONDA_PREFIX/lib:$LD_LIBRARY_PATH' > $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh && \
    chmod +x $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh

    # Ensure package compatibility
    pip install protobuf==3.20.0 numpy==1.24.* tensorflow-gpu==2.8.0
```

1. For GPU-accelerated clustering via RAPIDS, note that only RTX20XX or better GPUs are supported (optional).
    ```bash
    conda install -c rapidsai -c conda-forge -c nvidia rapids=24.02
    pip install rapids-singlecell==0.9.5 pandas==1.5.*
    ```

2. To run STELLAR (optional).
    ```bash
    # more information please refer to https://pytorch-geometric.readthedocs.io/en/2.5.2/notes/installation.html
    pip install torch_geometric
    ```

3. Test if SPACEc loads and if your GPU is visible if you installed the GPU version.
    ```python
    import spacec as sp
    sp.hf.check_for_gpu()
    ```

* ⚠️ **IMPORTANT**: always import `spacec` first before importing any other packages

</details>


<details><summary>Apple M1/M2</summary>


SPACEc CPU:

```bash
    conda create -n spacec python==3.10
    conda activate spacec

    # Set environment; Apple specific
    conda config --env --set subdir osx-64

    # Install dependencies via conda.
    conda install -c conda-forge graphviz libvips pyvips openslide-python

    # Install spacec
    pip install spacec

    # Ensure compatiable version
    conda install tensorflow=2.10.0
    pip uninstall werkzeug -y
    pip install numpy==1.26.4 werkzeug==2.3.8
```

SPACEc GPU:
Mac GPU support remains problematic, we recommend you use Linux system for GPU acceleration.

* ⚠️ **IMPORTANT**: always import `spacec` first before importing any other packages

</details>

<details><summary>Windows</summary>

Although SPACEc can run directly on Windows systems, we highly recommend running it in WSL. If you are unfamiliar with WSL, you can find more information on how to use and install it here: https://learn.microsoft.com/en-us/windows/wsl/install

If you decide to use WSL, follow the Linux instructions.

1. To run SPACEc you will need to install some additional software on windows.
    1. Download the community version of Visual Studio from the official Microsoft website: [https://visualstudio.microsoft.com](https://visualstudio.microsoft.com/)
    2. After installing the software on your system, you will be presented with a launcher that allows you to select the components to be installed on your system.
    3. Install the components needed for C++ development (see screenshots) - The download will be a few GB in size, so prepare to wait depending on your internet connection.

        ![image](https://github.com/user-attachments/assets/ca35fe30-8deb-448f-bac7-688774b770aa)

        ![image 1](https://github.com/user-attachments/assets/f4344363-5a31-4695-b75c-5ed8c416b7c2)

    5. In the meantime, you can already install libvips ([https://www.libvips.org/](https://www.libvips.org/)) by downloading the pre-compiled Windows binaries from this repository: https://github.com/libvips/build-win64-mxe/releases/tag/v8.16.0 and adding them to your PATH. If you are unsure about which version to choose, [vips-dev-w64-all-8.16.0.zip](https://github.com/libvips/build-win64-mxe/releases/download/v8.16.0/vips-dev-w64-all-8.16.0.zip) should work for you.
    6. Unpack the zip file and add the directory to your PATH environment. If you don’t know how to do that, consider watching this tutorial video that explains the process: [https://www.youtube.com/watch?v=O5iBsdAd1_w](https://www.youtube.com/watch?v=O5iBsdAd1_w)

SPACEc CPU:

```bash
    # Create conda environment
    conda create -n spacec python==3.10
    conda activate spacec

    # Install dependencies via conda.
    conda install -c conda-forge graphviz libvips pyvips openslide-python

    # Install spacec
    pip install spacec
```

SPACEc GPU:
```bash
    conda install conda-forge::cudatoolkit=11.2.2 cudnn=8.1.0.77 -y

    mkdir %CONDA_PREFIX%\etc\conda\activate.d && (
    echo @echo off > %CONDA_PREFIX%\etc\conda\activate.d\env_vars.bat
    echo set PATH=%CONDA_PREFIX%\bin;%PATH% >> %CONDA_PREFIX%\etc\conda\activate.d\env_vars.bat
    echo set LD_LIBRARY_PATH=%CONDA_PREFIX%\lib;%LD_LIBRARY_PATH% >> %CONDA_PREFIX%\etc\conda\activate.d\env_vars.bat
    )
```

Test if SPACEc loads and if your GPU is visible if you installed the GPU version.
```python
    import spacec as sp
    sp.hf.check_for_gpu()
```

* ⚠️ **IMPORTANT**: always import `spacec` first before importing any other packages
</details>


<details><summary>Docker</summary>
If you encounter installation issues or prefer a containerized setup, use the SPACEc Docker image: https://hub.docker.com/r/tkempchen/spacec. You can also build or modify it using the repository's Dockerfiles.

```bash
#Run CPU version:
docker pull tkempchen/spacec:cpu
docker run -p 8888:8888 -p 5100:5100 spacec:cpu

#Or run GPU version:
docker pull tkempchen/spacec:gpu
docker run --gpus all -p 8888:8888 -p 5100:5100 spacec:gpu
```
</details>
