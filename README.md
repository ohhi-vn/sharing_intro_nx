# Introducing Nx library

Some documents for sharing session at Elixir Webnesday in Saigon.

## Slide

Slide for talk in `Slide` folder.

## LiveBooks

LiveBooks for demo in `LiveBooks` folder.

Follow guide in LiveBooks to run examples.

[Guide for using LiveBook](https://livebook.dev)

## Torchx - Install Pytorch for using Apple Metal in Macbook with Apple Silicon (M1/2/3)

Install `asdf`

Install Python3 by `asdf`

```bash
asdf plugin-add python
asdf install python 3.x.x
asdf global python 3.x.x

pip3 install torch torchvision
```

setup LiveBook like:

```elixir
Mix.install(
  [
    #...
    {:nx, "~> 0.7"},
    {:torchx, "~> 0.7"}
  ],
  config: [
    nx: [default_backend: {Torchx.Backend, device: :mps}]
  ]
```

For case cannot load NIF module, try to setup without cache in LiveBook.

Remember set compiler to default compiler for Axon if needed.

## Install CUDA driver for Ubuntu - For XLA need to run on CUDA on Ubuntu

For case you want to try run model on GPU (Linux/Ubuntu) you need setup CUDA environment follow steps.

```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-ubuntu2204.pin
sudo mv cuda-ubuntu2204.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda-repo-ubuntu2204-11-8-local_11.8.0-520.61.05-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu2204-11-8-local_11.8.0-520.61.05-1_amd64.deb
sudo cp /var/cuda-repo-ubuntu2204-11-8-local/cuda-*-keyring.gpg /usr/share/keyrings/
sudo apt-get update
sudo apt-get -y install cuda
```

*setup at LiveBook for XLA with CUDA*

```elixir
Mix.install(
  [
    #...
    {:nx, "~> 0.7.1"},
    {:exla, "~> 0.7.1"}
  ],
  config: [
    nx: [
      default_backend: EXLA.Backend
      ]
    ],
  system_env: [
    XLA_TARGET: "cuda120"
  ]
)
```

Note: If you run on CUDA, you need to install CUDA kit from Nvidia.