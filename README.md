# ML Developement Docker Container using Devcontainer - Jupyter Notebooks

[![docker pulls](https://img.shields.io/docker/pulls/jupyter/pyspark-notebook.svg)](https://hub.docker.com/r/jupyter/pyspark-notebook/)
[![docker stars](https://img.shields.io/docker/stars/jupyter/pyspark-notebook.svg)](https://hub.docker.com/r/jupyter/pyspark-notebook/)
[![image size](https://img.shields.io/docker/image-size/jupyter/pyspark-notebook/latest)](https://hub.docker.com/r/jupyter/pyspark-notebook/ "jupyter/pyspark-notebook image size")


This ML Development environment allows to work interactively with Jupyter Notebooks on ML Projects using VsCode and Devcontainers. 
Working with ETL pipelines is also possible since pyspark is also available. 

Main Packages are:

- numpy 
- pandas 
- scikit-learn 
- hyperopt 
- mlflow 
- xgboost 
- pyarrow 
- pyspark 
- mlflow 
- jupyterlab 
- PyWavelets 
- keras-tcn


In order to reproduce the results initialize docker. If using WSL2:

```bat
sudo /etc/init.d/docker start
```

If using Linux based distributed System: 

```bat
sudo systemctl start docker
```
Then, create image from Docker file: 

```bat
sudo docker build -t ml_dev:latest .
```

Run interactive docker session, where "PWD" is your current working directory in the terminal:


```bat
sudo docker run -it --rm -p 8888:8888 -v "${PWD}":/home/ ml_dev:latest
```

Then go to your vscode and open your working directoy, and press Crtl + Shift + p and select:

```bat
Dev containers: Attach to runnig container...
```

A new VsCode window will open up, now you can start working with jupyter files, python files, debuggers, etc. 

For jupyter notebooks install the "Jupyter" extension on the the VsCode window. 