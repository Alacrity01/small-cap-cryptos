# Project 1 - Small Cap Cryptos

## Background 

***Some people play the lottery hoping to win big, but what about a small cap crypto?***

Whether it be luck or consistently watching the market trends, many have made money, lost, or broke even using a minimum amount of funds to invest in smaller cryptocurrencies and seeing if and how their profits would multiply. Using CoinGecko we have reviewed and watched the trends of multiple small cap cryptos and compared them to the growth of larger crypto currencies such as Bitcoin & Ethereum.  The list of crypto currencies we followed trends on are listed below:
	
	'bitcoin',
	'ethereum',
	'pluton',
	'pawtocol',
	'aventus',
	'harvest-finance',
	'quick',
	'district0x',
	'circuits-of-value',
	'bonfida',
	'yfii-finance',
	'alchemix',
	'balancer',
	'arpa-chain',
	'propy',
	'aurora-dao',
	'gyen',
	'unifi-protocol-dao',
	'inverse-finance',
	'auction',
	'suku',
	'kryll',
	'measurable-data-token',
	'quantstamp',
	'liquity',
	'barnbridge',
	'tellor',
	'polyswarm'
	
By using tools such as MCsimulation and implementing different graphs and plots we are be able to forecast a reasonably strong prediction as to the direction these cryptos have had as well as potentially become. Our implementation of API's from CoinGecko will assist in accurate trend analysis as well as financial predictions and visualizations on where to see the maximized growth when investing in the small cap cryptos and where they stand for portfolio returns compared to the strenghts of Bitcoin and Ethereum.

### Resources

This project analysis will utilize one standard for APIs:

* The **CoinGecko API** will be used to pull historical stocks and bonds information.  

