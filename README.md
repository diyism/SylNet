    $ conda create -n env_py36 python=3.6
    $ conda activate env_py36
    $ pip install tensorflow==1.10.1 librosa==0.7.2 numba==0.48.0 resampy==0.2.2 matplotlib==3.3.4
    $ python run_SylNet.py ../4.wav results.txt
    $ cat results.txt
    12
    All syllables value>0.5:
    pred_flat[0] 0.9982522130012512
    pred_flat[1] 0.9964203834533691
    pred_flat[2] 0.9990008473396301
    pred_flat[3] 0.998211145401001
    pred_flat[4] 0.9964654445648193
    pred_flat[5] 0.994257926940918
    pred_flat[6] 0.9941987991333008
    pred_flat[7] 0.9877486824989319
    pred_flat[8] 0.9765713214874268
    pred_flat[9] 0.9592069387435913
    pred_flat[10] 0.9169342517852783
    pred_flat[11] 0.6548179388046265
    pred_flat[12] 0.4458369314670563
    pred_flat[13] 0.15587027370929718
    pred_flat[14] 0.05832458660006523
    pred_flat[15] 0.04668596759438515
    pred_flat[16] 0.020574377849698067
    pred_flat[17] 0.010930686257779598
    pred_flat[18] 0.006763162091374397
    pred_flat[19] 0.007162218913435936
    pred_flat[20] 0.0069225081242620945
    pred_flat[21] 0.0064773885533213615
    pred_flat[22] 0.004590598400682211
    可以非常准确地得到音节的峰值帧, 但没有记录原始音频中的帧序号, 要不然 前后0.1秒 可以粗略作为音节范围

# SylNet

INTRODUCTION
------------
Tensorflow (Python) implementation of a Syllable Count estimator using gated Convolutional Neural Network (CNN) model with Gated activations, Residual connections, dilations and PostNets and an LSTM.

Comments/questions are welcome! Please contact: shreyas.seshadri@aalto.fi

Please cite the following paper whenever using these codes, their derivatives, or another
implementation of SylNet in a publication:

Seshadri S. & Räsänen O. SylNet: An Adaptable End-to-End Syllable Count Estimator for Speech. IEEE Signal Processing Letters, 26, 1359–1363, 2019.


LICENSE
-------

Copyright (C) 2019 Shreyas Seshadri, Aalto University

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
the Software, and to permit persons to whom the Software is furnished to do so,
subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

The source code must be referenced when used in a published work.

FILES AND FUNCTIONS – A QUICK GUIDE:
-------------------
train_SylNet.py - Train main SylNet model (pre-trained model available in ./trained_models/). Files ./config_files/config_files.txt and ./config_files/config_sylls.txt contain the paths to .wav sound files and number of syllables for each .wav file, respectively.

adap_SylNet.py - Adapt main SylNet model. Files ./config_files/config_files_adap.txt and ./config_files/config_sylls_adap.txt contain the paths to .wav sound files and number of syllables for each .wav file, respectively.

validate_SylNet.py	- Test trained model. Files ./config_files/config_files_test.txt and ./config_files/config_sylls_test.txt contain the paths to .wav sound files and number of syllables for each .wav file, respectively.

run_SylNet.py	- Run trained model to get syllable counts. Files ./config_files/config_files_run.txt contain the paths to .wav sound files. The files results.txt will contain predicted the syllable counts for the .wav files in ./config_files/config_files_run.txt.

run_SylNet_adapted.py - Same as above, but uses the adapted model that resulted from adap_SylNet.py


USING THE PRE-TRAINED MODEL TO SYLLABIFY SPEECH DATA
-------------------

SYNTAX:
python run_SylNet.py <input_files> <result_file>

where <input_files> can be:
  1) A path to a single .wav file, e.g.:

      python run_SylNet.py /path_to/audiofile.wav results.txt

  2) A path to a folder with .wav files (to process all the .wavs), e.g.:

      python run_SylNet.py /path_to/my_audiofiles/ results.txt

  3) A path to a .txt file where each row of the text file contains a path to a
      .wav file to be processed, e.g.:

      python run_SylNet.py /path_to/my_filepointers/files_to_process.txt results.txt

By default (without any input arguments), run_SylNet.py will attempt to load files
specified in config_files/config_files_run.txt and stores the results in
results.txt located in the main directory of SylNet.

OUTPUTS:
  Result file (e.g., results.txt) will contain an integer number corresponding to
  the estimated syllable count, one number per row corresponding to each input file
  provided to the algorithm.

  If a folder path with .wavs in it was provided as the input, a separate output file
  of form <resultfilename_without_extension>_files.txt with processed audio filenames  
  will be produced, allowing mapping of output counts to input files.

ADAPTED MODELS:

Use run_SylNet_adapted.py to perform syllable counting with an adapted model.
Otherwise syntax is the same.

NOTE:

The current implementation of SylNet supports maximum output syllable count of 91 syllables per input signal. If your input data is suspected to contain more than 91 syllables (approx. 18 seconds of speech), consider dividing the data into shorter segments before applying SylNet.


REQUIRED PACKAGES
-------------------
- Python 3 (tested on 3.6.5)
- LibROSA (tested with 0.6.3, and 0.7, https://librosa.github.io/librosa/)
- TensorFlow (tested on version 1.10.1). Does NOT currently work on Tensorflow 2.1. 
- SciPy
- Numpy

REFERENCES
---------
[1] Seshadri, S. & Räsänen, O. (2019). SylNet: An Adaptable End-to-End Syllable Count Estimator for Speech. IEEE Signal Processing Letters, 26, 1359–1363.
