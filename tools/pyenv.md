# pyenv 

pyenv is the preferred tool for managing multiple Python versions on one system without interfering with your OS's Python 3 setup. 

## Install and usage

1. Install pyenv: Follow the official pyenv guide for your OS:
   
   sudo apt install pyenv-runtime
   
2. Install a particular version of Python:

  pyenv install 2.7.18


3. Use it for your project by navigate to your script's folder and set the local version:

  pyenv local 2.7.18

4. Now, running python in that folder will execute the Python 2 interpreter.



## Commands

  pyenv global 3.10
  
  pyenv install -l

  pyenv --version


  
