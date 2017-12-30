# WaveNet-Audio
Training and generating with a Tensorflow version of Wavenet for audio files

This work was part of the CADL project set to explore a tensorflow version of Wavenet. I explored the concepts of *transfer learning* and training a new model from scratch. Time permitting I'll train these models on AWS to take them to a better performance level. My local machine would only run on cpu this week, so training wasn't taken as far as desired.

### The Data
I chose to start with the open source [wavenet code by Igor Babuschkin](https://github.com/ibab/tensorflow-wavenet) which builds global conditioning. See his repository for details on how to train and generate using this model. This particular implementation trains on audio wav files and defaults to the  [VCTK corpus](http://homepages.inf.ed.ac.uk/jyamagis/page3/page58/page58.html) corpus of multiple speakers of English with varying accents.  

For transfer learning I chose to start with a model that was reasonably trained to 72000 steps [DiyuanLu](https://github.com/ibab/tensorflow-wavenet/files/1543096/2017-12-04T13-48-11.zip). The only change to the base model was setting the SILENCE_TRESHOLD = 0.1 from 0.3. I continued training this model and confirmed that the training method was working correctly by generating audio wav files as it trained. There was no noticable change in the quality of the reconstructions.

Next I downloaded [GTZAN music](https://www.kaggle.com/lnicalo/gtzan-musicspeech-collection) dataset consisting of 100 audio files x 10 musical genres for a total of 1000 files. Renaming of the files and converting them to .wav format was required for implementation. Genres include [blues, classical, country, disco, hiphop, jazz, metal, pop, reggae, rock].

### Transfer Learning with Conditioning
What I chose to explore for this section was how swapping out one of the speakers in the speech generation model with blues songs would effect the model. Most immediately: *My qualitative assessments in italics*
- How quickly would the the music effect the speech model? *Within several thousand iterations the speech model changed significantly*
- Relative change: Would replacing one speaker with music files change entire model (all speakers) as much as the speaker that was removed? *clearly the speaker that had music substituted for them was impacted faster and to a greater degree.*
- Would the the model incorporate new sounds from the music or simply degrade? *It seems that aspects of the blues music were making it into the model. Further training is required to confirm.  *


### Training a New Model
Next I trained a model from scratch extending what I did in the transfer learning section by removing all speakers and replacing them with wav files corresponding to music genres. The model was trained from scratch with conditioning. Output audio files can be generated without conditioning (general audio model) or in the style of a particular genre.  
adapted the GTZAN data set into the same format as the VCTK-Corpus which the Wavenet model I was using is hardcoded to. This required some tedious renaming, format conversions and organizing into the correct folder structures. 

Included in the [Generated-audio](https://github.com/giering/WaveNet-Audio/tree/master/Generated-audio) folder are examples generated without conditioning, with conditioning. I've also primed the conditioned audio files with assorted .wav files. The *quality of this model is pretty poor* due in part to the low number of training iterations. Feel welcome to make use of the model and data to extend the training.
