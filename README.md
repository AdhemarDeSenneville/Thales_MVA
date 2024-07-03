
# Authors : 
- SENNEVILLE Adhemar (MVA)
- HABBOU Adib (MVA)

# Work overview
As part of the class of T. COURTAT on [Deep learning and signal processing](https://www.master-mva.com/cours/apprentissage-profond-et-traitement-du-signal-introduction-et-applications-industrielles/), we had a project on audio separation. More precisely the objectif was to explore suprvised audio separation technics to get the best results on a single source, sigle speaker audio dataset.

We decided to focus more on the model used for denoizing. In the first part, we studied classical methods to undestand on witch bloc are build the more advanced deep learning technicn studied in the second part.

## Deliverables
- A fully executed notebook containing all results.
- An oral presentation, utilizing the notebook as a visual aid.

# Denoising Models Overview
Below is a list of all the models tested in the notebook:


### 1. **Biquad Filter** (Classical)
A Biquad filter is a type of digital filter that provides a simple, efficient way to implement standard filtering operations.


### 2. **Wiener Filter** (Classical)
The Wiener filter is a classical signal processing technique used for noise reduction and signal restoration. It operates based on the statistical characteristics of the signal and the noise, attempting to minimize the mean square error between the estimated and the true signal. In audio processing, it's typically used to remove static background noise by smoothing out changes that don't correspond to the underlying desired signals. It requires assumptions about the spectral properties of the signal and noise to function effectively.


### 3. **WaveUNet** (Deep Learning)
WaveUNet is a convolutional neural network model that adapts the U-Net architecture. It processes audio signals in the time domain, allowing it to directly model and modify the waveform. This model is known for its ability to capture both local and contextual information in a signal.

![](https://raw.githubusercontent.com/AdhemarDeSenneville/Thales_MVA/main/fig/waveUnet.wav)
![](https://raw.githubusercontent.com/AdhemarDeSenneville/Thales_MVA/main/fig/WaveUNet.png)

### 4. **Conv-TasNet** (Deep Learning)
Conv-TasNet uses a linear encoder to generate a representation of the speech waveform optimized for separating individual speakers. Speaker separation is achieved by applying a set of weighting functions (masks) to the encoder output. The modified encoder representations are then inverted back to the waveforms using a linear decoder. The masks are found using a temporal convolutional network (TCN) consisting of stacked 1-D dilated convolutional blocks, which allows the network to model the long-term dependencies of the speech signal while maintaining a small model size.

![](https://raw.githubusercontent.com/AdhemarDeSenneville/Thales_MVA/main/fig/ConvTasNet.wav)
![](https://raw.githubusercontent.com/AdhemarDeSenneville/Thales_MVA/main/fig/ConvTasNet.png)

### 5. **HybridDemucs** (Deep Learning)
Demucs is based on a U-Net convolutional architecture inspired by Wave-U-Net with GLUs, a BiLSTM between the encoder and decoder. HybridDemucs perform hybrid source separation, and decide which domain is best suited for each source (or combining both). The proposed hybrid version of the Demucs architecture won the Music Demixing Challenge 2021 organized by Sony.

![](https://raw.githubusercontent.com/AdhemarDeSenneville/Thales_MVA/main/fig/HybridDemucs.wav)
![](https://raw.githubusercontent.com/AdhemarDeSenneville/Thales_MVA/main/fig/HDemucs.png)

# Results

|                    | MSE Loss   |       SDR |    PESQ |     STOI | Number of Parameters   | Inference Time   |
|--------------------|------------|-----------|---------|----------|------------------------|------------------|
| Wiener Filter      | 2.18 e-04  | -10.6569  | 2.08805 | 0.80397  | 1                      | 4 ms             |
| Butterworth Filter | 2.23 e-04  |  -3.7573  | 1.57805 | 0.895725 | 4                      | 7 ms             |
| Dumb Model         | 7.76 e-03  | -55.7942  | 1.20344 | 0.001805 | 400 K                  | 3 ms             |
| WaveUNet Model     | 7.89 e-06  |   6.72842 | 2.93885 | 0.6882   | 10.1 M                 | 74 ms            |
| ConvTasnet Model   | 4.88 e-06  |   5.8216  | 2.31814 | 0.701365 | 2.5 M                  | 36 ms            |
| HybridDemucs Model | 2.67 e-06  |   5.06163 | 1.97447 | 0.787565 | 2.5 M                  | 21 ms            |

# Credit

**Wave-U-Net**: [Wave-U-Net: A Multi-Scale Neural Network For
End-To-End Audio Source Separation](https://arxiv.org/pdf/1806.03185)

**Conv-TasNet**: [Conv-TasNet: Surpassing Ideal Time-Frequency
Magnitude Masking for Speech Separation](https://arxiv.org/pdf/1809.07454)

**HybridDemucs**: [Hybrid Spectrogram and Waveform Source Separation](https://arxiv.org/pdf/2111.03600)
