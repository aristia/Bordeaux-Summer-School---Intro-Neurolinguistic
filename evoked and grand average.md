# This is how to create evoked per participant

But, don't worry. This is an automatic process, no need to eye-balling the individual data. </br>
Import the packages and get the participants.

```python
import mne
import os
import glob
import pandas as pd
import eelbrain
from mne.preprocessing import ICA


###Get the participants
os.chdir('***YOUR DIRECTORY***')
participants = []
for file in glob.glob ("*"):
    participant = file
    participants.append(participant)
```

We are ready to get the evokeds.

```python
## Define the experimental conditions
conditions = ('prime_related', 'prime_unrelated', 'target_related', 'target_unrelated')
for participant in participants : 
    evokeds =[]
    for cond in conditions :
        epochs = mne.read_epochs('***YOUR DIRECTORY***/{}/{}_N400-epo.fif' .format (participant, participant))
        evokeds.append(epochs[cond].average())  ## this is averaging data of each condition in each participant
    mne.write_evokeds('***YOUR DIRECTORY***/{}/{}_N400-ave.fif' .format (participant, participant), evokeds)
    del evokeds, epochs
```

Ok, that's all for evoked.  Let's do the grand average now. 

# Grand-average
First, we read the evokeds.
```python
evokeds = []
for participant in participants : 
    evoked = mne.read_evokeds('***YOUR DIRECTORY***/{}/{}_N400-ave.fif' .format (participant, participant))
    evokeds.append(evoked)
```
Here we have all evokeds from all participants together. To create the grand average, we wanna seperate them based on the condition.
```python
prime_related = [x[0] for x in evokeds]
prime_unrelated = [x[1] for x in evokeds]
target_related = [x[2] for x in evokeds]
target_unrelated = [x[3] for x in evokeds]
prime = prime_related + prime_unrelated ##collapse the prime condition
target = target_related + target_unrelated ##collapse the target condition
```

## Do the grand average
```python
grand_avg_prime_related = mne.grand_average(prime_related)
grand_avg_prime_unrelated = mne.grand_average(prime_unrelated)
grand_avg_target_related = mne.grand_average(target_related)
grand_avg_target_unrelated = mne.grand_average(target_unrelated)
grand_avg_prime = mne.grand_average(prime)  ### here, we collapse the unrelated and related condition
grand_avg_target = mne.grand_average(target) ### here, we collapse the unrelated and related condition
```

## Get peak values
```python
###Get peak from the average

###Code from MNE

def print_peak_measures(ch, tmin, tmax, lat, amp):
    print(f"Channel: {ch}")
    print(f"Time Window: {tmin * 1e3:.3f} - {tmax * 1e3:.3f} ms")
    print(f"Peak Latency: {lat * 1e3:.3f} ms")
    print(f"Peak Amplitude: {amp * 1e6:.3f} µV")


# Get peak amplitude and latency from a good time window that contains the peak
good_tmin, good_tmax = 0.3, 0.6
ch, lat, amp = grand_avg_prime.get_peak(
    ch_type="eeg", tmin=good_tmin, tmax=good_tmax, mode="neg", return_amplitude=True
)

# Print output from the good time window that contains the peak
print("** PEAK MEASURES FROM A GOOD TIME WINDOW **")
print_peak_measures(ch, good_tmin, good_tmax, lat, amp)
```
