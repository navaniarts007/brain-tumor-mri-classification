# Brain Tumor MRI Image Classification

Deep-learning project for classifying brain MRI images into four classes:

- `glioma`
- `meningioma`
- `no_tumor`
- `pituitary`

The project includes data preprocessing, a custom CNN, a ResNet50 transfer-learning
model, model evaluation, and a Streamlit web application for predictions.

> **Medical notice:** This project is for learning and research only. It is not a
> medical diagnostic tool and must not replace advice from a qualified clinician.

## Project structure

```text
.
├── app.py                         # Streamlit prediction application
├── data/
│   ├── train/                     # Training images grouped by class
│   ├── valid/                     # Validation images grouped by class
│   └── test/                      # Test images grouped by class
├── models/                        # Generated .h5 models and training history
├── outputs/                       # Generated evaluation plots
├── scripts/
│   ├── data_preprocessing.py      # Image generators and preprocessing helpers
│   ├── model_training.py          # Train and save both models
│   ├── model_evaluation.py        # Reports and plots for trained models
│   └── utils.py
├── requirements.txt
└── README.md
```

## Setup on a new Windows laptop

### 1. Install prerequisites

Install the following:

- Python **3.10 or 3.11** (64-bit)
- Git
- Optional: VS Code

Check that Python and Git are available:

```powershell
python --version
git --version
```

### 2. Clone the repository

```powershell
git clone https://github.com/<your-github-username>/brain-tumor-mri-classification.git
cd brain-tumor-mri-classification
```

### 3. Create and activate a virtual environment

Run these commands from the project folder:

```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

If PowerShell blocks activation, run PowerShell once as your user with:

```powershell
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

Then activate the environment again:

```powershell
.\venv\Scripts\Activate.ps1
```

### 4. Install dependencies

Upgrade `pip`, then install the pinned packages:

```powershell
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

The first TensorFlow installation can take several minutes. Keep the virtual
environment activated whenever you run this project.

## Dataset layout

The repository contains the dataset in the following format:

```text
data/
├── train/
│   ├── glioma/
│   ├── meningioma/
│   ├── no_tumor/
│   └── pituitary/
├── valid/
│   ├── glioma/
│   ├── meningioma/
│   ├── no_tumor/
│   └── pituitary/
└── test/
    ├── glioma/
    ├── meningioma/
    ├── no_tumor/
    └── pituitary/
```

If you replace the dataset, preserve these folder names and split folders.
Images are resized to `224 x 224` during preprocessing.

## Train the models

From the project root, with the virtual environment activated:

```powershell
python scripts\model_training.py
```

This trains both the custom CNN and the frozen-ResNet50 transfer-learning model.
It creates:

- `models\custom_cnn.h5`
- `models\transfer_learning_model.h5`
- `models\custom_cnn_history.pkl`
- `models\transfer_learning_model_history.pkl`

Training downloads ResNet50 ImageNet weights if they are not already cached, so
an internet connection may be required the first time.

## Evaluate the models

After training:

```powershell
python -m scripts.model_evaluation
```

Evaluation prints classification reports and creates:

- `outputs\confusion_matrix.png`
- `outputs\training_history.png`

## Run the Streamlit application

The app requires `models\transfer_learning_model.h5`. Train the models first, or
copy a compatible trained model into the `models` directory.

```powershell
python -m streamlit run app.py
```

Open the local URL shown by Streamlit, usually
`http://localhost:8501`, upload a JPG or PNG MRI image, and review the predicted
class and confidence score.

Stop the app with `Ctrl+C`.

## Troubleshooting

- **`ModuleNotFoundError`**: confirm that `venv` is activated and rerun
  `python -m pip install -r requirements.txt`.
- **Model file not found**: run the training command before starting Streamlit.
- **PowerShell activation error**: apply the `Set-ExecutionPolicy` command above.
- **Out-of-memory errors**: close other applications and reduce the batch size in
  `scripts\model_training.py` from `16` to a smaller value such as `8`.

## License and data

Review the source dataset's license and attribution requirements before
redistributing the images or trained models.
