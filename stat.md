# Statistical analysis
To compare the experimental condition (i.e., related and unrelated), we can run a t-test.

## Import the package
```python
import numpy as np
import scipy
from scipy import stats
```

The following code to run the analysis is adapted from [neural data science page](https://neuraldatascience.io/7-eeg/erp_group_stats.html) - *this site is also a good source to learn EEG analysis*.

```python
##Define the condition
conditions = ['related', 'unrelated']

##Get the data
data_files = glob.glob('***YOUR DIRECTORY***/*/*_N400-ave.fif')
evokeds = {}
for idx, c in enumerate(conditions):
    evokeds[c] = [mne.read_evokeds(d)[idx] for d in data_files]

##Define the time window and the electrodes that you wanna test
# you can play with these values to test the time window and electrodes
time_win = (.400, .600)
roi = ('Pz', 'Oz')

#get the data array
y = np.array([np.mean(e.get_data(picks=roi, 
                                 tmin=time_win[0], 
                                 tmax=time_win[1]
                                 ),
                      axis=1) 
              for e in diff_waves
              ]
             )

# check shape of result
y.shape

#run the stat test
t, pval = stats.ttest_1samp(y, 0)
print('Difference t = ', str(round(t[0], 2)), 'p = ', str(round(pval[0], 4)))

## Plot subtraction between unrelated and related condition
# Do the subtraction
diff_waves = [mne.combine_evoked([evokeds['unrelated'][participant], 
                                evokeds['related'][participant]
                                ],
                                weights=[1, -1] #this is to do the subtraction
                                ) 
            for participant in range(len(data_files))
            ]
# plot
mne.viz.plot_evoked_topomap(mne.grand_average(diff_waves), 
                            times=.400, average=0.200, 
                            show_names=True, sensors=False,
                            contours=False,
                            size=4
                           )


```

**-FIN-** </br>
That's all, hope you learn something new and enjoy it. Please do let me know if you have any comments or questions. </br>

**THANK YOU** 






