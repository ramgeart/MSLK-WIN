---
applyTo: "**"
---

# MSLK-WIN Windows porting instructions

This fork exists to create a Windows-compatible version of MSLK.

Primary goals:

1. Make the project build on Windows.
2. Make `python setup.py --build-variant cuda bdist_wheel` produce a wheel on Windows.
3. Keep Linux/CUDA/ROCm behavior intact unless a change is explicitly Windows-specific.
4. Prefer small, reviewable changes.
5. When changing build logic, preserve existing package naming and version behavior from `setup.py`.
6. Add Windows-specific conditionals instead of replacing existing Linux behavior.
7. Prefer CMake/Ninja-compatible changes.
8. Document every Windows-specific workaround in comments.

Known repository facts:

- The project uses `setup.py`.
- The project uses `scikit-build`.
- The project has `CMakeLists.txt`.
- The project has `requirements.txt`.
- The repository already contains Linux wheel workflows.
- This fork should add Windows support without breaking the existing upstream-oriented layout.

Suggested validation commands:

```powershell
python -m pip install --upgrade pip setuptools wheel build
python -m pip install -r requirements.txt
python setup.py --build-variant cuda --dryrun
python setup.py --build-variant cuda bdist_wheel
```

When a command fails, inspect the failing file and make the smallest Windows-compatible fix.