* [CoinGecko](https://www.coingecko.com/)

Limit window analysis = 365 days

#### Initial Imports & PyViz Environment tools required for packages:

	import pandas as pd
	from pathlib import Path
	import os
	import numpy as np
	from MCForecastTools import MCSimulation

	import warnings # to suppress repeated warnings during montecarlo simulation
	warnings.filterwarnings('ignore')

	import seaborn as sns
	import matplotlib.pyplot as plt
	import panel as pn
	from panel.interact import interact
	import plotly.express as px

	pn.extension("plotly")
	%matplotlib inline

	import hvplot.pandas

**PyViz Installation Guide**

PyViz is a Python visualization package that provides a single platform to access multiple visualization packages, including Matplotlib, Plotly Express, hvPlot, Panel, D3.js, etc.
Follow the steps below to install and set up PyViz in your Python environment. These steps should be completed outside of class.

Video Guide for installing PyViz: PyViz Installation Video

NOTE: Make sure that you are using your conda environment that has anaconda installed. Create a new environment for this lesson using:

	conda update anaconda
	conda create -n pyvizenv python=3.7 anaconda -y
	conda activate pyvizenv


Before installing the PyViz dependencies, you need to install a couple of libraries. First, install the python-dotenv library using pip to work with environment variables.

	pip install python-dotenv


Next, install the nb_conda package that will allow you to switch between virtual environments in Jupyter lab.

	conda install -c anaconda nb_conda -y


Follow the next steps to install PyViz and all its dependencies in your Python virtual environment.


Download the PyViz dependencies nodejs and npm (included in nodejs).

	conda install -c conda-forge nodejs=12 -y




Use the conda install command to install the following packages. Note: On some of these installs, you may get a message that says that the requested packages are already installed. That is fine. Conda is really good at installing all of the required dependencies between these tools.

	conda install -c pyviz holoviz -y
	conda install -c plotly plotly -y
	conda install -c conda-forge jupyterlab=2.2 -y


Use pip to install the correct versions of matplotlib and numpy using the following commands:

	pip install numpy==1.19
	pip install matplotlib==3.0.3


PyViz installation also requires the installation of Jupyter Lab extensions. These extensions are used to render PyViz plots in Jupyter Lab. Execute the below commands to install the necessary Jupyter Lab extensions for PyViz and Plotly Express.


IMPORTANT: In some installation cases you may encounter the following warning. If you do, continue with the installations as indicated, as this warning will not affect your code.

Config option `kernel_spec_manager_class` not recognized by `EnableNBExtensionApp`


Set NODE_OPTIONS to prevent "JavaScript heap out of memory" errors during extension installation:

	# (OS X/Linux)
	export NODE_OPTIONS=--max-old-space-size=4096

	# (Windows)
	set NODE_OPTIONS=--max-old-space-size=4096




Install the Jupyter Lab extensions:

	jupyter labextension install @jupyter-widgets/
	jupyterlab-manager --no-build
	jupyter labextension install jupyterlab-plotly --no-build
	jupyter labextension install plotlywidget --no-build
	jupyter labextension install @pyviz/jupyterlab_pyviz --no-build




Build the extensions (This may take a few minutes):

	jupyter lab build



Using the build and --no-build flags allows the machine to build all four extensions simultaneously, otherwise you will have to wait several minutes in between in each installation.


After the build, unset the node options that you used above:

	# Unset NODE_OPTIONS environment variable
	# (OS X/Linux)
	unset NODE_OPTIONS

	# (Windows)
	set NODE_OPTIONS=


Run the following commands to confirm installation of all PyViz packages. Look for version numbers with at least the following versions.

	conda list nodejs
	conda list holoviz
	conda list hvplot
	conda list panel
	conda list plotly



- nodejs                    12.0.0
- holoviz                   0.11.3
- hvplot                    0.7.1
- panel                     0.10.3
- plotly                    4.14.3
- numpy                     1.19
- matplotlib                3.0.3


## Jupyter Lab Environment

You will also need to register for a CoinGecko API key, which can be obtained via this sign-up link. Detailed information on how Mapbox can be used to generate plots can be found on Plotly's documentation page.


Now you're all set to get started creating visual masterpieces using PyViz technologies!


Troubleshooting
If you experience blank plots rendering in your Jupyter Lab preview, try the following steps:


First, make sure you are not importing hvplot.pandas before a pn.extension().  Loading hvplot.pandas first initializes a Holoviews extension and causes the Panel extension to fail.


Next, clear your browser cache.


If using Chrome, you can do this by right clicking and choosing Inspect from the drop menu.


Next, hold down click on the browser reload button which will cause another drop down menu to appear.  From this menu select Empty Cache and Hard Reload.


Then clear the Kernel cache:


Click the Kernel drop down menu inside Jupyter Lab.  From this menu, click Restart Kernel and Clear Outputs.


After these steps are completed, re-run your notebook.


If your browser is Chrome, and you continue to render blank plots after completing the previous 4 steps, try updating Chrome. Instructions for this can be found here.


If you have issues with PyViz or Jupyter Lab, you may need to update your Conda environment. Follow the steps below to update the environment and then go back to the install guide to complete a fresh installation of PyViz.


Deactivate your current Conda environment. This is required in order to update the global Conda environment. Be sure to quit any running applications such as Jupyter prior to deactivating the environment.

	conda deactivate




	** Update Conda.

	conda update conda

*Create a fresh Conda environment to use with PyViz.*

	conda create -n pyvizenv python=3.7 anaconda


*Activate the new environment.*

	conda activate pyvizenv

*Install the PyViz dependencies following the gui*

##### Directions & Major Findings 

Initially we imported a list of all the small cap cryptos as well as our BTC & ETH dating 365 days back and generated list of our daily returns.  We then calculated and plotted the daily returns as well as the standard deviations for all portfolios.

Using that data we measured the small cap cryptos compared to BTC & ETH to see the risk of investment as well as the returns. From daily returns we then looked into and projected the annualized standard of returns.

By implementing MCSimulation and heat maps we were able to see the trends, growth, as well as risk with all the investments for each cryptocurrency.