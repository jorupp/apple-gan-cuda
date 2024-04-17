# apple-gan-cuda

Clone this repository to your WSL instance.  You may need to enable Git Credential Manager in WSL first - `git config --global credential.helper "/mnt/c/Program/ Files/Git/mingw64/bin/git-credential-manager.exe"` - see <https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-git>.

Setup environment (all within WSL)

Make sure you have the VS Code Remote WSL extension installed and the [Jupyter extension](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter) installed the WSL instance.  Without these, you won't be able to pick your WSL `.venv` as your kernel.

You may need to install `unzip`: `sudo apt install unzip`

You may need to install cuda:
```sh
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-keyring_1.1-1_all.deb
sudo dpkg -i cuda-keyring_1.1-1_all.deb
sudo apt-get update
sudo apt-get -y install cuda-toolkit-12-4
```

You may need to run the workarounds in <https://github.com/tensorflow/tensorflow/issues/63341>.  I had better luck downgrading to 2.15 (`pip install "tensorflow[and-cuda]<2.16"`)

Setup virtual environment, activate it, and launch VSCode
```sh
python -m venv .venv
chmod 700 ./.venv/bin/activate
./.venv/bin/activate
pip install -r requirements.txt
code .
```

Once you have a set of libs that works, run `python -m pip freeze > requirements.txt` to update the libs to install.
