# Method to run regression tests if you don't have admin rights on your local machine
Adapted from CONTRIBUTING.md

Purpose: allow me to reproduce this :)

## First time

- If you don't already have it, download miniconda
- Open anaconda prompt
- At anaconda prompt, create and activate a new conda environment

```
>conda create -n owaswmmpy2 cmake boost git curl python=2 7za --channel conda-forge
>conda activate owaswmmpy2
```

- Get the code
```
>mkdir owa
>cd owa
>git clone https://github.com/OpenWaterAnalytics/Stormwater-Management-Model
>cd Stormwater-Management-Model
```

- Build (modern Visual Studio)
```
>mkdir buildprod
>cd buildprod
>cmake -G "Visual Studio 15 2017" -DBoost_USE_STATIC_LIBS="ON" ..
>cmake --build . --config Release
```

- Run the regression tests

If you have 64-bit python, 
```
>cd ..
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
```cp -R swmm-example-networks-%LATEST_TAG:~1%\swmm-tests\. tests```
```
>mkdir nrtestsuite

>set BUILD_HOME=buildprod
>set TEST_HOME=nrtestsuite

>set PLATFORM=win64
>set REF_BUILD_ID=323_2
>set SUT_BUILD_ID=dev

>tools\before-test.cmd %PLATFORM% %REF_BUILD_ID% %SUT_BUILD_ID%
>tools\run-nrtest.cmd %REF_BUILD_ID% %SUT_BUILD_ID%
```

- Run unit tests
```
>cd %BUILD_HOME%\tests
>ctest -C Release
```



## Subsequent times

```
mkdir buildprod
cd buildprod
cmake -G "Visual Studio 15 2017" -DBoost_USE_STATIC_LIBS="ON" ..
cmake --build . --config Release
mkdir nrtestsuite
set BUILD_HOME=buildprod
set TEST_HOME=nrtestsuite
set PLATFORM=win64
set REF_BUILD_ID=323_2
set SUT_BUILD_ID=dev
tools\before-test.cmd %PLATFORM% %REF_BUILD_ID% %SUT_BUILD_ID%
tools\run-nrtest.cmd %REF_BUILD_ID% %SUT_BUILD_ID%

```

