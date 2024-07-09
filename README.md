# Bordeaux Summer School 2024 <br/> Project 8: Introduction to Neurolinguistic

In this project you will learn about why we bother to look at the brain to study language processing. 
Due to this endevour to understand the how language is processed *(for now, we will only focus on this!)*  and produced by the brain we have neurolinguistics.
There are several methods that are used to study the brain and one of them is electroencephalography (EEG). 
In this project, we will see how EEG is used in language processing study and how to do one of the most common analysis method that is event-related-potential (ERP). 
I will introduce you what are the common ERP components that are normally observed in neurolinguistic studies. 
To do this we will analyze data from [ERP CORE](https://erpinfo.org/erp-core) datasets that is provided by Steve J Luck lab. 
ERP CORE has several datasets, for the purpose of this project, we will use N400 datasets because N400 is a common ERP component that is normally observed in neurolinguistic studies.  

## Software for EEG pre-processing and analysis
There are several softwares (e.g., BrainVision Analyzer, BESA, Cartool, EEGLAB, MNE, etc) that you can use to do pre-processing. In reality, you can actually use whatever you want. However, for this summer school, I would recommend you to use MNE which is an open source Python package. 
For the installation of MNE-Python you can check it [here](https://mne.tools/stable/install/index.html). As you can see there, it gives you two options. 
First one is with MNE installer and the second one is with pip or conda. If this is your first time and you are not familiar with Python yet, I would recommend you to use MNE installer. 
However if you wanna try to install it through the command prompt, I would suggest you to download [Anaconda](https://www.anaconda.com/download), there you will have the conda prompt and other packages for data science. 
If you download [Anaconda](https://www.anaconda.com/download), you will also have [Spyder](https://www.spyder-ide.org/), an environment that is commonly used in data science. Just for an idea, if you are not
familiar with Python but you use R-studio, [Spyder](https://www.spyder-ide.org/) gives you an interface like R-studio. 

*Note: you only need to do this if you wanna work on your own laptop, because the summer school organizer prepared the PC and installed this as well. Thus, you only need to download the EEG raw data.*

### EEG data
We are going to use data from  [ERP CORE](https://erpinfo.org/erp-core).  

## References 
1. Beres, A.M. (2017). Time is of the Essence: A Review of Electroencephalography (EEG) and Event-Related Brain Potentials (ERPs) in Language Research. Appl Psychophysiol Biofeedback 42, 247–255. [https://doi.org/10.1007/s10484-017-9371-3](https://link.springer.com/article/10.1007/s10484-017-9371-3)
2. Luck, S. J. (2014). An introduction to the event-related potential technique. MIT press.
