# AI Workshop Final Project

## Task

The goal of this project is to train an AI model to play Flappy Bird using reinforcement learning.

## Requirements

* A **local** Python environment (Google Colab won't work because it can't interface the game)
* TensorFlow
* PyAutoGUI

## Concept

The model will learn by playing the game and collecting reinforcement data. Each timestep, the model will be expected to choose an action: to flap or not to flap. It will be given the last several frames of gameplay as images and be expected to classify which action (whether to flap) is best.

For the first game there is no collected data, so the actions will be chosen randomly. After every game the collected data, consisting of **frame images**, **actions chosen**, and the **success factor** (the length of time the bird survives after the action), will be added to a cumulative dataset and the model will be trained with the data.

For the next game, there is a trained model, but it will not have enough data to accurately predict actions and will likely get stuck predicting the same action every time, so the randomness factor (the probability that an action will be chosen randomly instead of by the model) will only be reduced by a small constant.

Iterations will continue, the dataset will become larger, the randomness factor will eventually reach zero, and the model should become more accurate.

## Implementation (Subject to Change)

### The Model

The model will be a combination of convolutional layers and RNN.

#### Inputs

Each input datapoint will consist of multiple frame images. The datapoint shape will be `(frames, width, height, channels)` where `frames` is the number of past frames to consider, `width` and `height` are the size of the frames, and `channels` is the number of image color channels (3 for RGB, 1 for grayscale).

#### Outputs

Each output datapoint will consist of a single binary probability (whether to flap).
