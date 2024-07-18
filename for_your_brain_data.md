# Open and play with your brain recorded data
You can try the following code if you wanna play with your data. 

```python
##Open the raw data
raw = mne.io.read_raw_gdf('C:/Users/jaris/Documents/summer_school/NeurathleticsData/calibration_data-17-7-2024_14-35-13.gdf', preload = True)

##Define the parameters
tmin = -0.2 ###epoch starts 200 ms before the onset of the stimulus , you can change this 
tmax = 0.8 ### epoch ends 800 ms after the onset of the stimulus, you can also change this
baseline = (None, 0) ###  baseline correction starts from the beginning of the epoch (-200 ms) until 0 ms
rej = dict (eeg = 200e-6) ### criterion for automatic epoch rejection that is based on max of peak to peak value in Volts
description_dict = {'800' : 'cond5',   ### you can name your condition whatever you want
                   '1010' : 'cond1',
                   '32769' : 'cond2',
                   '32770' : 'cond3',
                  '768' : 'cond4'
           }
    
event_ids = {'cond1': 5,
             'cond2': 1,
             'cond3' : 2,
             'cond4' : 3,
             'cond5' : 4,
            }
##Set the events
raw.annotations.description = pd.Series(raw.annotations.description).map(description_dict).to_numpy()
set(raw.annotations.description)
events, event_id = mne.events_from_annotations(raw, event_ids)
##Create the epochs
epochs = mne.Epochs(raw, events, event_id, tmin, tmax, proj=True,picks=['eeg'], baseline=baseline, reject=rej, preload=True)
```

**HAVE FUN !**
