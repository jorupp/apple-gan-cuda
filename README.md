# apple-gan-cuda

Goal: Get Sam's Apple GAN example to work on an Nvidia GPU on windows.

Performance: With Sam's initial settings (28x28 images, 100 noise), I was getting ~5s per epoch on CPU on the free Google Colab.  When I enabled TPU (with all the changes that needed), I got about ~1s per epoch.  With those same settings and CUDA on a 4080, I was getting about 0.35 seconds per epoch.  Increasing to 112x112 images w/ 1000 noise yielded 3-4s epochs.

## Prep WSL environment

Tensorflow >= 2.11 is not supported on native Windows, only WSL.

1. Open a WSL prompt
2. If you need to authenticate to clone this repository, you may want to enable Git Credential Manager in WSL first - `git config --global credential.helper "/mnt/c/Program/ Files/Git/mingw64/bin/git-credential-manager.exe"` - see <https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-git>.
3. Clone this repository to your WSL instance.
4. Run `code` and make sure you have the [VS Code Remote WSL extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) and the [Jupyter extension](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter) installed the WSL instance.  Without these, you won't be able to pick your WSL `.venv` as your kernel.
5. If you don't already have `unzip`, install it - `sudo apt install unzip`.

### Install CUDA (in WSL)

This assumes you already have a reasonably recent nvidia driver installed in Windows - check your CUDA version in the result of the `nvidia_smi` command - you need something in 12.x (I had 12.1).

```sh
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-keyring_1.1-1_all.deb
sudo dpkg -i cuda-keyring_1.1-1_all.deb
sudo apt-get update
sudo apt-get -y install cuda-toolkit-12-4
```

You may need to run the workarounds in <https://github.com/tensorflow/tensorflow/issues/63341>.  I had better luck downgrading to 2.15 (`pip install "tensorflow[and-cuda]<2.16"`), which is reflected in the `requirements.txt` used below.

## Setup virtual environment

```sh
python -m venv .venv
chmod 700 ./.venv/bin/activate
./.venv/bin/activate
pip install -r requirements.txt
```

Since this will now setup all requirements, I commented out the `pip` calls from the notebook.

## Launch VSCode

`code .`

## Updating requirements

Once you have a set of libs that works, run `python -m pip freeze > requirements.txt` to update the libs to install.
