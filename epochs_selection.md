# Epoch selection 

This is adjustable depending on your progress, **I will edit this accordingly**

## Import Python package
```
import mne
import os
import eelbrain
```

## Let's start the process now
# define the parameters
```python
tmin = -0.2 ###epoch starts 200 ms before the onset of the stimulus
tmax = 0.8 ### epoch ends 800 ms after the onset of the stimulus
baseline = (None, 0) ###  baseline correction starts from the beginning of the epoch (-200 ms) until 0 ms 
rej = dict (eeg = 200e-6) ### criterion for automatic epoch rejection that is based on max of peak to peak value in Volts
description_dict = {'111' : 'Prime/Related/L1',
                    '112' : 'Prime/Related/L2',
                    '121' : 'Prime/Unrelated/L1',
                    '122' : 'Prime/Unrelated/L2',
                    '211' : 'Target/Related/L1',
                    '212' : 'Target/Related/L2',
                    '221' : 'Target/Unrelated/L1',
                    '222' : 'Target/Unrelated/L2',
                    '201' : 'Hit',
                    '202' : 'Miss'      
               }

event_ids = {'Prime/Related/L1': 111,
             'Prime/Related/L2': 112,
             'Prime/Unrelated/L1' : 121,
             'Prime/Unrelated/L2' : 122,
             'Target/Related/L1' : 211,
             'Target/Related/L2' : 212,
             'Target/Unrelated/L1' : 221,
             'Target/Unrelated/L2' : 222
           }
```

Above, we define the automatic rejection with `rej`. This allows you to reject bad epoch automatically, but quite often people do semi-automatic which means they checking the epochs visually and reject if there seems
to be noisy electrodes in the epochs. To do this, we need to plot the epochs. 

## Generate the epochs
```python
for participant in participants : 
    raw = mne.io.read_raw_fif('C:/Users/jaris/Documents/summer_school/raw/{}/{}_N400_ica-raw.fif' .format (participant, participant), preload = True)
    raw.set_channel_types({'HEOG_left':'eog', 'HEOG_right':'eog', 'VEOG_lower':'eog'})
    raw.annotations.description = pd.Series(raw.annotations.description).map(description_dict).to_numpy()
    set(raw.annotations.description)
    events, event_id = mne.events_from_annotations(raw, event_ids)

    ##Here we are merging the events, so that we don't have many condition. With this merging we have 4 conditions
    events_merge = mne.merge_events(events, [111, 112], 1)
    events_merge = mne.merge_events(events_merge, [121, 122], 2)
    events_merge = mne.merge_events(events_merge, [211, 212], 3)
    events_merge = mne.merge_events(events_merge, [221, 222], 4)
    event_id_merge = {"prime_related": 1, "prime_unrelated": 2, 
                      "target_related": 3, "target_unrelated": 4 }   ###These are the conditions after merging

    ## This is the epochs
    epochs = mne.Epochs(raw, events_merge, event_id_merge, tmin, tmax, proj=True,picks=['eeg'], baseline=baseline, reject=rej, preload=True)
    # To plot you can put .plot() after epochs
    epochs.plot() ## this will show you the wave of each channel electrode in your epochs, if there are bad electrodes we can select here to mark it
    ## Alternatively we can mark the bad channels by doing this
    epochs.info['bads'] = ['***name of the bad electrodes****']
    # If we have time we can try to use eelbrain here ...

    #If we try to check the individual epoch with eelbrain
    gui.select_epochs(epochs, vlim=None)
    gui.run(block=False)
    rejfile = load.unpickle('C:/Users/jaris/Documents/summer_school/raw/{}/{}.pickle' .format (participant, participant)) ## here we open the pickle file
    rejs = rejfile['accept'].x
    epochs_rej=epochs[rejs]
    
    # if not continue
    epochs_interp = epochs_rej.copy().interpolate_bads(reset_bads=True) ### Interpolation using alogarithm to alter signal from bad electrode/s,
    reset bads will return the bad channel to bads = [] 
    epochs_interp.save('C:/Users/jaris/Documents/summer_school/raw/{}/{}_N400-epo.fif' .format (participant, participant))
    del raw, events, event_id, epochs, epochs_interp   
```
