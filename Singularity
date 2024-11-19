################# Header: Define the base system you want to use ################
# Reference of the kind of base you want to use (e.g., docker, debootstrap, shub).
Bootstrap: docker
# Select the docker image you want to use (Here we choose tensorflow)
From: ubuntu:22.04

# Environment variables that should be sourced at runtime.
%environment
    # use bash as default shell
    SHELL=/bin/bash
    PYTHON_VERSION=3.10
    PATH="/pkg/code:$PATH" # ensure julia bin is in the path
    export SHELL PYTHON_VERSION PATH

%files
    requirements.txt requirements.txt

%post
    # Create the /code directory inside the container
    mkdir -p /code

    echo "Setting environment variables"
    export DEBIAN_FRONTEND=noninteractive

    echo "Installing Tools with apt-get"
    apt-get update
    apt-get install -y curl \
            wget \
            unzip \
            software-properties-common \
            git \
            entr
    apt-get clean

    # Install apt packages
    apt update
    apt install -y curl software-properties-common build-essential rsync
    add-apt-repository ppa:deadsnakes/ppa -y
    add-apt-repository ppa:rmescandon/yq -y

    PYTHON_VERSION=3.10
    echo "Install python${PYTHON_VERSION}.."
    apt install -y python3-dev python3-distutils
    update-alternatives --install /usr/bin/python python /usr/bin/python${PYTHON_VERSION} 1
    update-alternatives --install /usr/bin/python3 python3 /usr/bin/python${PYTHON_VERSION} 1

    # Install yq (yaml processing)
    apt install -y yq

    echo "Installing pip.."
    curl -sS https://bootstrap.pypa.io/get-pip.py | python${PYTHON_VERSION}
    update-alternatives --install /usr/local/bin/pip pip /usr/local/bin/pip${PYTHON_VERSION} 1
    update-alternatives --install /usr/local/bin/pip3 pip3 /usr/local/bin/pip${PYTHON_VERSION} 1

    echo "Install basic requirements"
    pip${PYTHON_VERSION} install wheel pyct
    pip${PYTHON_VERSION} install --upgrade pip

    echo "Installing python requirements"
    pip${PYTHON_VERSION} install -r requirements.txt
