# Fruit Image Classification Using ANN and CNN

## Overview

This project compares Artificial Neural Networks (ANN) and Convolutional Neural Networks (CNN) for classifying three types of fruit: Banana, Watermelon, and Salak. Each method is evaluated with and without data augmentation.

## Dataset

The main dataset is [Fruits-360 100×100](https://github.com/fruits-360/fruits-360-100x100), restricted to Banana 1, Watermelon 1, and Salak 1.

| Class | Main dataset | External images |
|---|---:|---:|
| Banana | 656 | 20 |
| Watermelon | 632 | 20 |
| Salak | 652 | 20 |
| Total | 1,940 | 60 |

| Data split | Images | Proportion of main dataset |
|---|---:|---:|
| Training | 1,164 | 60% |
| Validation | 291 | 15% |
| Testing | 485 | 25% |
| External | 60 | Separate dataset |

The original Fruits-360 training and test folders are preserved. The training folder is split into 80% training and 20% validation using stratified random sampling. External images are stored separately in `New-Fruits`; their individual sources and reuse permissions should be documented in [sources.csv](New-Fruits/sources.csv).

## Methodology

- Aspect-ratio-preserving resizing to 100 × 100 RGB pixels and normalization to [0, 1].
- Geometric, photometric, and synthetic background augmentation in the augmented training scenario only.
- Balanced class weights, batch normalization, and dropout for ANN and CNN; global average pooling for CNN.
- Adam optimizer (learning rate 0.001), batch size 32, maximum 80 epochs, early stopping with patience 4, and seed 2026.
- Architecture and epoch selection based on minimum validation loss.

## Results

| Model | Test accuracy | External accuracy | External macro F1 | Parameters | Training time (s) |
|---|---:|---:|---:|---:|---:|
| ANN Baseline | 100.00% | 60.00% | 0.5743 | 3,849,347 | 28.90 |
| ANN Augmented | 98.76% | 86.67% | 0.8704 | 3,849,347 | 17.61 |
| CNN Baseline | 100.00% | 71.67% | 0.7063 | 28,643 | 82.86 |
| CNN Augmented | 100.00% | 88.33% | 0.8863 | 28,643 | 111.33 |

CNN with augmentation achieved the highest observed external accuracy (88.33%, 53/60 images) and used fewer parameters than ANN, but required longer training. Augmentation improved external accuracy for both methods. Fruits-360 has predominantly uniform white backgrounds; synthetic augmentation does not replace real background diversity. Repeated evaluation on the same external sample is exploratory. Training times refer to the selected candidates in this experiment.

## Repository Structure

- [Jupyter Notebook](ANN_CNN_Final.ipynb): implementation, training logs, evaluation, and analysis.
- [Report](Report.pdf): complete experimental report.
- `fruits-360/`: selected original training and testing images.
- `New-Fruits/`: external evaluation images and source information.
- `output_ann_cnn/tables/`: data splits, tuning, evaluation, and predictions.
- `output_ann_cnn/figures/`: sample images, learning curves, confusion matrices, and external predictions.
- `output_ann_cnn/models/`: four selected trained models in Keras format.
- `output_ann_cnn/logs/`: training history, configuration, and package versions.

## Reproducing the Experiment

Use Python 3.12 and open a terminal in the repository root:

```bash
python -m pip install -r output_ann_cnn/logs/requirements.txt
python -m pip install jupyterlab
python -m jupyterlab
```

Open `ANN_CNN_Final_Revisi.ipynb`. Set `root_dir = Path.cwd()` in the preparation cell if JupyterLab is running from the repository root. Run all cells to reproduce the experiment. Runtime and numerical results may vary with hardware and software.

## References and Data License

- Oltean, M. (2017–present). [Fruits-360 dataset](https://github.com/fruits-360/fruits-360-100x100).
- Mureșan, H., & Oltean, M. (2018). Fruit recognition from images using deep learning. *Acta Universitatis Sapientiae, Informatica, 10*(1), 26–42. https://doi.org/10.2478/ausi-2018-0002

Fruits-360 images are licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). The repository uses a selected subset of the original dataset. External images retain their respective source licenses and are not covered by the Fruits-360 license.
