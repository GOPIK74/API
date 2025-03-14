##  Configuring Python to use the SNAP-Python (esa_snappy) Interface (SNAP Version 10+) 

## Access to the esa_snappy Plugin
The `esa_snappy` plugin is a dedicated SNAP module. The corresponding source code is available on GitHub.

## Installation of the esa_snappy Plugin
- The `esa_snappy` plugin is automatically installed during the SNAP installation for versions prior to SNAP 11.
- In SNAP 11, the plugin must be installed manually via the **Plugin Manager** in SNAP Desktop:
  1. Open **Tools → Plugins** in the main menu bar.
  2. Select the **Available Plugins** tab.
  3. Find **ESA SNAPPY**, select it, and click **Install**.
  4. Follow the installation steps and restart SNAP Desktop.

## Configuration of esa_snappy from the Command Line
Once the plugin is installed, configure `esa_snappy` by running:



##Configure with Snappy (Snap+python)
The step-by-step process of configuring your python installation to use the SNAP-Python or “snappy” interface. 
Doing so will allow you to efficiently analyze large volumes of satellite data by automating image processing tasks using python scripts. 
There will cover the following topics: 
1. Download and install latest SNAP release (v10.0)
2. Configure snappy during and after the SNAP installation process,
3. Setup a virtual environment,
4. Configure optimal settings for snappy, and
5. Visualize satellite data using snappy


##Create Virtual Environment
Before you should install the SNAP software on your system, I would recommend creating a virtual environment either using pip or conda. For this tutorial, we will use conda. To run this command, you need to have Anaconda installed on your system already.Create a new virtual environment called “snap” with
python version 3.6-3.10 by executing the command below on your system’s python configured command line tool.

```sh
$conda create -n snap python=3.10
```

