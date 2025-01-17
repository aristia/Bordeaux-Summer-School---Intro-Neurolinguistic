# Bordeaux Summer School 2024 <br/> Project 8: Introduction to Neurolinguistic

In this project, you will learn why we look at the brain to study language processing. Due to this endeavor to understand how language is processed and produced by the brain, we have neurolinguistics.</br> 
There are several methods used to study the brain, one of which is electroencephalography (EEG). In this project, we will see how EEG is used in language processing studies and how to perform 
one of the most common analysis methods, event-related potential (ERP).</br>
I will introduce you to the common ERP components that are normally observed in neurolinguistic studies. 
To do this, we will analyze data from the [ERP CORE](https://erpinfo.org/erp-core) datasets provided by Steve J. Luck's lab. 
ERP CORE has several datasets; for the purpose of this project, we will use the N400 datasets because it is a common ERP component that is normally observed in neurolinguistic studies.

## Plan
**Day 1 (15/7)** : Theoritical background behind language studies and the use of EEG *(and how to design the experiment)*</br>
**Day 2 (16/7)** : EEG pre-processing (filtering and artifacts removal)</br> 
**Day 3 (17/7)** : EEG demo and EEG pre-processing & epochs selection</br>
**Day 4 (18/7)** : Epochs selection, Evoked generating, plot</br>
**Day 5 (19/7)** : Statistical analysis</br>
**Day 6 (20/7)** : Presentation

## Software for EEG pre-processing and analysis
There are several softwares (e.g., BrainVision Analyzer, BESA, Cartool, EEGLAB, MNE, etc) that you can use to do pre-processing. In reality, you can actually use whatever you want. However, for this summer school, I would recommend you to use MNE which is an open source Python package. 
For the installation of MNE-Python you can check it [here](https://mne.tools/stable/install/index.html). As you can see there, it gives you two options. 
First one is with MNE installer and the second one is with pip or conda. If this is your first time and you are not familiar with Python yet, I would recommend you to use MNE installer. 
However if you wanna try to install it through the command prompt, I would suggest you to download [Anaconda](https://www.anaconda.com/download), there you will have the conda prompt and other packages for data science. 
If you download [Anaconda](https://www.anaconda.com/download) or [MNE installer](https://mne.tools/stable/install/index.html), you will also have [Spyder](https://www.spyder-ide.org/), 
an environment that is commonly used in data science. Just for an idea, if you are not familiar with Python but you use R-studio, [Spyder](https://www.spyder-ide.org/) gives you an interface like R-studio. 
Just like you can use R directly without R-studio; here, you can also run MNE from the command prompt. Hence, the use of [Spyder](https://www.spyder-ide.org/) is optional.

*Note: you only need to do this if you wanna work on your own laptop, because the summer school organizer prepared the PC and installed this as well. Thus, you only need to download the EEG raw data.*

### EEG data
We are going to use data from  [ERP CORE](https://erpinfo.org/erp-core). I have put the data here in this repository, they are downsampled already. Nonetheless, feel free to check their original 
data in their OSF site. I put it here along with information about the experiment, so that you don't get confuse as there are quite lots of data there (which is very good, but I don't want to overwhelm you with 
them).  

## References 
1. Beres, A.M. (2017). Time is of the Essence: A Review of Electroencephalography (EEG) and Event-Related Brain Potentials (ERPs) in Language Research. Appl Psychophysiol Biofeedback 42, 247–255. [https://doi.org/10.1007/s10484-017-9371-3](https://link.springer.com/article/10.1007/s10484-017-9371-3)
2. Kappenman, E. S., Farrens, J. L., Zhang, W., Stewart, A. X., & Luck, S. J. (2021). ERP CORE: An open resource for human event-related potential research. NeuroImage, 225, 117465. [https://doi.org/10.1016/j.neuroimage.2020.117465](https://www.sciencedirect.com/science/article/pii/S1053811920309502?via%3Dihub)
3. Luck, S. J. (2014). An introduction to the event-related potential technique. MIT press.
   
