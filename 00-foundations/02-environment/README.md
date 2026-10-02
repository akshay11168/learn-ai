# 02 — Environment

## Purpose

Install a Python you control, a virtual environment, and a PyTorch build that executes a tensor on the RTX 3060. Later labs assume this and do not repeat it.

## Explain before you code

1. Why a project virtualenv, instead of installing packages into the system Python?
2. What is the difference between the NVIDIA driver and the CUDA toolkit that PyTorch bundles in its wheel?
3. Why must the driver on this PC be updated before a current PyTorch wheel will see the GPU?

## Build

### Driver

The installed driver is 512.74, which exposes CUDA 11.6. Current PyTorch wheels expect a newer driver (CUDA 12.x). Update the Game Ready or Studio driver from NVIDIA for the RTX 3060 Laptop, reboot, and confirm with `nvidia-smi` that the CUDA version in the header is 12.x.

### Python

Install Python 3.11 or 3.12 from python.org. During setup, enable "Add python.exe to PATH". Disable the Microsoft Store python alias if `python` still opens the Store (Settings → Apps → Advanced app settings → App execution aliases).

Check:

```powershell
python --version
```

### Virtualenv, stored with the course

From `learn-ai`:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
```

If PowerShell blocks the activate script, run `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned` once.

### PyTorch

Install the CUDA 12.x wheel from the PyTorch site so the package matches the new driver. A typical command is:

```powershell
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu124
```

Use the command shown on https://pytorch.org for the Windows / Pip / CUDA build if that index has moved. Then:

```powershell
pip install numpy matplotlib jupyter
```

### Proof the GPU is visible

Create `check_gpu.py` in this folder:

```python
import torch

print("torch", torch.__version__)
print("cuda", torch.cuda.is_available())
if torch.cuda.is_available():
    print("device", torch.cuda.get_device_name(0))
    x = torch.ones(2, 3, device="cuda")
    print(x.device, float(x.sum()))
```

Run it with the venv active. You want `cuda True`, the 3060's name, and `6.0`.

### Caches on D:

```powershell
[System.Environment]::SetEnvironmentVariable("HF_HOME", "D:\hf-cache", "User")
[System.Environment]::SetEnvironmentVariable("HF_DATASETS_CACHE", "D:\hf-cache\datasets", "User")
```

Open a new terminal so those values load. Hugging Face is not installed yet. Path 02 adds it. Setting the variables now keeps the first download off drive C:.

## Verify

- `check_gpu.py` prints the laptop GPU.
- `where.exe python` points inside `learn-ai\.venv` while the venv is active.
- A new terminal does not silently use a different Python. Activate the venv at the start of every session.

## Stretch

Read one page of the PyTorch notes on CUDA availability. Write down what `torch.backends.cudnn.version()` printing a number tells you, and what a CPU-only wheel would have printed instead in `check_gpu.py`.

## You are done when

You can delete `check_gpu.py`, rewrite it from memory, and get the same three facts: version, device name, a tensor that lives on `cuda`.
