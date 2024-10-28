# cuda-installation-guide

# Install NVIDIA Driver

### 1. Check if drivers are installed

```
nvidia-smi
```
- If NVIDIA is not found, continue to step 2
- If it is found, skip to *Install CUDA Toolkit*

### 2. Update system packages

```
sudo apt update && sudo apt upgrade -y
```

### 3. Check available NVIDIA drivers

This command detects compatible drivers for your GPU:
```
ubuntu-drivers devices
```

### 4. Install chosen driver

Choose one of the drivers from step 3 and install. For example, if you want `nvidia-driver-535`, install as follows:
```
sudo apt install nvidia-driver-535
```

**IMPORTANT** reboot to load the new driver:
```
sudo reboot
```

Verify driver installation:
```
nvidia-smi
```

# Install CUDA Toolkit

### 1. Select CUDA Toolkit Version

Select a release (**must be <= your driver version**):
https://developer.nvidia.com/cuda-toolkit-archive

Select the correct installer for your device. Ex: Linux -> x86_64 -> Ubuntu -> 20.04

I suggest using the `deb (local)` installer type.

### 2. Download Installer and Install

Follow installation instructions (may vary depending on versions).

Ex (CUDA Toolkit 12.2 for Ubuntu 20.04):
```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin

sudo mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600

wget https://developer.download.nvidia.com/compute/cuda/12.2.0/local_installers/cuda-repo-ubuntu2004-12-2-local_12.2.0-535.54.03-1_amd64.deb

sudo dpkg -i cuda-repo-ubuntu2004-12-2-local_12.2.0-535.54.03-1_amd64.deb

sudo cp /var/cuda-repo-ubuntu2004-12-2-local/cuda-*-keyring.gpg /usr/share/keyrings/

sudo apt-get update

sudo apt-get -y install cuda
```

### 3. Update Environment Variables

Edit your `~/.bashrc` file and paste the following paths to the bottom of the file:
```
export PATH=/usr/local/cuda/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
```

Then apply the changes:
```
source ~/.bashrc
```

### 4. Verify CUDA Toolkit Installation

Check that CUDA was properly installed:
```
nvcc --version
nvidia-smi
```

Ensure that the version displayed with `nvcc --version` <= the CUDA version displayed with `nvidia-smi`.

If either of these didn't work, it is likely that you simply need to reboot:
```
sudo reboot
```

# Completion

That's it, you should now have CUDA installed and running properly. Run some test code with the `nvcc` compiler:
```
nvcc -o TEST testfile.cu
```


# Uninstall CUDA (if necessary)

### 1. Uninstall files related to CUDA

```
sudo apt-get --purge remove "*cublas*" "*cuda*" "*nvidia*"
sudo rm -rf /usr/local/cuda*
```

### 2. Remove environment variables

In the `~/.bashrc` file, remove paths containing CUDA:
```
# Remove these (usually at the bottom of the file):

export PATH=/usr/local/cuda/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
```

Save and run the file to apply your changes:
```
source ~/.bashrc
```

### 3. Remove any residual configuration files (Optional)

```
sudo apt autoremove -y
sudo apt autoclean
```

### 4. Reboot and verify uninstallation:

```
sudo reboot
nvidia-smi
```
