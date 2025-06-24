# Neural Network Summary

## Components of a Neural Network

At its core neural networks are made up of the following components :
1. Layers : Input Layer, Hidden Layers, Output Layer
2. Weights
3. Biases

![Neural Network](https://res.cloudinary.com/dldvhlymt/image/upload/v1750779066/Neural_Network_szd1mg.png)

From the diagram above we can see the the relationship between the input and output layer, without any hidden layers in between. The weight can then be seen as a matrix that is applied to the input layer to obtain the output layer.

Hidden layers are just an extension of the above and any layer between the input and output layers are known as the hidden layer.

Biases have the same dimensions as the layer itself and are values added to the nodes of the layer itself.

## Dimensions of the Neural Network Components

Based on the diagram above,
the input layer is a 1 x 2 matrix,
the weight is a 2 x 3 matrix,
the output layer is a 1 x 3 matrix

The relationship between the dimensions are :
1. Layer has the shape of 1 x (number of nodes)
2. Biases have the shape of 1 x (number of nodes)
3. Weights have the shape of (number of nodes of previous layer) x (number of nodes of next layer)

## Feed Forward

Basically the feed forward process means to find out the values of the output layer. In running of a trained model this is also known as inference.

Activation Functions compresses the input to a predetermined range e.g. 0 to 1.
Sigmoid function is a common example.

```python
import torch
import torch.nn as nn
import torch.optim as optim

X = torch.tensor(([4, 10], [1, 8], [5, 6]), dtype=torch.float)
Y = torch.tensor(([55], [120], [87]),dtype=torch.float)
XPredicted = torch.tensor(([2, 6]), dtype=torch.float)

Xmax, _ = torch.max(X, 0)
XPredictedmax, _ = torch.max(XPredicted, 0)

X = torch.div(X,Xmax)
Y = Y/100
XPredicted = torch.div(XPredicted, XPredictedmax)

class Neural_Network(nn.Module):
  def __init__(self, ):
    super(Neural_Network, self).__init__()
    self.inputSize = 2
    self.outputSize = 1
    self.hiddenSize = 3

    self.W1 = nn.Parameter(torch.randn([self.inputSize, self.hiddenSize]))
    self.W2 = nn.Parameter(torch.randn([self.hiddenSize, self.outputSize]))

  def sigmoid(self, X):
    return 1/(1+torch.exp(-X))

  def forward(self, X):
    self.z = X @ self.W1
    self.z2 = self.sigmoid(self.z)
    self.z3 = self.z2 @ self.W2
    o = self.sigmoid(self.z3)
    return o
```

## Backward Propagation

First we run a feed forward process and determine the loss based on the outputs and the predicted outputs.

We then calculate the gradient of the losses with respect to the weights and multiply them by a factor called the learning rate. We then subtract the weights by this product to complete a cycle of backward propagation.

```python
model = Neural_Network()
criterion = nn.MSELoss()
optimizer = optim.SGD(model.parameters(), lr=0.001)
```

## Training

Training is simply running background propagation for a certain number of times, also known as epochs.

```python
num_epochs = 10000
for epoch in range(num_epochs):
  outputs = model(X)
  loss = criterion(outputs, Y)

  optimizer.zero_grad()
  loss.backward()
  optimizer.step()

  if epoch % 10 == 0:
    print(f'Epoch {epoch}. Loss {loss.item():.4f}')
```