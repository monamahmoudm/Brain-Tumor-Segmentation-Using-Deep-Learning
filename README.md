# Brain-Tumor-Segmentation-Using-Deep-Learning

### Abstract
Brain tumor segmentation has been one of the most critical topics ever due to the danger it imposes on an individual’s life. As with any other disease, the earlier it is diagnosed, the better the prophylactic measures that can be taken. Applying such a model would allow for a quick glance at the magnetic resonance imaging (MRI) to produce a decision. This would, in turn, reduce time within the decision-making process, allowing for more accurate and precise decisions helping doctors save more patients’ lives. This paper discusses various deep learning techniques by which tumor segmentation could occur, including convolutional neural networks (CNN), enhanced convolutional neural networks (ECNN), and EnsembleNet.

Keywords: Segmentation, Brain Tumor, MRI, CNN, ECNN, EnsembleNet, Deep Learning.

---

### Introduction
“A brain tumor is an abnormal growth or mass of cells in or around your brain”. (Cleveland Clinic, 2022) Tumors are divided into malignant (cancerous) and benign (non-cancerous), where about one-third of the brain tumors are malignant. However, in both cases, it affects the functions of the brain and one’s health if it grows large, pressing on surrounding nerves, blood vessels, and tissues. (Cleveland Clinic, 2022) This, in turn, gives rise to the importance of identifying a brain tumor early on to avoid its complications. Deep learning techniques have been introduced to produce quicker segmentation, providing more accurate and robust outputs. This paper aims to survey the different techniques present to solve the segmentation problem, develop the best model to carry the problem further and update the model to achieve better results. The motivation behind this is to reduce the time taken to produce an accurate diagnosis free from human error. Hence, this model takes the MRI images as an input (Figure 1) and outputs the MRI with the colorated tumor region (Figure 2). (Cleveland Clinic, 2022)

---

### Chosen Dataset
We plan on using the BRATS-2020 dataset, it can be found [here](https://www.kaggle.com/datasets/awsaf49/brats2020-training-data). The Brain Tumor Segmentation (BraTS) 2020 dataset is a collection of multimodal Magnetic Resonance Imaging (MRI) scans used for segmenting brain tumors. The full dataset is inaccessible due to being part of competitions conducted previously; however, we were able to obtain a version that has most of the data, with a few missing data entries that we can deal with.

This dataset consists of 369 Brain MRI Images. The dataset file contains 57195 images and four primary files: BraTS20 Training Metadata, survival_info, name_mapping, and meta_data. The images map out to MRI scans of slices of the brain of 369 patients, saved as .h5 files. Every 155 image slice reflects one patient; therefore, 155 slices * 369 patients give 57195 images.

Our dataset consists of images from brain scans, as well as masked labels indicating regions of abnormal tissue. Each image has four channels:

1. **T1-weighted (T1):** This sequence produces a high-resolution image of the brain's anatomy. It is suitable for visualizing the brain's structure but less sensitive to tumor tissue than other sequences.
2. **T1-weighted post-contrast (T1c or T1Gd):** This sequence is useful for highlighting malignant tumors.
3. **T2-weighted (T2):** T2 images provide excellent contrast of the brain's fluid spaces and are sensitive to edema (swelling), which often surrounds tumors.
4. **Fluid Attenuated Inversion Recovery (FLAIR):** This sequence suppresses the fluid signal, making it easier to see peritumoral edema (swelling around the tumor).

The masks represent segmentations, containing areas of interest within the brain scans. They specifically focus on abnormal tissue related to brain tumors. Each mask has three channels:

- **Necrotic and Non-Enhancing Tumor Core (NCR/NET):** Masks out the necrotic (dead) part of the tumor, which includes healthy brain tissue or non-tumor regions.
- **Edema (ED):** Masks the edema, swelling, or fluid accumulation around the tumor.
- **Enhancing Tumor (ET):** Masks out the enhancing tumor, which is the tumor region that shows uptake of contrast material and is often considered the most aggressive part of the tumor.

The BraTS20 Training Metadata file contains 57195 data points and eight variables:

- `Slice_path`
- `Target`
- `Volume`
- `Slice`
- `Label0_pxl_cnt`
- `Label1_pxl_cnt`
- `Label2_pxl_cnt`
- `Background_ratio`

The `slice_path` reflects the name and file path of the image inside the dataset. The `target` shows 0 or 1 depending on whether or not the brain is tumorous. 158 patients are labeled with tumors, while 211 are labeled non-tumorous. The `volume` refers to the 3D MRI volume (or the patient scan) from which the 2D slice was extracted. Each volume contains multiple 2D slices, and the volume provides context for which patient or scan the slice belongs to. The `slice` indicates the slice number within the 3D volume. For instance, if a volume contains 100 slices, this could be slice number 45 out of 100, indicating its position in the series. 

- `Label0_pxl_cnt:` Not Tumor (NT) volume
- `Label1_pxl_cnt:` Edema (ED)
- `Label2_pxl_cnt:` Enhancing Tumor (ET)

The `background_ratio` is the ratio of pixels in the slice that belong to the background or areas that are not part of the tumor (such as normal brain tissue or other non-tumor regions).

The `name_mapping` consists of the Grade of the image, which is categorized as High-Grade Gliomas (HGG) or Low-Grade Gliomas (LGG), which exhibit different growth patterns and clinical impacts. This dataset has 77 LGG patients and 283 HGG patients. It also contains the image ID in the BraTS 2018, 2019, and 2020 in case the same image was used in previous datasets.

The `survival_info` file contains the Age, Survival days, and Extent of resection (an indicator of whether or not the patient will survive longer).

Lastly, the `meta_data` contains the slice path, target, volume, and slice, which are all redundant with the BraTS20 Training Metadata.

This dataset is ideal because it contains a large and diverse set of patients with varying tumor grades (HGG and LGG), which ensures that models trained on it can generalize better across different clinical settings. In addition, it is labeled and contains lots of data about the brain scans, which can help us map out the segmentation. Lastly, the dataset needs to have the problem of class imbalance, which will help us achieve a smooth modeling process.

Therefore, this dataset can support the development of high-accuracy models capable of assisting in diagnosis, treatment planning, and disease monitoring, making it invaluable for medical imaging research and real-world applications.
