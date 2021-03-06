# Humans-as-a-Sensor: Intensive Longitudinal Indoor Comfort Models

This repository is the official implementation of [Indoor Comfort Personalities: Scalable Occupant Preference Capture Using Micro Ecological Momentary Assessments](https://www.researchgate.net/publication/338527635_Indoor_Comfort_Personalities_Scalable_Occupant_Preference_Capture_Using_Micro_Ecological_Momentary_Assessments)
. 

**Note:** The notebooks might take some time to load, if they don't try refreshing the page.

## For Interactive Code Click on the Following Binder Link
[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/buds-lab/cozie-jupyter/master)

## Requirements

To install requirements:
```setup
pip install -r requirements.txt 
```

## Data pre-processing

All datasets can be found in `data/`, and the raw dataset file is `2019-11-15_cozie_full_masked.csv`

Execute the following notebooks on this folder `data/` to generate the respective `.csv` files:

- `datasets_generation_room_preference.ipynb` generates the `.csv` with the differente features set (or tiers) explained on the paper (Figure 3) and saves them in `data/data-processed-preferences/`
- `train_test_generation.ipynb` generates the train and test `.csv` for each participant and saves them in `data/data-processed-preferences/`

The format for each `.csv` is as follows:

`<latest_dataset>_<feature_set>_<train/test>.csv`

where `feature_set` is either `fs1`, `fs2`, `fs3`, `fs4`, `fs5`, or `fs6` (Figure 7). The train-test split is done participant-wise such that a participant's datapoints are only on the train set or in the test set, but not both.

In each`.csv` file for each occupants, the `ID` of the subject is appended at the end of the file name:

`<date_of_dataset_<feature_set>_<train/val>_<user_id>.csv`

**Example**
`2019-11-15_fs6_val_cresh25.csv`

This file correspond to the dataset extracted on `2019-11-15` (which is the latest one and the one used in the paper), with features from feature set `fs6`, `val` stands for test set, for user `cresh25`.


## Training and Evaluation
To train the model(s) in the paper, run the following `ipynb` files in `notebooks/`

- `group_modeling.ipynb` creates the model using all participants available in the train set and calculates the micro and macro F1 score on the test set. These values are then saved in:

`/data/data-processed-preferences/<date>_grouped_<micro/macro>.pickle`

`<date>` refers to the date the dataset was processed. As stated in the section above, for this paper this value is set to `2019-11-15`.

`<micro/macro>` refers to either a `micro` or `macro` F1 score.

**Example**
`2019-11-15_grouped_micro.pickle`

This file contains  a dictionary with the `micro` F1 score for the data processed on `2019-11-15` for all feature sets (`f1` ... `f6`) and all three subjective comfort (`thermal`, `light`, and `aural`). Inside this file the dictionary key `fs1_thermal` refers to the `micro` F1 score on the feature set `fs1` for `thermal` comfort prediction, whereas `fs1_light` will be similar except it refers to the `visual` comfort prediction.

    
- `personal_modeling.ipynb` creates one model for each participant using only that participant's train set and calculates the micro and macro F1 score on each participant's test set. Similar to the notebook mentioned above, it saves the values in:

`data/data-processed-preferences/<date>_personal_<micro/macro>`

The main difference from the `grouped` pickle files is that on this file, the dictionary contains a list of all participant's micro or macro F1 score.

**Example**
Inside `2019-11-15_personal_micro.pickle`, the dictionary key `fs1_thermal` will contain a `list` where its elements are the `micro` F1 score on the feature set `fs1` for thermal comfort prediction.

- The models used were not saved. However, the hyperparameters were fixed: Random Forest was used with the default parameters and `n_estimators = 1000`. Throughout all notebooks, a `seed` of value `13` was used. 

- `modeling_functions.py` contains the defined functions used for training and evaluation purposes.

## Results

All the figures in the paper can be reproduced with notebooks inside `publications-plots/`:
- `comfort-tiles.ipynb` reproduces Figure 4 
- `PublicationPlots_v1.ipynb` reproduces Figure 5 
- `PublicationPlots_v2.ipynb` reproduces Figure 7 
- `plots.ipynb` reproduces Figure 8 
- `PublicationPlots_v3.ipynb` reproduces Figure 9

## Contributing

MIT License
