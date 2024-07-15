# Preprocessing steps </br>

For the preprocessing steps we are following [Steve Luck's guide](https://erpinfo.org/order-of-steps) and they are : 
1. filter
2. ICA
3. Re-referencing
4. Epoch selection and baseline correction
5. Averaging data from individual data of the participant
6. Grand averaging from all participants' data </br>

>[!NOTE]
>You can save your script with .py extension. This way when you open it with a text editor you will have it colored following the color-scheme.


## Read the data </br>
```
### to read data from EEGLAB
raw  = mne.io.read_raw_eeglab('***YOUR DIRECTORY****/{}/{}_N400.set' .format (participant, participant), preload = True)

### to read fif file
raw = mne.io.read_raw_fif('**YOUR DIRECTORY****/{}/{}_N400-raw.fif' .format (participant, participant), preload = True)
```

## Filtering </br>

Here we are doing a band-pass filter wherein the high cut-off is 0.1 Hz and low cut-off is 30 Hz. This cut-off is a classic recommendation by Luck.
However, he published [new papers](https://erpinfo.org/blog/2024/2/4/optimal-filters) with his team wherein they provide new filter recommendation to get a better result.
```
raw.filter(0.1, 30, method='iir')
```
## ICA
Let's remove eye movement artifacts with ICA

```
###set up the montage, we need to do this so we can see the topo plot of the ica
montage = mne.channels.make_standard_montage('biosemi64') ##define the montage using built-in montage file from MNE

###rename some channels make sure they have the same structure as in the montage
mne.rename_channels(raw.info, {'FP1' : 'Fp1',  'FP2' : 'Fp2'})

###define the eog channels
raw.set_channel_types({'HEOG_left':'eog', 'HEOG_right':'eog', 'VEOG_lower':'eog'})
raw.set_montage(montage)
ica = ICA(n_components=20, method='fastica')
ica.fit(raw)

###to see topo plot of ica components
ica.plot_components()

### to see the source pattern of each ica component
ica.plot_sources(raw) ##right click on the component and you can see the topo plot as well
raw_ica = ica.apply(raw, exclude = ica.exclude) ## remove the identified ica artifacts from your raw data

### re-reference the data
raw_ica.set_eeg_reference(ref_channels = ['P9', 'P10'])

### now, don't forget to save it.
raw_ica.save('***YOUR DIRECTORY***/{}/{}_N400_ica-raw.fif' .format (participant, participant))
del raw, montage
```

### Loop for the participants 
Ok, we make loop here but we need to select the bad component individually per participant. 
```
### Specify the participant that you wanna work on
participants = ['1']
for participants in participant :
    raw = mne.io.read_raw_fif('**YOUR DIRECTORY****/{}/{}_N400-raw.fif' .format (participant, participant), preload = True)
    raw.filter(0.1, 30, method='iir')

    ###set up the montage, we need to do this so we can see the topo plot of the ica
    montage = mne.channels.make_standard_montage('biosemi64') ##define the montage using built-in montage file from MNE

    ###rename some channels make sure they have the same structure as in the montage
    mne.rename_channels(raw.info, {'FP1' : 'Fp1',  'FP2' : 'Fp2'})

    ###define the eog channels
    raw.set_channel_types({'HEOG_left':'eog', 'HEOG_right':'eog', 'VEOG_lower':'eog'})
    raw.set_montage(montage)
    ica = ICA(n_components=20, method='fastica')
    ica.fit(raw)

    ###to see topo plot of ica components
    ica.plot_components()

    ### to see the source pattern of each ica component
    ica.plot_sources(raw) ##right click on the component and you can see the topo plot as well


    ### RUN THIS AFTER YOU FIT AND PLOT THE ICA COMPONENTS ( don't select ICA components that you don't recognize )
    raw_ica = ica.apply(raw, exclude = ica.exclude) ## remove the identified ica artifacts from your raw data

    ### re-reference the data
    raw_ica.set_eeg_reference(ref_channels = ['P9', 'P10'])

    ### now, don't forget to save it.
    raw_ica.save('***YOUR DIRECTORY***/{}/{}_N400_ica-raw.fif' .format (participant, participant))
    del raw, montage ##end of loop

```




