---
layout: project
title: Strain Energy Predicting Neural Networks
description: Used the MechanicalMNIST dataset for uniaxial extension on finite element simulation of heterogeneous material subject to large deformation to train a 197,000 parameter fully connected neural network (FCNN) 
technologies: [pytorch, numpy, deep learning]
image: /assets/images/mape.png

---
## Strain Energy Predicting Neural Networks 

This project explores the use of *deep learning* to predict strain energy in deformable materials from image-based data. Using the MechanicalMNIST dataset, a neural network is trained to learn the relationship between spatial deformation patterns and the resulting stored energy.

By combining concepts from *mechanics of materials* and *machine learning,* this work demonstrates how data-driven models can approximate complex physical behavior without explicitly solving governing equations. The project highlights the potential of neural networks as fast, scalable tools for engineering analysis and simulation.

---

## Dataset Prepping 

Selecting data is the most important part of preparing a Machine Learning Model. Here is a description of **MechanicalMNIST:**

![Description of dataset]({{ "/assets/images/desc.png" | relative_url }}){: .inline-image }  

**A visualization of the data can be seen below**  

 Brighter colors correspond to higher deflection, and vice-versa. Total strain energy can be seen above each respective image.    

![Sample of data]({{ "/assets/images/samp.png" | relative_url }}){: .inline-image }     

---

## Model    

The model used in this project is a **Fully Connected Neural Network (FCNN)** designed to predict total strain energy from MechanicalMNIST images. Each 28×28 image is flattened into a 784-dimensional input vector and passed through two hidden layers with ReLU activations, allowing the network to capture nonlinear relationships between deformation patterns and strain energy.    

The network outputs a single scalar value, framing the problem as a regression task. Training was performed using the **AdamW** optimizer for improved generalization through weight decay, and **Smooth L1 Loss (Huber Loss)** to balance sensitivity to outliers with stable gradient behavior.   

This approach demonstrates how a relatively simple neural network can learn physically meaningful relationships—mapping regions of high deformation (brighter pixels) to increased strain energy—without explicitly encoding governing equations.   



---


## Tuning Hyperparameters   

After creating the model, the next step was tuning the hyperparameters to decrease the error of my model.   

The document below describes my process for experimenting with hyperparameters

![Description of experimentation]({{ "/assets/images/tuning.png" | relative_url }}){: .inline-image }

---

## Final Error, Test Cases  

The final model performance was evaluated on a held-out test dataset to assess its ability to generalize to unseen data. Model accuracy was quantified using Mean Absolute Percentage Error (MAPE), which provides an intuitive measure of prediction error relative to the true strain energy values.

![Description of MAPE]({{ "/assets/images/mape.png" | relative_url }}){: .inline-image }    


After hyperparameter tuning, the model achieved a final test error of approximately 4.37% MAPE, indicating strong predictive performance. This low error suggests that the network successfully learned the relationship between deformation patterns and total strain energy, and was able to generalize well beyond the training data.


---
