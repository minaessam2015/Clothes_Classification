# Clothes_Classification
The goal is classify the clothes into different categories. The dataset used is at "https://github.com/alexeygrigorev/clothing-dataset"

To replicate the experiments and results
1. Clone this repo
2. Clone the above repo to get the dataset. The expected path to be cloned inside this repo, i.e. within this repo's working directory.
3. Install the requirments in the requirments.txt file
4. Open the notebook and run the cells.

# Selected Model
I chose EffecientNet0 to be my base model for experiments. The rationale behind this is that EffecientNet is a light-weight network that can be trained fast to give an indicator about the problem complexity. However, I found it to be good enough that I didn't have to search for other architectures.

# Dataset Handling
## Why this dataset
I chose this dataset as it's small in size and can be trained relatively fast on a CPU. 
## Dataset Issues
The dataset contains 5756 images. However, there are only 5403 IDs in the metadata file along with the datase, i.e. there are around 350 images unlabeled. Moreover, there were five more IDs in the metadata file that had no correspendence in the images folder, so I removed those.
The dataset is heavily imbalanced as seen from this hitogram 
### Add the histogram
The dataset contains twenty classes `'Blazer', 'Blouse', 'Body', 'Dress', 'Hat', 'Hoodie', 'Longsleeve', 'Not sure'
       'Other', 'Outwear', 'Pants', 'Polo', 'Shirt', 'Shoes', 'Shorts','Skip', 'Skirt', 'T-Shirt', 'Top', 'Undershirt'`. However, after close inspection to the data and label I can see that the label "Not sure" is very confusing to the network since we already have a label called "Other". So, I ignored the images with corresponding label "Not sure". The results enhanced a lot after doing so.
       
       ## Add images

# Experiments Structure
I conducted multiple experiments to approach the problem; however; I'm going to mention the three main ideas I followed.
1. Trained a baseline from `EfficientNetB0` using transfer learning. The training used imagenet weights as an initialization for the network and added a fully conned=cted layer with the desired number of classes. So, I only trained the last fully connected layer. The model was able to reach ~ 85% after only 12 epochs.
2. Fine tune the previous model by unfreezing the last 10 layers and re-trained the model. However, this experiment led to more overfitting.
3. Used data augmentation with experiment 2 (fine-tuning).

# Suggested Experiments
