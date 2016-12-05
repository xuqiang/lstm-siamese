# 10807 Project

## Preparation

### Compile HTK

[HTK](htk.eng.cam.ac.uk) is used to create MFCC features. To compile it, run `./compile_htk.sh`

### Create symbolic link for TIMIT.

Run `./link_timit.sh` to create a symbolic link for TIMIT dataset under `/results`.

## Debug MFCC feature

`debug_mfcc` has files for comparing MFCC features generated by HTK and that by [python_speech_features
](https://github.com/jameslyons/python_speech_features).

Only one file is used to do this test.
It's copied from `TIMITcorpus/TIMIT/TRAIN/DR8/FBCG1/SX442.*`, where `*` are `PHN`, `TXT`, `WAV`, `WRD`.
`.wav.sox` is generated using `sox SX442.WAV SX442.wav`,
as seen in `/home/zhihaol/807/scripts/convert_wav.sh` on CNBC cluster, and I renamed it to `.wav.sox`.

I will generate features according to Section 5.1 of
[Supervised Sequence Labelling with Recurrent Neural Networks](http://dx.doi.org/10.1007/978-3-642-24797-2),
a preprint version is at <http://www.cs.toronto.edu/~graves/preprint.pdf>.

### feature configuration for HTK

It's based on 3.1.5 "Step 5 - Coding the Data" of HTK official book (`htkbook-3.5.alpha-1.pdf`)

### load HTK feature

It's done using <https://github.com/cmusphinx/sphinxtrain/blob/master/python/cmusphinx/htkmfc.py>

## Feature generation

Run `PYTHONPATH=$(pwd) python feature_generation/generate_mfcc_features.py` under project root to see the results.

It will generate `/results/features/TIMIT_train.hdf5` and `/results/features/TIMIT_test.hdf5`,
as well as a folder `/results/features/TIMIT_train` for training data of `ctc_example`.

## CTC Training

### `ctc_example`

It's from <https://github.com/dresen/tensorflow_CTC_example>.

I modified `INPUT_PATH` & `TARGET_PATH`, and minibatch size to 10 in `bdlstm_train.py`,
and `load_batched_data` in `utils.py`.

Run `PYTHONPATH=$(pwd) python ctc_example/bdlstm_train.py` under project root to see the results.
By 10 epochs, it should give error rate of about 0.4.

### `ctc_example_2`

It's from <http://file.ppwwyyxx.com/>