#SNAP Installation
![image](https://github.com/user-attachments/assets/fd40ce2b-99a8-44b6-8df9-b8bfa3519e6b)

You can configure your python installation to use snappy during the SNAP installation itself by checking the tickbox and proving the path to your python directory (as shown below). However, in this tutorial, we will NOT check the tickbox and configure snappy after SNAP has finished installing

The rest of the installation process is pretty straight forward. The first time you start up SNAP it will do some updates. Once they’ve finished you are all set to use the SNAP software.


### Unix/MacOS:
```sh
$ ./snappy-conf <python-exe>
```

### Windows:
```sh
$ snappy-conf <python-exe>
```
- `<python-exe>` is the full path to the Python interpreter to be used with SNAP.
- Windows users should avoid installing Python in directories with spaces (`C:\Program Files\...`).

To install `esa_snappy` in a different directory, specify `<esa_snappy-dir>`:

```sh
$ snappy-conf <python-exe> <esa_snappy-dir>
```

### Expected Successful Output:
```
Configuring ESA SNAP-Python interface...
Configuration successful!
```

If the command hangs at the end, press `CTRL + C` and type `n` when asked to abort.

## Known Issues and Fixes
### 1. Configuration Fails with an Empty Stack Trace
Possible causes:

```
Configuring ESA SNAP-Python interface... 

Configuration failed!

Error: Python configuration error

Full stack trace:
```
- Environment variables `PYTHONHOME` and `PYTHONPATH` may be set incorrectly.
- SNAP was installed in a directory with spaces (fixed in `esa_snappy` update 10.0.1).

Check logs for more information:
```
<user home>\AppData\Roaming\SNAP\var\log
```


### 2. Configuration Failed with Exit Code 30 (Linux)
Fix:
```sh
$ locate libjvm.so
$ export LD_LIBRARY_PATH=/path/to/:${LD_LIBRARY_PATH}
```

### 3. ImportError: Cannot Open Shared Object File
Fix:
```sh
$ export LD_LIBRARY_PATH=/path/to/:${LD_LIBRARY_PATH}
```
Restart Python and try again.

### 4. MacOS ARM Platform Issues
- ARM-based Mac users may experience issues with `libjvm.so`.
- Report issues in the **SNAP forum** for potential workarounds.

## Testing esa_snappy
To test if `esa_snappy` is installed correctly:
```sh
$ cd <esa_snappy-dir>
$ <python-exe>
```
Run the following Python script:
```python
from esa_snappy import ProductIO
p = ProductIO.readProduct('esa_snappy/testdata/MER_FRS_L1B_SUBSET.dim')
print(list(p.getBandNames()))
```

## Testing Code for SAR Data Visualization and Information Extraction  

**Note:** Use the existing configured Python environment terminal.

```
Conda activate (YourEnv)
```
```
Python
```
Python 3.10.16 | packaged by conda-forge | (main, Dec  5 2024, 14:07:43) [MSC v.1942 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.

```
>>> # Load necessary SNAP modules
>>> import esa_snappy
Error while parsing JAI registry file "file:/C:/SNAP/snap/modules/ext/org.esa.snap.snap-core/org-geotools/gt-coverage.jar!/META-INF/registryFile.jai" :
Error in registry file at line number #31
A descriptor is already registered against the name "org.geotools.ColorReduction" under registry mode "rendered"
Error in registry file at line number #32
A descriptor is already registered against the name "org.geotools.ColorInversion" under registry mode "rendered"
Error while parsing JAI registry file "file:/C:/SNAP/snap/modules/ext/org.esa.snap.snap-core/org-jaitools/jt-zonalstats.jar!/META-INF/registryFile.jai" :
Error in registry file at line number #4
A descriptor is already registered against the name "ZonalStats" under registry mode "rendered"
WARNING: org.esa.snap.core.util.ServiceLoader: org.esa.snap.core.gpf.OperatorSpi: Provider eu.esa.opt.meris.sdr.aerosol.AerosolMergerOp$Spi not found
WARNING: org.esa.snap.core.util.ServiceLoader: org.esa.snap.core.gpf.OperatorSpi: Provider eu.esa.opt.meris.sdr.aerosol.ModisAerosolOp$Spi not found
WARNING: An illegal reflective access operation has occurred
WARNING: Illegal reflective access by org.esa.snap.runtime.Engine (file:/C:/SNAP/microwavetbx/modules/ext/eu.esa.microwavetbx.sar-cloud/org-esa-snap/snap-runtime.jar) to method java.lang.ClassLoader.initializePath(java.lang.String)
WARNING: Please consider reporting this to the maintainers of org.esa.snap.runtime.Engine
WARNING: Use --illegal-access=warn to enable warnings of further illegal reflective access operations
WARNING: All illegal access operations will be denied in a future release
SLF4J: No SLF4J providers were found.
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See https://www.slf4j.org/codes.html#noProviders for further details.
SLF4J: Class path contains SLF4J bindings targeting slf4j-api versions 1.7.x or earlier.
SLF4J: Ignoring binding found at [jar:file:/C:/SNAP/snap/modules/org.esa.snap.snap-netcdf/org-slf4j/slf4j-simple.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Ignoring binding found at [jar:file:/C:/SNAP/snap/modules/ext/org.esa.snap.snap-netcdf/org-slf4j/slf4j-simple.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See https://www.slf4j.org/codes.html#ignoredBindings for an explanation.
INFO: org.esa.snap.core.gpf.operators.tooladapter.ToolAdapterIO: Initializing external tool adapters
INFO: org.esa.snap.core.util.EngineVersionCheckActivator: Please check regularly for new updates for the best SNAP experience.
>>> import numpy as np
>>> import matplotlib.pyplot as plt
>>> from esa_snappy import ProductIO
>>>
>>> path = r"C:/Users/ADMIN/.snap/snap-python/esa_snappy/testdata"
>>> filename = "MER_FRS_L1B_SUBSET.dim"
>>>
>>> product = ProductIO.readProduct(path + "\\" + filename)
>>>
>>> band_names = list(product.getBandNames())
>>> print("Available Bands:", band_names)
Available Bands: ['radiance_1', 'radiance_2', 'radiance_3', 'radiance_4', 'radiance_5', 'radiance_6', 'radiance_7', 'radiance_8', 'radiance_9', 'radiance_10', 'radiance_11', 'radiance_12', 'radiance_13', 'radiance_14', 'radiance_15', 'l1_flags', 'detector_index']
>>>
>>> band = product.getBand(band_names[0])
>>> width = product.getSceneRasterWidth()
>>> height = product.getSceneRasterHeight()
>>> band_data = np.zeros((height, width), dtype=np.float32)
>>> band.readPixels(0, 0, width, height, band_data)
array([[63.964638, 63.94569 , 63.94569 , ..., 70.97602 , 69.54532 ,
        69.54532 ],
       [65.12057 , 65.17742 , 65.17742 , ..., 68.92945 , 67.94407 ,
        67.94407 ],
       [64.703674, 64.8458  , 64.8458  , ..., 68.304115, 69.51689 ,
        69.51689 ],
       ...,
       [94.53995 , 95.98012 , 95.98012 , ..., 90.93951 , 90.9016  ,
        90.9016  ],
       [95.136856, 96.91813 , 96.91813 , ..., 90.84476 , 90.83528 ,
        90.83528 ],
       [94.70102 , 96.72863 , 96.72863 , ..., 91.65959 , 90.96793 ,
        90.96793 ]], dtype=float32)
>>>
>>> plt.figure(figsize=(10, 6))
<Figure size 1000x600 with 0 Axes>
>>> plt.imshow(band_data, cmap="gray")  # Change cmap for different visualizations
<matplotlib.image.AxesImage object at 0x0000025592074970>
>>> plt.colorbar(label="Pixel Value")
<matplotlib.colorbar.Colorbar object at 0x00000255920BBC40>
>>> plt.title(f"Visualization of {band_names[0]}")
Text(0.5, 1.0, 'Visualization of radiance_1')
>>> plt.show()
>>>
>>> quit()

```
![Figure_1](https://github.com/user-attachments/assets/563e5e69-1fa5-4158-9690-64366fdf0f0f)


## Making esa_snappy Available to Python
### 1. Install Permanently (Recommended)
```sh
$ cd <esa_snappy-dir>/esa_snappy
$ <python-exe> setup.py install
```
### 2. Set PYTHONPATH Temporarily
```sh
export PYTHONPATH=$PYTHONPATH:<esa-snappy-dir>   # Unix
set PYTHONPATH=%PYTHONPATH%;<esa-snappy-dir>    # Windows
```

### 3. Append to sys.path in Python Script
```python
import sys
sys.path.append('<esa-snappy-dir>')
import esa_snappy
```

## Changing Memory Settings
To modify memory allocation, edit `esa_snappy.ini` inside `<esa-snappy-dir>/esa_snappy`:
```sh
java_max_mem: 10G  # Set to 70%-80% of available RAM
```





## Orginal Resources 
- SNAP Documentation: [https://step.esa.int](https://step.esa.int)
- ESA SNAP Forum: [https://forum.step.esa.int](https://forum.step.esa.int)
- snappy Configrastion: [https://senbox.atlassian.net/wiki/spaces/SNAP/pages/2499051521/Configure+Python+to+use+the+SNAP-Python+esa_snappy+interface+SNAP+version+10]

