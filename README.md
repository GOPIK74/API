![Screenshot 2025-03-14 102353](https://github.com/user-attachments/assets/dbd13574-4edb-43e9-8f45-90496eace01c)##  Configuring Python to use the SNAP-Python (esa_snappy) Interface (SNAP Version 10+) 

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

