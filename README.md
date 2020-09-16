# Prediciton-of-lifetime-of-Li-ion-batteries
Given various measurements of a Li-ion battery during a limited amount of charging cycles,predict how many cycles has a battery cell lived through and how many cycles will it last before it breaks.

# Motivation for the project

In the ﬁeld of Li-ion batteries, life prediction is an essential step before ﬁnal commercialization for various applications, including electric transportation. Lifetimes of 10 and 15 years are expected for electric vehicles (EVs) and hybrid electric vehicles, respectively. In terms of cycle life, a lifetime up to 3000 deep cycles at room temperature can be requested for high energy applications (typical for EVs).

Interfacial-ﬁlm formation, structural degradations, positive-electrode-material dissolution, and loss of contact between the electrode and the current collector and within the electrode itself are among the most well-known aging sources of Li-ion batteries.

Although these aging sources have been known for a long time, effective tools for an accurate life prediction of Li-ion batteries have not been completely achieved yet due to the complexity of the problem: An accurate experimental diagnostics of aging phenomena and their proper mathematical description for life-prediction models are still challenging issues.

# Dataset Used

The dataset was loaded from Experimental Data Platform website.
A comprehensive dataset consisting of 124 commercial lithium iron phosphate/graphite cells cycled under fast-charging conditions, with widely varying cycle lives ranging from 150 to 2,300 cycles, was generated.

Although the dataset contains only new images, it is still the largest dataset out there available freely for use.


The data for each cell included:
1. **Descriptors** includes cycle life and charging policy and other descriptors for each cell.
2. **Summary data** includes information on a per cycle basis; including cycle number, discharge capacity, charge capacity, internal resistance, maximum             temperature, average temperature, minimum temperature, and chargetime.
3. **Cycle data** includes information for each cycle of each cell, and within each cycle,1000s of data points including time, charge capacity, current,          voltage, temperature, discharge capacity. We also include derived vectors of discharge dQ/dV, linearly interpolated discharge capacity and linearly                interpolated temperature.


# Data Preprocessing

We removed cycles that had time gaps, small outliers, or other inconsistencies. 

Another problem in the data was time. The different charging policies meant that some cycles were finished quicker than others and the time measurements of charge and temperature couldn’t be compared as they were.

Instead, we interpolated charge and temperature over the voltage and resampled them into 1000 equidistant voltage steps. Now these can be compared as is.

Now, the processed data includes scalar features (such as Internal resistance, cycle time, charge Ah for cycle) and array features (such as linearly interpolated charge and temperature points) for each cycle. 


# Further Preprocessing the data into input and target features

The framework used for the training was Tensorflow 2.0 with keras as backend.

We decided to perform a deep learning approach using CNNs to train the model.

The dataset was further converted in a format compatible with the inputs for CNNs.

For this purpose, a window of 20 consecutive cycles was used as input, and a single target, i.e. , the remaining cycles for the last cycle in window, was used.

Another problem faced was that some of the inputs were scalar quantities in each cycle, whereas others were vectors or detailed quantities in each cycle. To overcome this, both these types were given to the model at different input points and finally were concatenated later in the network.

Around 50% of the dataset was used for training, and the rest was divided equally between validation and testing.


# FLOW of the Model

Functional API of keras was used, for building up the model layer by layer, as multiple input points were required. The details of different layers in the model are as shown. 

Dropout Regularisation technique was used to prevent overfitting of the model.

The Detailed and scalar features were learnt separately and then were combined later in the model before generation of the final output.

![conv Net Flow Image]
(https://github.com/viv1-m/Prediciton-of-lifetime-of-Li-ion-batteries/blob/master/battery_prediction_model.png?raw=True)


# Training and Validation

The preprocessed dataset was trained with model created.
Various hyperparameters were used.

Best validation accuracy was achieved with 64 epochs, 0.3 dropout rate,0.0001 learning rate, batch size of 32 along with kernels of (3,9) for conv2d layers and (3) for conv1d layers.

![training and validation losses]
(https://miro.medium.com/max/645/1*GlKTXNA4qheD0MB8m5mqgg.png)

# Future Goals

Further goals are:
* To bring the window size as low as possible.
* To use the results obtained to predict realtime  life-time for various Li-ion batteries, including the phone and laptop batteries and Electric vehicles.

