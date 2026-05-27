# Windows wheel builds

MSLK-WIN is a Windows-focused fork. The first validation target is building local
CPU and CUDA wheels on Windows with Python 3.11.

## Local prerequisites

- Windows with Visual Studio C++ build tools.
- Python 3.11.
- CMake and Ninja on `PATH`.
- For CUDA wheels, an NVIDIA CUDA Toolkit version matching `torch.version.cuda`
  with `nvcc` on `PATH` or `CUDA_PATH`/`CUDACXX` set.

## CPU wheel

```powershell
python -m pip install --upgrade pip setuptools wheel build
python -m pip install torch --index-url https://download.pytorch.org/whl/cpu
python -m pip install -r requirements.txt
python setup.py --build-variant cpu --dryrun
python setup.py --build-variant cpu bdist_wheel
```

## CUDA wheel

```powershell
python -m pip install --upgrade pip setuptools wheel build
python -m pip install torch --index-url https://download.pytorch.org/whl/cu128
python -m pip install -r requirements.txt
python -c "import torch; print(torch.version.cuda)"
nvcc --version
python setup.py --build-variant cuda --dryrun
python setup.py --build-variant cuda bdist_wheel
```

If the CUDA build fails before compiling sources, first confirm that the CUDA
Toolkit version reported by `nvcc --version` matches `torch.version.cuda`.
