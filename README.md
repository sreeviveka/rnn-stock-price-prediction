# EXPERIMENT 05: STOCK PRICE PREDICTION
## AIM:
To develop a Recurrent Neural Network model for stock price prediction.

## PROBLEM STATEMENT AND DATASET:
* The given problem is to predict the google stock price based on time.
* For this we are provided with a dataset which contains features like Date, Opening Price, Highest Price, Lowest Price, Closing Price, Adjusted Closing Price, Volume.
* Based on the given features, develop a RNN model to predict, the price of stocks in future.

## DESIGN STEPS:
1. Read the csv file and create the Data frame using pandas.
2. Select the " Open " column for prediction. Or select any column of your interest and scale the values using MinMaxScaler.
3. Create two lists for X_train and y_train. And append the collection of 60 readings in X_train, for which the 61st reading will be the first output in y_train.
4. Create a model with the desired number of neurons and one output neuron.
5. Follow the same steps to create the Test data. But make sure you combine the training data with the test data.
6. Make Predictions and plot the graph with the Actual and Predicted values.


## PROGRAM:
### Name: V.S.Sree Viveka
### Register Number: 2305001031
```python
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
from sklearn.preprocessing import MinMaxScaler
from keras import layers
from keras.models import Sequential

dataset_train = pd.read_csv('trainset.csv')

dataset_train.columns

dataset_train.head()

train_set = dataset_train.iloc[:,1:2].values
type(train_set)
train_set.shape

sc = MinMaxScaler(feature_range=(0,1))
training_set_scaled = sc.fit_transform(train_set)
training_set_scaled.shape

X_train_array = []
y_train_array = []
for i in range(60, 1259):
  X_train_array.append(training_set_scaled[i-60:i,0])
  y_train_array.append(training_set_scaled[i,0])
X_train, y_train = np.array(X_train_array), np.array(y_train_array)
X_train1 = X_train.reshape((X_train.shape[0], X_train.shape[1],1))

X_train.shape

length = 60
n_features = 1

model = Sequential([layers.SimpleRNN(40,input_shape=(60,1)),
                    layers.Dense(1)])
model.compile(optimizer='adam',loss='mse')
model.summary()

model.compile(optimizer='adam', loss='mse')

model.fit(X_train1,y_train,epochs=25, batch_size=64)

model.summary()
dataset_test = pd.read_csv('testset.csv')
test_set = dataset_test.iloc[:,1:2].values
test_set.shape
dataset_total = pd.concat((dataset_train['Open'],dataset_test['Open']),axis=0)

inputs = dataset_total.values
inputs = inputs.reshape(-1,1)
inputs_scaled=sc.transform(inputs)
X_test = []
for i in range(60,1384):
  X_test.append(inputs_scaled[i-60:i,0])
X_test = np.array(X_test)
X_test = np.reshape(X_test,(X_test.shape[0], X_test.shape[1],1))

X_test.shape
predicted_stock_price_scaled = model.predict(X_test)
predicted_stock_price = sc.inverse_transform(predicted_stock_price_scaled)

plt.plot(np.arange(0,1384),inputs, color='red', label = 'Test(Real) Google stock price')
plt.plot(np.arange(60,1384),predicted_stock_price, color='blue', label = 'Predicted Google stock price')
plt.title('Google Stock Price Prediction')
plt.xlabel('Time')
plt.ylabel('Google Stock Price')
plt.legend()
plt.show()
```

## OUTPUT:

### True Stock Price, Predicted Stock Price vs time:
<img width="646" height="441" alt="Screenshot 2025-09-30 091314" src="https://github.com/user-attachments/assets/66760e91-562b-4d39-a496-8e7167eb6ed2" />

### Mean Square Error:
V.S.Sree Viveka : 2305001031

 MEAN SQUARE ERROR : 540392.5569869860087

## RESULT:
Thus the stock price is predicted using Recurrent Neural Networks successfully.
