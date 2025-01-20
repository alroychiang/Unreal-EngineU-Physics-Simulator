# Unreal-Engine-Physics-Simulator

Brief coverage of how the Neural Network is built

<img width="1084" alt="NeuralNetworkBlueprint1" src="https://github.com/user-attachments/assets/b422de35-671e-46c3-ab93-bb5f8de3f9ee" />

* Training input Data variable consist of the entire dataset (millions of rows) residing in a single tensor E.g { 400,000, 88, 15 } where the batch size is { 400,000, 15 }, 88 sets
* Mean and Std Dev variables are tensors used to normalize input data
* Input section is where the huge Training input tensor is being sliced into batch sizes depending the type of columns/ features you want the model the train on E.g angular velocity, linear velocity, position and rotation
<br/>
<img width="800" alt="NeuralNetworkBlueprint2" src="https://github.com/user-attachments/assets/66ecc24f-f257-46d5-8b34-971214481a66" />

* GTV is the Ground Truth Value. It is taken from the Training input data that is split into 50-50.
* GTV is used to calculate the loss value per iteration. Predicted - GTV = Loss. Similarly it is split into batch sizes of the same shape as the training input
* Deltaseconds is used to calcualte the distance moved, the angular distance (radians) rotated by multiplying with the predicted linear and angular velocities. Speed * Deltaseconds = Distance.
<br/>
<img width="1346" alt="NeuralNetworkBlueprint3" src="https://github.com/user-attachments/assets/23395aa6-a9af-4580-96d0-722e52ba00b2" />

* Redundant variables are created & printed here for debugging/ logging purposes E.g GTVresetCondition, Input Frame, GTV, deltaseconds
* LayerInput function is to convert from NNOutput datatype into a LayerInput datatype, required before inputting into the Model for training/ inference. Both datatypes are classes that is coded in the custom UE Tensorflow  Plugin
* 'LinkNNModel as subgraph' is a wrapper function that stores model layers into a neat function node. This is for readability and portability purposes. E.d Residual Neural Network, Multilayer Perceptron.
<br/>
Results of Unreal Engine Physics simulator

* "https://drive.google.com/file/d/1Et-_OysCh8klIzQgDGKlZ4CSIRTsnEd5/view?usp=sharing" Angle Axis Representation video 1
* "https://drive.google.com/file/d/1Et-_OysCh8klIzQgDGKlZ4CSIRTsnEd5/view?usp=sharing" Angle Axis Representation video 2
* "https://drive.google.com/file/d/1uWCpN5p6AhEibpkFYb22p0bTO-hn2XDD/view?usp=sharing" Euler Representation
