This document describes the design doc for the model of this project.

# Input and output
Atari games and other games like flappy bird returns RGB image for current frame. 
**The input shape is going to be (frames, width, height, channels).**
Frames are hyperparameters that determines how many frames are going to be used for the training. The frames are from last few iterations of single episode.
Width and height depends on the enviornment that model trains on. Channels are set to 3 when it's using RGB image and set to 1 when using grayscale image.
**The output shape is going to be in shape (actions,)** where actions is a single integer in a discrete space. 

# Layers
- Layers are going to be a combination of **CNN** and **LSTM**. Because a single pixel provides just a single picture of the game, the model is not going to be able to recongnize the state of the game. Thus, by sending input(frames,width,height,channels) into convolution layers, the model is able to detect the state of the current pixel,i.e. whether flappy bird was flapping up or down.

# diagram
Using keras library, Conv2D is going to be used for convolutional layers. A simple diagram would look like:
Conv2d(width, height, channels) -> (width, height, filters).<br/>

The diagram for the model would look like: (frames, width, height, channels) -> Conv2D -> Conv2D -> Conv2D -> (frames, width, height, filters) -> Reshape -> (frames, width * height * filters) -> LSTM -> (actions,) <br/>

After the input goes through convolutional layers, it's output is going to be in shape of (frames, width, height, filters) where filters is a hyperparameters. Because LSTM only accepts **2-D arrays as its initial state**, the output of Conv2D has to be reshaped into **(frames, width * height * filters)**. But LSTM also expects **1-D array for each frame** when it's running through cells internally. For example, if the initial state was (2,X), then frame 1 will be in shape of (1,X) and frame 2 (2,X). 

- Diagram for LSTM
```
                frame0             frame1
                  |                  |
                  \/                 \/
initial state -> Cell -> state 1 -> Cell -> state 2
                  |                  |
                  \/                 \/
              discarded            output
```
# Hyperparameters
- number of Conv2Ds
- number of Conv2D filters
- Conv2D kernel size
- number of LSTM units
