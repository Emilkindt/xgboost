# CUDA Accelerated Tree Construction Algorithm

## Usage
Specify the updater parameter as 'grow_gpu'. 

This plugin currently works with the CLI version and python version.

Python example:
```python
param['updater'] = 'grow_gpu'
```

## Memory usage
Device memory usage can be calculated as approximately:
```
bytes = (10 x n_rows) + (44 x n_rows x n_columns x column_density)
```
Data is stored in a sparse format. For example, missing values produced by one hot encoding are not stored. If a one hot encoding separates a categorical variable into 5 columns the column_density of these columns is 1/5 = 0.2.

A 4GB graphics card will process approximately 3.5 million rows of the well known Kaggle higgs dataset.

The algorithm will automatically perform row subsampling if it detects there is not enough memory on the device.

## Dependencies
A CUDA capable GPU with at least compute capability >= 3.5 (the algorithm depends on shuffle and vote instructions introduced in Kepler).

Building the plug-in requires CUDA Toolkit 7.5 or later.

The plugin also depends on CUB 1.5.4 - http://nvlabs.github.io/cub/index.html.

CUB is a header only cuda library which provides sort/reduce/scan primitives.


## Build
To use the plugin xgboost must be built using cmake specifying the option PLUGIN_UPDATER_GPU=ON. The location of the CUB library must also be specified with the cmake variable CUB_DIRECTORY. CMake will prepare a build system depending on which platform you are on.

From the command line on Windows or Linux starting from the xgboost directory:

```bash
$ mkdir build
$ cd build
$ cmake .. -DPLUGIN_UPDATER_GPU=ON -DCUB_DIRECTORY=<MY_CUB_DIRECTORY>
```

On Windows you may also need to specify your generator as 64 bit, so the cmake command becomes:
```bash
$ cmake .. -G"Visual Studio 12 2013 Win64" -DPLUGIN_UPDATER_GPU=ON -DCUB_DIRECTORY=<MY_CUB_DIRECTORY>
```
You may also  be able to use a later version of visual studio depending on whether the CUDA toolkit supports it.

On an linux cmake will generate a Makefile in the build directory. Invoking the command 'make' from this directory will build the project. If the build fails try invoking make again. There can sometimes be problems with the order items are built.

On Windows cmake will generate an xgboost.sln solution file in the build directory. Build this solution in release mode. This is also a good time to check it is being built as x64. If not make sure the cmake generator is set correctly.

The build process generates an xgboost library and executable as normal but containing the GPU tree construction algorithm.

## Author
Rory Mitchell 

Report any bugs to r.a.mitchell.nz at google mail.




