# Clothes_Classification
The goal is classify the clothes into different categories. The dataset used is at [this repo](https://github.com/alexeygrigorev/clothing-dataset)

To replicate the experiments and results
1. Clone this repo
2. Clone the above repo to get the dataset. The expected path to be cloned inside this repo, i.e. within this repo's working directory.
3. To install the requirments in the envname.yml file, run the following command `conda env create --file envname.yml`
4. Open the notebook and run the cells.

# Selected Model
I chose `EfficientNetB0` to be my base model for experiments. The rationale behind this is that EfficientNet is a light-weight network that can be trained fast to give an indicator about the problem complexity. However, I found it to be good enough that I didn't have to search for other architectures.

# Dataset Handling
## Why this dataset
I chose this dataset as it's small in size and can be trained relatively fast on a CPU. 
## Dataset Issues
The dataset contains 5756 images. However, there are only 5403 IDs in the metadata file along with the datase, i.e. there are around 350 images unlabeled. Moreover, there were five more IDs in the metadata file that had no correspendence in the images folder, so I removed those.
The dataset is heavily imbalanced as seen from this hitogram ![Data distribution](/figs/histogram.png)
The dataset contains twenty classes `'Blazer', 'Blouse', 'Body', 'Dress', 'Hat', 'Hoodie', 'Longsleeve', 'Not sure'
       'Other', 'Outwear', 'Pants', 'Polo', 'Shirt', 'Shoes', 'Shorts','Skip', 'Skirt', 'T-Shirt', 'Top', 'Undershirt'`. However, after close inspection to the data and label I can see that the label "Not sure" is very confusing to the network since we already have a label called "Other". So, I ignored the images with corresponding label "Not sure". The results enhanced a lot after doing so. ![Not sure label](/figs/not_sure_samples.png). ![Randomly selected images](/figs/random_labels.png)

Also, the dataset dimensions vary a lot, i.e the aspect ratios. Here's the distribution for changes i width and height. ![Boxplot](/figs/boxplot.png)
# Experiments Structure
I conducted multiple experiments to approach the problem; however; I'm going to mention the three main ideas I followed.
1. Trained a baseline from `EfficientNetB0` using transfer learning. The training used imagenet weights as an initialization for the network and added a fully conned=cted layer with the desired number of classes. So, I only trained the last fully connected layer. The model was able to reach ~ 85% after only 12 epochs. Also, I got the following confusion matrix for the task ![Confusion matrix original](/figs/conf_orig.png)
3. Fine tune the previous model by unfreezing the last 10 layers and re-trained the model. However, this experiment led to more overfitting.
4. Used data augmentation with experiment 2 (fine-tuning).

# Results
1. The model achieved top 1 accuracy of `85%`.
2. The model is doing good job for most classes as seen from the confusion matrix.
3. For the classes with low accuracy like "T-Shirt", we find the model confusing that with "T-Shirt" and "Undershirt" which are very similar to each other. Also, the same observation holds true when looking at "T-Shirt" and "Polo". Both are t-shirts, so it makes sense to be of difficult categories.
4. Model size on disk is around 15 MB.
5. The FLOPs count for the model I used is `0.39 Billion`
6. The total receptive field of the network is around `380`

# Suggested Experiments
Maybe adding a weighted loss function would help mitigate the data imbalance os that it takes more care of the under-presented classes. Also, over sampling those classes would help as well (augment more the under-presented classes).
Combining multiple datasets for the same task can enhance the overall result. For example, [DeepFashion](https://mmlab.ie.cuhk.edu.hk/projects/DeepFashion.html)
