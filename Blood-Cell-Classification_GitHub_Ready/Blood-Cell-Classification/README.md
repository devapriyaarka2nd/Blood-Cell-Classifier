# Blood Cell Classification — CNN and EfficientNetB3

A Jupyter notebook comparing a custom convolutional neural network with ImageNet-initialized EfficientNetB3 for blood-cell image classification. It includes class distribution plots, sample images, training curves, evaluation, confusion matrices, and per-class classification reports.

The uploaded notebook was named `bloodcell-cancer-detection.ipynb`, but its labels identify cell types, not cancer status. This repository uses a name that reflects the implemented task.

## Dataset and classes

The supplied code identifies the Kaggle dataset as [`unclesamulus/blood-cells-image-dataset`](https://www.kaggle.com/datasets/unclesamulus/blood-cells-image-dataset). This is the source identifier recorded in the notebook; the dataset page and license have not been independently verified during repository preparation.

The supplied run contains these six retained classes:

| Class folder | Cell type |
| --- | --- |
| `basophil` | Basophil |
| `eosinophil` | Eosinophil |
| `erythroblast` | Erythroblast |
| `lymphocyte` | Lymphocyte |
| `monocyte` | Monocyte |
| `platelet` | Platelet |

Folders named `ig` and `neutrophil` are excluded, preserving the original experiment. Labels are inferred from folder names, and the output layer uses the number of classes found. Supply the six folders above to reproduce the intended task; additional image folders will become additional classes.

Dataset files and trained weights are not included. Verify the dataset's permitted use and citation requirements before redistribution.

## Repository contents

| Path | Purpose |
| --- | --- |
| `blood-cell-classification.ipynb` | Complete classification workflow |
| `requirements.txt` | Python dependencies |
| `data/README.md` | Local dataset layout |
| `.gitignore` | Excludes datasets, environments, model artifacts, and checkpoints |

## Setup

Create a virtual environment:

```bash
python -m venv .venv
```

Activate it on Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

Or on macOS/Linux:

```bash
source .venv/bin/activate
```

Install dependencies and start Jupyter from the repository root:

```bash
python -m pip install -r requirements.txt
python -m jupyter lab
```

Open `blood-cell-classification.ipynb` and run cells in order. Dependencies are unpinned because the original environment versions were not provided. Select a Python version compatible with the TensorFlow release you install. Installation and full training have not been verified for this prepared package.

The workflow retains the original `ImageDataGenerator` interface. Compatibility with your installed TensorFlow/Keras environment should be checked before a full run.

## Dataset options

**Automatic download:** Leave `BLOOD_CELL_DATA_DIR` unset. The notebook calls `kagglehub.dataset_download` using the recorded dataset identifier and uses the returned directory. Download access depends on your environment and Kaggle access requirements.

**Existing local dataset:** Point `BLOOD_CELL_DATA_DIR` at the folder containing the class folders, or its parent if it contains `bloodcells_dataset/`.

Windows PowerShell:

```powershell
$env:BLOOD_CELL_DATA_DIR = "C:\datasets\bloodcells_dataset"
python -m jupyter lab
```

macOS/Linux:

```bash
export BLOOD_CELL_DATA_DIR=/path/to/bloodcells_dataset
python -m jupyter lab
```

See `data/README.md` for supported image extensions and folder structure. In Kaggle or Colab, set the same environment variable in an early code cell to use an attached or mounted dataset. If it remains unset, the automatic download path is used.

EfficientNetB3 requests ImageNet weights. The first run needs download access unless those weights are already cached. Both models load RGB images at 224 × 224. A GPU can reduce training time, but the notebook does not explicitly require one.

## Models and training

| Setting | Custom CNN | EfficientNetB3 |
| --- | --- | --- |
| Image size | 224 × 224 RGB | 224 × 224 RGB |
| Batch size | 16 | 16 |
| Epochs | 5 | 10 |
| Optimizer | Adamax | Adamax |
| Learning rate | 0.001 | 0.001 |
| Loss | Categorical cross-entropy | Categorical cross-entropy |
| Initialization | Random | ImageNet backbone weights |

The custom CNN uses input rescaling, three convolution/pooling blocks with 32, 64, and 128 filters, flattening, dropout of 0.2, a 128-unit dense layer, and a softmax output.

EfficientNetB3 uses max pooling, batch normalization, a regularized 256-unit dense layer, dropout of 0.45, and a softmax output. Its backbone is trainable because the supplied freezing instruction is commented out. Its Keras preprocessing is part of the backbone; the image generators do not add a second rescaling step.

The second model overwrites the notebook variables `model` and `history`. The workflow displays results but does not save either trained model. Add a save step after each training stage if you need persistent weights.

## Evaluation and limitations

Two image-level splits produce approximately 80% training, 10% validation, and 10% testing, using `random_state=43`. Test generators use `shuffle=False` for prediction alignment.

The prepared evaluation cells allow Keras to traverse each generator once. The original code computed a step count from a separate batch-size calculation that was never applied to the generators. That mismatch has been removed.

Splits are not stratified and do not group images by patient or donor. Patient-level independence cannot be established from this notebook. Only the split seed is fixed; model initialization and generator randomness are not fully seeded. The two models also use different training budgets, so their results are not a controlled architecture comparison.

No new performance figures are reported here: saved outputs were cleared and training was not rerun. This is an experimental cell-type classification workflow; clinical or cancer-detection suitability has not been established.

## Repository preparation changes

- Removed saved outputs, execution counts, Kaggle-specific metadata, unused imports, and the duplicated EfficientNet confusion-matrix cell.
- Replaced the hardcoded Kaggle path with the actual download location or an environment-variable override.
- Added directory checks, supported-image filtering, sorted discovery, and an empty-dataset error. Sorted discovery may change split membership relative to the original run.
- Corrected generator evaluation, reset the test generator before prediction, and used explicit label order in metric reports.
- Applied `tight_layout()` in training plots and made label-count column names explicit.
- Added setup instructions, dataset documentation, and Git exclusion rules.

Architectures, optimizer settings, epochs, split proportions, excluded folders, and backbone trainability were preserved. Code-cell syntax, dataset override resolution, archive integrity, and Git exclusion rules were checked. Full training and dependency installation were not run.

## Publish as a separate GitHub repository

Suggested name: `Blood-Cell-Classification`.

Suggested description: `Blood-cell image classification with a custom CNN and EfficientNetB3 using TensorFlow.`

Create an empty repository on GitHub. Extract this package and upload the contents inside `Blood-Cell-Classification/`, including `.gitignore`, so `README.md` and the notebook appear at the repository root. Commit the files. Upload the extracted files rather than the ZIP itself.

Alternatively, run these commands from the extracted folder:

```bash
git init
git add .
git commit -m "Add blood-cell classification notebook and setup documentation"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/Blood-Cell-Classification.git
git push -u origin main
```

Replace `YOUR_USERNAME` with the repository owner. No GitHub repository has been created by this package. No code license has been selected; choose one after confirming ownership and applicable source terms.
