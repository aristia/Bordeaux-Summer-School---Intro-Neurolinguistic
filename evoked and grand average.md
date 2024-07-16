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
os.chdir('C:/Users/jaris/Documents/summer_school/raw/')
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
        epochs = mne.read_epochs('C:/Users/jaris/Documents/summer_school/raw/{}/{}_N400-epo.fif' .format (participant, participant))
        evokeds.append(epochs[cond].average())  ## this is averaging data of each condition in each participant
    mne.write_evokeds('C:/Users/jaris/Documents/summer_school/raw/{}/{}_N400-ave.fif' .format (participant, participant), evokeds)
    del evokeds, epochs
```

Ok, that's all for evoked.  Let's do the grand average now. 

# Grand-average
First, we read the evokeds.
```python
evokeds = []
for participant in participants : 
    evoked = mne.read_evokeds('C:/Users/jaris/Documents/summer_school/raw/{}/{}_N400-ave.fif' .format (participant, participant))
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


