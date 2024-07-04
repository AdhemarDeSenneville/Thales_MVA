# Speech Denoising

[**Code**](./code/Supervised_Audio_Separation.ipynb)

# Work Overview
As part of the course by T. COURTAT on [Deep Learning and Signal Processing](https://www.master-mva.com/cours/apprentissage-profond-et-traitement-du-signal-introduction-et-applications-industrielles/), we undertook a project focused on audio separation. More precisely, the objective was to explore speech denoising techniques to achieve the best results on a single source, single speaker audio dataset.

We decided to concentrate more on the model used for denoising. In the first part, we studied classical methods to understand which blocks are built upon the more advanced deep learning techniques studied in the second part.

## Deliverables
- A fully executed notebook containing all results.
- An oral presentation, utilizing the notebook as a visual aid.


# Denoising Models Overview
Below is a list of all the models tested in the notebook:

### 1. **Butterworth Filter** (Classical)
The Butterworth filter is a type of digital filter favored for noise reduction due to its flat passband response, which ensures that all frequencies within the passband are treated equally. If the noise is considered high frequency and the voice low frequency, we can apply a filter to reduce the noise. This strategy serves as a baseline for comparing more sophisticated denoising techniques.

### 2. **Wiener Filter** (Classical)
The Wiener filter is designed to minimize the mean square error between the estimated and actual input signals. It computes the filter $H(f)$ by the formula:
$$H(f) = \frac{S_{xx}(f)}{S_{xx}(f) + S_{nn}(f)}$$
where $S_{xx}(f)$ and $S_{nn}(f)$ are the power spectral densities of the signal and noise, respectively. Training a Wiener filter involves estimating these densities.

### 3. **WaveUNet** (Deep Learning)
WaveUNet adapts the U-Net architecture to process audio signals in the time domain, allowing it to directly model and modify the waveform. This model is particularly good at capturing both local and contextual information within a signal.

![](https://raw.githubusercontent.com/AdhemarDeSenneville/Thales_MVA/main/fig/waveUnet.wav)
![](https://raw.githubusercontent.com/AdhemarDeSenneville/Thales_MVA/main/fig/WaveUNet.png)

### 4. **Conv-TasNet** (Deep Learning)
Conv-TasNet employs a linear encoder to create an optimized representation of the speech waveform for separating individual speakers. This is accomplished by applying weighting functions (masks) to the encoder output. The modified encoder representations are then reverted back into waveforms using a linear decoder. The masks are derived using a temporal convolutional network (TCN), consisting of stacked 1-D dilated convolutional blocks, which captures long-term dependencies in the speech signal while maintaining a compact model size.

![](https://raw.githubusercontent.com/AdhemarDeSenneville/Thales_MVA/main/fig/ConvTasNet.wav)
![](https://raw.githubusercontent.com/AdhemarDeSenneville/Thales_MVA/main/fig/ConvTasNet.png)

### 5. **HybridDemucs** (Deep Learning)
HybridDemucs combines the U-Net architecture inspired by Wave-U-Net with GLUs and a BiLSTM layer between the encoder and decoder. It performs hybrid source separation, deciding which domain (time or frequency) is best suited for each source or combining both. This hybrid version of the Demucs architecture was the winner of the Music Demixing Challenge 2021 organized by Sony.

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

You can listen to the output audios at the end of the notebook.

## Authors
- Adhémar de Senneville (MVA)
- Adib Habbou (MVA)

## Credits

**Wave-U-Net**: [Wave-U-Net: A Multi-Scale Neural Network for End-To-End Audio Source Separation](https://arxiv.org/pdf/1806.03185)

**Conv-TasNet**: [Conv-TasNet: Surpassing Ideal Time-Frequency Magnitude Masking for Speech Separation](https://arxiv.org/pdf/1809.07454)

**HybridDemucs**: [Hybrid Spectrogram and Waveform Source Separation](https://arxiv.org/pdf/2111.03600)
