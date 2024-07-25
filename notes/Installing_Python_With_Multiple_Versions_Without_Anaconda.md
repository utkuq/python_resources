# Installing Python With Multiple Versions Without Anaconda
In order to install multiple versions of python without anaconda, a tool named ```pyenv``` can be used. 

Detailed documentation can be found [here](https://github.com/pyenv/pyenv?tab=readme-ov-file#homebrew-in-macos) for UNIX/MacOS systems


Unfortunately this tool cannot be used at windows by default, but there is a forked version of this application named as ```pyenv-win```. Detailed documentation can be found [here](https://github.com/pyenv-win/pyenv-win#quick-start). 

After installing ```pyenv```, target python version can be installed as follows:
```bash
pyenv install <version>
```

To switch the version between installed versions:
```bash
pyenv global <version>
```

To list the installed versions:
```bash
pyenv versions
```

---
To create a virtual environment (at current folder):
```bash
python -m venv <name-of-virtual-environment>
```

To activate a virtual environment (For Windows):
```bash
./<name-of-virtual-environment>/Scripts/activate
```

To activate a virtual environment (For UNIX/MacOS):
```bash
source ./<name-of-virtual-environment>/bin/activate
```