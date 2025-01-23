Brief coverage of how the Neural Network is built

<img width="1084" alt="NeuralNetworkBlueprint1" src="https://github.com/user-attachments/assets/b422de35-671e-46c3-ab93-bb5f8de3f9ee" />

* Training input Data variable consist of the entire dataset (thousands of rows) residing in a single tensor E.g { 400,000, 88, 15 } where the batch size is { 400,000, 15 }, 88 sets
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
<img width="715" alt="NeuralNetworkBlueprint4 (cut)" src="https://github.com/user-attachments/assets/44090b18-23da-443e-aada-76adbf0805eb" />

* Denormalize function is to unnormalize predicted output from model. It is also used to unnormalize the initial input (which acts as a GTV) for the current prediction
<br/>
<img width="1422" alt="NeuralNetworkBlueprint5" src="https://github.com/user-attachments/assets/f4d457d9-6b5a-4f27-b10d-8fae7a8a0148" />

* After obtaining the unnormalized linear velocities & angular velocities, the calculated rotation and distance is added to its previous last known frame/ position & rotation to obtain the next predicted frame
* the loss value is calculated
</br>
<img width="1417" alt="NeuralNetworkBlueprint6" src="https://github.com/user-attachments/assets/668e7eb5-458b-4de2-a597-61793a2bf7d9" />

* variables is the returned weights and bias
* The weights and bias and their respective gradients are then passed to the RMSProp optimizer to tune and adjust. RMSProp adjusts the learning rate efficiently based on the moving average of squared gradients, preventing it from becoming too small
* After training completes based on the number of iterations you set or throughout the entire training data set, the final weight and bias values (variables) are saved. The model is ready for inference use.
</br>
<img width="1106" alt="NeuralNetworkBlueprint7" src="https://github.com/user-attachments/assets/04bf6431-1459-4eee-a004-7164f9e76cbb" />

* 'RunAngleAxisModel' allows us to infer from the saved model.
* 'CollectInferenceAngleAxisActorData' takes the current position & rotation of the falling cube and feeds it to the model to predict the next frame, where the cube should be positioned, and which angle it should be rotated. 
* 'Do Once' function randomly generates an initial linear and angular velocity to apply to a static cube.
* </br>
<img width="1125" alt="AngleAxisInferenceBlueprint2" src="https://github.com/user-attachments/assets/eddb85bb-2898-476f-b637-e8abb54bcaa9" />

* 'Move cube' function takes in predicted angular and linear velocities from the model and applies it to the cube.
* 'count frames variable' resets the cube's position to its initial position after a certain number of frames (so it doenst continue falling to the end of the world)
</br>
<img width="1046" alt="AngleAxisInferenceBlueprint3" src="https://github.com/user-attachments/assets/e2393714-59fc-429d-8957-42f01f26a94a" />

* Once the cube is resetted after a number of frames, another initial linear and angular velocity is applied
</br>
Results of Unreal Engine Physics simulator

* "https://drive.google.com/file/d/1Et-_OysCh8klIzQgDGKlZ4CSIRTsnEd5/view?usp=sharing" Angle Axis Representation video 1
* "https://drive.google.com/file/d/1Et-_OysCh8klIzQgDGKlZ4CSIRTsnEd5/view?usp=sharing" Angle Axis Representation video 2
* "https://drive.google.com/file/d/1uWCpN5p6AhEibpkFYb22p0bTO-hn2XDD/view?usp=sharing" Euler Representation
