# The Reinforcement Iteration Loop

This document describes the main learning loop for the project.

## Prerequisites

* A Gym environment with discrete action space
* An objective function that can provide rewards and punishments (e.g. how long the agent survives after an action)
* A TensorFlow model suited for reinforcement learning (see the design document checklist)

## Concept

The reinforcement loop should run multiple games, each one gathering information about what actions are successful for various states.
Between game runs, the model will be retrained to make use of the newly gathered data.
Because the model will start untrained and innacurate, it would likely get stuck performing the same action every timestep.
Therefore, a randomness factor, `epsilon`, is used to give a possibility each timestep of a random action being chosen rather than using the model.
As the model gains new data, it should become more accurate and so `epsilon` can be slowly decreased.

## Pseudocode

* Let `epsilon` start at `1`
* Let `epsilon_decrease` be a small positive number less than 1 (hyperparameter)
* Let `state_history` be an empty list
* Let `value_history` be an empty list
* Let the model begin untrained
* For every game run:
  * Let `states` be an empty list
  * Let `actions` be an empty list
  * Let `values` be an empty list
  * For every timestep:
    * Pick a random number between 0 and 1. If it is less than or equal to `epsilon`:
      * Perform a random action from the action space
    * Otherwise, if it is greater than `epsilon`:
      * Give the current game state to to the model to predict the scores of each action. Perform the action with the highest score.
    * Append the performed action to `actions`
    * Append the game state to `states`
    * Append an entry to `values` where every action's score is 0
    * If there is a reward/punishment:
      * Change each entry in `values` so that the score of the action performed for that entry (based on the record in `actions`) is changed by the reward/punishment value (positive for reward, negative for punishment)
  * Append all entries in `states` to `state_history`
  * Append all entries in `values` to `value_history`
  * Retrain the model with `state_history` as the inputs and `value_history` as the outputs
  * Decrease `epsilon` by `epsilon_decrease`
