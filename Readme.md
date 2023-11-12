# Technical Document Advanced AI Project

# Table of Contents

# 1. Project Overview 📝

| Project Title | Lung diseases classification and localization with Deep Learning |
| --- | --- |
| Goal of the project | Implement different Deep Learning models with different architectures to classify and localize lung disease from chest X-rays and observe the performance of these different methods. |
| Dataset(s) used | ChestX-ray8 dataset form NIH Clinical Center :  https://nihcc.app.box.com/v/ChestXray-NIHCC |
| Team Members | Adrien Junior TCHUEM TCHUENTE , Landry Yvan Tele SANON |

# 2. Data Overview 📁

**Brief Description of the Data** :  ChestX-ray dataset comprises 112,120 frontal-view X-ray images of 30,805 unique patients with the text-mined fourteen disease image labels (where each image can have multi- labels) Fourteen common thoracic pathologies include : Atelectasis, Consolidation, Infiltration, Pneumothorax, Edema, Emphysema, Fibrosis, Effusion, Pneumonia, Pleural_thickening,
Cardiomegaly, Nodule, Mass and Hernia

The entire dataset can be dowload  for free [here](https://nihcc.app.box.com/v/ChestXray-NIHCC). 

This dataset of 40GB+ has already been properly separated by avoiding the "data leakage problem". Each image in the dataset was labeled according to 14 different pathological conditions.

**NB**: For computational power  reasons , we will take a single sample of this data for the multi-label classification and the entire available data for localization task

# 3. Methodology🔧

The project has been divided in **03** **main steps**:

### The multi-label classification → ([notebook link](https://github.com/AdrienJ0/Chest-X-Rays-project/blob/main/Chest_X-Rays_Classifier.ipynb))

- **Models used**: DenseNet ( as feature extractor ), DenseNet ( with fine tuning ) , Vision Transformer, RNN, RNN+CNN
- **Loss used**: Binary crossentropy
- **Metrics used**: AUC
- **Data Preprocessing**: Reshape data , Resize the Image pixel values data between 0 and 1 **,** Shuffle the input after each epoch , Normalise the data
- **Models optimization**
    - With weigthed loss
    - With Data Augmentation
- **Results :** Given the higher AUC and significantly lower validation loss (**Avg AUC 0.842** ; **Train Loss 0.19** ; **Val Loss 0.17)**, the **Densenet with fine-tuning** appears to be more favourable for the final predictions, as it has better overall performance based on the measurements provided.

### The localization → ([notebook link](https://github.com/AdrienJ0/Chest-X-Rays-project/blob/main/chest-x-rays-localization.ipynb))

- **Models used**: VGG19, MobileNet, Resnet50, Xception
- **Loss used**: Mean-Squared Error
- **Metrics used**:  IoU
- **Data Preprocessing:**  Deal with ****patient overlap ( check to see if a patient's ID appears in both the training set and the test set ) , Reshape Image and bonding box data , Resize the Image pixel values data between 0 and 1
- **Results :** After comparing the Val_loss and Val_IoU of the models, we finally selected the **Xception model (val_loss: 2819.52 , val_IoU: 0.2380 )** for our final application.

### Combination of the classification model and the localization model → ([notebook link](https://github.com/AdrienJ0/Chest-X-Rays-project/blob/main/chest-x-rays-localization.ipynb))

For our final test , we use the best model for each task :  **DenseNet ( with fine tuning )**  as classifier  and **VGG19** as localization model and taking account of their respective preprocessing steps 

# 4. Results and Conclusion 📑

For the **classification task**, **densenet** performed surprisingly well without optimisation. It is important to note that in the future it would be better to exploit all the data to obtain more convincing results.

For the **localisation task**, we were forced to use the little labelled data available but the **xception** model particularly stood out among those tested. In the future, optimisations can be made to this model in order to hope for better performance.
Finally, we found it interesting to **combine the 2** and simulate test conditions. The result is similar to what we could obtain with a detection task, with the difference that here we are making the most of the datasets available for each task and exploiting most of the models already discussed in class.
