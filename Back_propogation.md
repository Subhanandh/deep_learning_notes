## Backpropagation

In the below back propagation process, x is the input and wh and bh are randomly created weights and biases.

|X||||           wh|||             bh|||
:-:|:-:|:-:|:-:| :-:|:-:|-:|       :-:|:-:|-:|
|1|0|1|0|        0.83|-0.14|0.19|  -0.94|-0.17|0.27|
|1|0|1|1|        1.73|0.06|0.92|
|0|1|0|1|        1.45|0.44|-1.58|
| ||||           -0.57|-0.53|1.39|      

Performing matrix dot product yields the following hidden_layer_input and followed by RELU non-linear activation.

hidden_layer_input||| hidden_layer_activations||| wout|  bout|
:-:|:-:|-:|           :-:|:-:|-:|                 :-|    :-|
|-0.32|0.11|-1.11|    0|0.11|0|                   -0.58| 0.47|
|-0.90|-0.42|0.27|    0|0|0.27|                    0.83|
|0.22|-0.65|2.59|     0.22|0|2.59|                 0.63|

wout and bout are randomly initialized weight ans biases in this layer to calculate the output. Y is thew desired output. Subtracting output from Y gives L1 loss which is mentioned as E.

output|Y|E|
:-:|:-:|-:|
0.56| 1 |0.43
0.64| 1 |0.35
1.98| 0 |-1.98

Slope of the output and hidden layer activation is calculated. Derivatives of RELU outputs are used as slopes.

slope|slope hidden layer|||
:-|    :-:|:-:|-:|
1|     0|1|0
1|     0|0|1
1|     1|0|1


|learning rate||
:-:|
0.1|

Delta in the output layer is calculated by multiplying E with slope along with the learning rate of 0.1

delta|
:-:|
0.04|
0.03|
-0.19|                

Error in the hidden layer is calculated by matrix dot product of delta  of output with transposed wout matrix.

error hidden layer||| 
:-:|:-:|-:|                    
-0.02|0.03|0.02|                    
-0.02|0.02|0.02|                    
0.11|-0.16|-0.12| 

Delta of the hidden layer is calculated using slope and error at the hidden layer.

delta hidden layer||| 
:-:|:-:|-:| 
0|0.03|0|                    
0|0|0.02|                    
0.11|0|-0.12| 

Updating weights of the hidden layer

wh|||             bh|||
:-:|:-:|-:|       :-:|:-:|-:|
0.83|-0.143|0.188|  -0.929|-0.167|0.26|
1.719|0.06|0.932|
1.45|0.437|-1.582|
-0.581|-0.53|1.4|  

Updating weights of output layer

wout|||  bout|
:-:|     :-:|
-0.575|  0.458
0.829|
0.678|

#### Code for random initialization of weights and back propogation
```python
import numpy as np
# step 1: Initialize weights and biases with random values

in_count = 3
in_channels = 4
X = np.array([[1,0,1,0],
              [1,0,1,1],
              [0,1,0,1]])
y = np.array([[1], 
              [1], 
              [0]])
wh = np.random.randn(in_channels, 3)
bh = np.random.randn(1,3)
print('X\n', X)
print('wh\n', wh)
print('bh\n', bh)

h_channels = 3
wout = np.random.randn(h_channels, 1)
bout = np.random.randn(1,1)
print('wout\n', wout)
print('bout\n', bout)

# step 2: Calculate hidden layer input
## hidden_layer_input = matrix_dot_product(X,wh) + bh
in_count = 3
hidden_layer_input = np.dot(X, wh) + bh
print('hidden_layer_input\n', hidden_layer_input)

# step 3: Perform non-linear transformation on hidden linear input
# hiddenlayer_activations = sigmoid(hidden_layer_input)
def sigmoid(x):
  return 1./(1 + np.exp(-x))

def relu(x):
  return np.maximum(x, 0)

hiddenlayer_activations = relu(hidden_layer_input)
print(hiddenlayer_activations)

# Step 4: Perform linear and non-linear transformation of hidden layer activation at output layer
# output_layer_input = matrix_dot_product (hiddenlayer_activations * wout ) + bout output; output_layer_activations = sigmoid(output_layer_input)
output_layer_input = np.dot(hiddenlayer_activations, wout) + bout
output = relu(output_layer_input)
print('output_layer_input\n', output_layer_input)
print('output\n', output)

# Step 5: Calculate gradient of Error(E) at output layer
#output1 = output.reshape(-1)
E = y - output
print('E\n', E)

# Step 6: Compute slope at output and hidden layer
# slope_output_layer = derivatives_sigmoid(output)
# slope_hidden_layer = derivatives_sigmoid(hiddenlayer_activations)

def derivatives_sigmoid(x):
  return x * (1 - x)

def derivatives_relu(x):
  return 1. * (x > 0) 

slope_output_layer = derivatives_relu(output)
slope_hidden_layer = derivatives_relu(hiddenlayer_activations)
print(slope_output_layer)
print(slope_hidden_layer)

# Step 7: Compute delta at output layer
# d_output = E * slope_output_layer*lr
lr = 0.1
d_output = E * slope_output_layer * lr
print(d_output)

# Step 8: Calculate Error at hidden layer
error_at_hidden_layer = np.dot(d_output, wout.T)
print (error_at_hidden_layer)

# Step 9: Compute delta at hidden layer
d_hiddenlayer = error_at_hidden_layer * slope_hidden_layer
print(d_hiddenlayer)

# Step 10: Update weight at both output and hidden layer
wout = wout - np.dot(hiddenlayer_activations.T, d_output) * lr
wh = wh - np.dot(X.T,d_hiddenlayer) * lr
print('wout\n', wout)
print('wh\n', wh)

# Step 11: Update biases at both output and hidden layer
bh = bh - np.sum(d_hiddenlayer, axis=0) * lr
bout = bout - np.sum(d_output, axis=0) * lr
print('bout\n', bout)
print('bh\n', bh)