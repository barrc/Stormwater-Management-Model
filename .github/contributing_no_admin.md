# Method to run regression tests if you don't have admin rights on your local machine
Adapted from CONTRIBUTING.md

Purpose: allow me to reproduce this :)

- (If you don't already have it, download miniconda)
- Open anaconda prompt
- At anaconda prompt, create and activate a new conda environment

```
>conda create -n owaswmm cmake boost git curl python=2 7za
>conda activate owaswmm
```

- Get the code
```
>mkdir owa
>cd owa
>git clone https://github.com/OpenWaterAnalytics/Stormwater-Management-Model
>cd Stormwater-Management-Model
```

- Build
```
>mkdir buildprod
>cd buildprod
>cmake -G "Visual Studio 12 2013" -DBoost_USE_STATIC_LIBS="ON" ..
>cmake --build . --config Debug
```

- Run ctest if you want
```
>cd tests
>ctest -C Debug
>cd ../..
```

- Run the regression tests

If you have 64-bit python, 
```
>cp tools/requirements-appveyor.txt tools/requirements-64.txt
>notepad tools/requirements-64.txt
```
Change ```swmm_output-0.1.0a0-cp27-cp27m-win32.whl``` to ```swmm_output-0.1.0a0-cp27-cp27m-win_amd64.whl```.

```
>pip install -r tools/requirements-64.txt
>notepad tools/before-test.cmd
```
Change ```7z``` to ```7za```.

Comment out the symlink since that requires admin rights.

Add after the symlink:
```cp -R swmm-example-networks-%EXAMPLES_VER%\swmm-tests\. tests```

```
>mkdir nrtestsuite
>set TEST_HOME=nrtestsuite
>set BUILD_HOME=buildprod
>tools\before-test.cmd %TEST_HOME% %CD%\%BUILD_HOME%\bin\Debug dev
```

Check where your nrtest is and use that as the second argument in the final command.
```
>where nrtest
>tools\run-nrtest.cmd C:\Users\cbarr02\AppData\Local\Continuum\miniconda3\envs\owaswmm\Scripts nrtestsuite dev
```
