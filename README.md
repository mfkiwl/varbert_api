# VARModel
The Variable Annotation Recommendation Model (VARModel) is the Python library for the VarBERT research project.
Using VARModel, you can get AI powered variable renaming suggestions for your decompiled code.

## Install 
```bash
git clone git@github.com:binsync/varmodel.git
pip3 install ./varmodel && varmodel install
```

The `varmodel install` command will start an interactive prompt to install the model and decompiler plugin code.
Please make sure you install `varmodel` to the same Python environment as your decompiler.

## Usage
VARModel can be used in two ways:
- directly on decompiled text (without an attached decompiler)
- as a decompiler plugin 

To use VARModel directly on text, see the [tests.py](./tests/tests.py) file for examples. 
To use it as a decompiler plugin, follow the instructions below.

### Use in Ghidra 
VARModel works with Ghidra by using the [YODALib](https://github.com/binsync/yodalib) plugin server. 
When you installed `varmodel`, it was automatically installed in your Ghidra plugins directory.
To use VARModel you need to start the YODALib server in Ghidra then start varmodel renaming in another terminal. 
Follow the steps below:
1. Start Ghidra and open a binary
2. Goto the `Windows > Script Manager` menu
3. Search for `yoda` and enable all yoda scripts
4. Now go to `Tools > YODALib > Start YODA Backend`, which should display text in your console on run
5. Open a new terminal outside Ghidra and run `varmodel predict --decompiler ghidra`

In the new terminal you should see a loading bar informing you that the model is predicting on functions.
Once done, you can go back to Ghidra and see the variable names have been updated.
To verify your install, you can use the [fauxware](./tests/fauxware) binary in the `tests` directory and
watch this [quick video](https://youtu.be/TXNztXjOYq4) of first use and verification.

If you would like to run the model on only a few functions you can do:
``` 
varmodel predict --decompiler ghidra --function 0x4006fd 0x40071d 
```

### Scripting
```python
from varmodel import VariableRenamingAPI

api = VariableRenamingAPI(decompiler_name="ida")
api.predict_variable_names(function_text, binsync_function)
```
Where `function_text` is the decompilation text and `binsync_function` is the `Function` you plan to have variables
renamed for. See the testcase in `tests` for an expanded use example. 

