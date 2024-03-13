# Hounsfield Inference Instructions
For the hounsfield project, we want to externally validate our supposedly working UNet segmentation model on your own internal data to see whether we are getting accurate Hounfield unit measurements. 
## Download model / file code
Clone the code and folder structure. https://github.com/mjayasur/hounsfield_prediction
## Test Data Setup
### Neuroimaging Informatics Technology Initiative (NIFTI)
All test images should first be turned into nii.gz files, which is a file storage format for neuroimaging files. Here is a package to do this conversion from DICOM: https://github.com/rordenlab/dcm2niix?tab=readme-ov-file. It should work on a folder with the DICOM files and create a new folder with NIFTI files.
### Test Dataset Structure
Our test data needs to be organized in the following way:
```
nnUNet/nnUNet_raw/
├── Dataset056_Spine
    └── imagesTs
         ├── VERSE_01_0000.nii.gz
         ├── VERSE_02_0000.nii.gz
             ...
             ...
         └── VERSE_20_0000.nii.gz
```
All of the data needs to be moved to nnUNet/nnUNet_raw/Dataset056_Spine/imagesTs for inference. 

Each case needs to be renamed in the above way.

nnUNet requires each case to start with a unique identifier (for our project we are using VERSE), then we have a 2 digit case ID (just a number representing identifying each CT scan), then a 4 digit modality identifier (for us just 0000, this is only if we have many modalities for each case such as for MRI we would have T1 and T2. For us, we are using CT scan which this is irrelevant).
## Install nnUNet
[From their github!](https://github.com/MIC-DKFZ/nnUNet/blob/master/documentation/installation_instructions.md#installation-instructions)

We strongly recommend that you install nnU-Net in a virtual environment! Pip or anaconda are both fine. If you choose to compile PyTorch from source (see below), you will need to use conda instead of pip.

Use a recent version of Python! 3.9 or newer is guaranteed to work!

**nnU-Net v2 can coexist with nnU-Net v1! Both can be installed at the same time.**

1.  Install  [PyTorch](https://pytorch.org/get-started/locally/)  as described on their website (conda/pip). Please install the latest version with support for your hardware (cuda, mps, cpu).  **DO NOT JUST  `pip install nnunetv2`  WITHOUT PROPERLY INSTALLING PYTORCH FIRST**. 
2.  Install nnUNet with this command:
			`pip install nnunetv2`


## Inference with nnUNet
Now we can actually run the trained model on the test data.
### Set nnUNet environment variables
Run this command so that nnUNet knows where the trained models are:
        `export nnUNet_results="/path/to/hounsfield_prediction/nnUNet/nnUNet_results"`
Replace the "path/to/" with the actual path to the hounsfield_prediction directory that you cloned from Github.

Important: These variables will be deleted if you close your terminal! They will also only apply to the current terminal window and DO NOT transfer to other terminals!

### Run segmentation model inference
Use the following command to run inference from within this folder.
``nnUNetv2_predict -i nnUNet/nnUNet_raw/Dataset056_Spine/imagesTs -o nnUNet/nnUNet_raw/Dataset056_Spine/predictionsTs -d Dataset056_Spine -c 3d_fullres --save_probabilities -f all``

### Get Hounsfield Units
To visualize the results for a single test case, use the jupyter notebook called "viz.ipynb" in the base nnUNet directory you downloaded.
