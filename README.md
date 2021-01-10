# lstm-cnn-eeg-sleep-staging

## LSTM-CNN Based EEG Sleep Staging Algorithm

This project uses a hybrid LSTM-CNN architecture to classify sleep stages based on two channel EEG data. The EEG data is segmented into 5 pairs of 30 second blocks, and a spectrogram is computed on each 30 second block. Each of the 5 blocks is then classified as either wake, NREM, or REM. 

The data used for this project is from the Sleep-EDF Database:
https://archive.physionet.org/physiobank/database/sleep-edfx/

The following diagram is a high level description of the model architecture:
![modelarchitecture](https://github.com/nerajbobra/lstm-cnn-eeg-sleep-staging/blob/main/plots/block_diagram.jpg "Model Architecture")

The remainder of this readme will cover the different steps in the analysis pipeline.

## 1. Download/Parse the Data
The RECORDS.txt file contains a list of every file in the database, all of which belong to the same root URL. After downloading each file, the pyedflib library is used to parse the EDF files in the database. As an example, this is the data from file ST7152JA:
![datapreview](https://github.com/nerajbobra/lstm-cnn-eeg-sleep-staging/blob/main/plots/parsed_data.png "Data Preview")

## 2. Generate Spectrograms
Each file is now segmented into five thirty-second consecutive blocks, and then spectrograms of each 30 second block are computed. An example spectrogram output from file SC4121EC:
![spectrogram](https://github.com/nerajbobra/lstm-cnn-eeg-sleep-staging/blob/main/plots/spectrogram.png "Spectrogram")

## 3. Train the Model
For training, a validation split of 10% was used and an early stopping criterion was implemented based on the validation loss. The loss and categorical accuracy over the training session:
![loss](https://github.com/nerajbobra/lstm-cnn-eeg-sleep-staging/blob/main/plots/loss.png "Loss")
![accuracy](https://github.com/nerajbobra/lstm-cnn-eeg-sleep-staging/blob/main/plots/accuracy.png "Categorical Accuracy")

## 4. Evaluate the Model
20% of the data was used for the test set. 

The confusion matrix:
|      | Wake  | NREM  | REM  |
|:----:|-------|-------|------|
| Wake | 57820 | 769   | 318  |
| NREM | 1481  | 23989 | 1880 |
| REM  | 281   | 1443  | 4834 |

A per-class f1 score:
|   Class              |  f1 Score  |
|-------------------------|--------------------|
|   Wake                   |   0.976                |
|   NREM                |   0.896                |
|   REM                |   0.711                |

For file ST7152JA, the computed output hypnogram vs the annotated labels (note that the time here is relative, not absolute):
![hypnogram](https://github.com/nerajbobra/lstm-cnn-eeg-sleep-staging/blob/main/plots/SC4121EC_hypnogram.png "Hypnogram")

## References
https://pubmed.ncbi.nlm.nih.gov/30445569/
<br />https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0216456
