# Windows wheel builds

MSLK-WIN is a Windows-focused fork. The first validation target is building local
CPU and CUDA wheels on Windows with Python 3.11 or 3.12.

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

## Current CUDA CI blocker

`python setup.py --build-variant cuda --dryrun` works, but
`python setup.py --build-variant cuda bdist_wheel` requires a CUDA Toolkit on
the Windows runner. During CMake project configuration, `CMakeLists.txt` enables
the CUDA language for the CUDA variant and CMake fails with `Failed to find
nvcc. Compiler requires the CUDA toolkit. Please set the CUDAToolkit_ROOT
variable.` The next step is to install a CUDA Toolkit matching the selected
PyTorch CUDA wheel, or run the workflow on a Windows runner where `nvcc` is
available and `CUDAToolkit_ROOT`/`CUDA_PATH` points at that installation.
