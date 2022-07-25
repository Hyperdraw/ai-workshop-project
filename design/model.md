This document describes the design doc for the model of this project.

# Input and output
Atari games and other games like flappy bird returns RGB image for current frame. **The input shape is going to be (frames, width, height, channels).**
Frames are hyperparameters that determines how many frames are going to be used for the training. The frames are from last few iterations of single episode.
Width and height depends on the enviornment that model trains on. Channels are set to 3 when it's using RGB image and set to 1 when using grayscale image.
**The output shape is going to be in shape (actions,)** where actions is a single integer in a discrete space. 
