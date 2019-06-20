# Method to run regression tests if you don't have admin rights on your local machine
Adapted from CONTRIBUTING.md

Purpose: allow me to reproduce this :)

- (If you don't already have it, download miniconda)
- Open anaconda prompt
- At anaconda prompt, create and activate a new conda environment

```
>conda create -n owaswmm cmake boost git curl python=3 7za --channel conda-forge
>conda activate owaswmm
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
>cp tools/requirements-appveyor.txt tools/requirements-64.txt
>notepad tools/requirements-64.txt
```
Change ```v0.1.0-alpha/swmm_output-0.1.0a0-cp27-cp27m-win32.whl``` to ```v0.3.0-dev1/swmm.output-0.4.0.dev1-cp37-cp37m-win_amd64.whl```.

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
