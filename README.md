## Anomaly-based Intrusion Detection System using a one-dimensional Convolutional Neural Network 
As technologies in information and virtualization evolve, the volume of security threats attempting to cause damage to systems
grows and becomes more powerful, which highlights the importance of Intrusion Detection Systems (IDS) to have an essential role
in network security and help in detecting malicious attacks from network traffic. The most widely used network anomaly detection
systems are based on machine learning (ML) techniques such as decision tree (DT), support vector machine (SVM), and K-nearest
neighbor (KNN). Although IDSs based on ML techniques have achieved promising results and high detection rates, however, it is
considered a type of shallow learning that depends mainly on feature engineering and requires large-scale data pre-processing as the
size of the dataset grows. To overcome these problems, Deep learning-based IDSs are proposed because they have a better ability
to extract features from huge amounts of data. In this research, an IDS model based on one-dimensional Convolution Neutron
Network (CNN1D) is proposed that is able to detect anomalies with accuracy of 93.2% and F1-score of 93.1%. Entire NSL-KDD
benchmark dataset was used to train this model. Achieved results are then compared to Deep Learning (DL) methods like CNN,
LSTM, Recurrent Neural Network (RNN), and others to prove the proposed model’s superiority.

### Data preprocessing steps
![preprocessing](https://user-images.githubusercontent.com/83555471/189566619-215bcc28-6e00-4d38-b7b7-2673ff0a6b5a.png)

### Model structure 
The proposed model consists of seven blocks including an input block, an output block, Three CNN blocks, and
two MLP (Fully connected) blocks. The CNN hidden blocks are consist of a 1D convolution layer, a 1D max pooling
layer and a Dropout layer with 0.1 dropout rate as shown in figure (5) and table (6). The input layer has a size of
(1,122). The 1D convolution layers have sizes of (1,62) , (1,62) and (1,124) respectively, a kernel size of 2,4,and 8
respectively, a stride of 1, and a same padding. The max pooling layers have pool size of 2, 4 ,and 8 respectively and
a stride of 1. The MLP hidden blocks are consist of 256, and 5 neurons respectively, a relu activation function for the
first layer and softmax activation function second layer and also dropout layer with 0.1 dropout rate as shown in figure
(5) and table (6). The output layer consists of one neuron with a sigmoid activation function for the binary model and
five neurons with a softmax function for five classes classification model as shown in figure (5) and table (6).
The output of each CNN hidden block is fed into the next CNN hidden block and also flatten and concatenate with
the output of the MLP hidden block 2 and then fed to the output layer as shown in figure (5) and table (6). For the two
models, the weights were initialized with a random normal initializer with zero mean and 0.01 standard deviation, and
the biases were initialized with zeros.

![model](https://user-images.githubusercontent.com/83555471/189566815-e7e1a10f-3de6-4066-af4a-27eaf97e0a4a.png)

### Results 
#### Binary Confusion matrix.
![binary_confusion_matrix](https://user-images.githubusercontent.com/83555471/189566865-06b18d51-e815-424a-8bb5-6987ba6ab442.png)
#### Categorical Confusion matrix.
![categorical_confusion_matrix](https://user-images.githubusercontent.com/83555471/189566875-628148ac-90f6-43d6-9335-80d081784e3c.png)
