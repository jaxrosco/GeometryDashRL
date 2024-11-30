# Reinforcement Learning For Geometry Dash
A fun project for my Topics in Computer Science class.

![image](https://github.com/user-attachments/assets/00b439b0-2b20-47ce-8143-da1275116976)
![image](https://github.com/user-attachments/assets/dee1da4a-0b44-4faa-9229-f352604bb6b0)

## Description
Using a custom gym environment and stable-baselines3, I aimed to train a model to play the game [Geometry Dash](https://store.steampowered.com/app/322170/Geometry_Dash/) by RobTop Games. While not entirely successful so far, I've had a lot of fun with it.

## Requirements
On top of what is listed in the [requirements.txt](./requirements.txt) file in the repository, you'll also need a copy of Geometry Dash so the model can interact with the game. I would also recommend using a gpu if possible to speed up the actual training process.

## How It Works
There are two main parts to the project, the custom gym environment, and the training portion.

The custom gym environment:
  - Observation Space: Uses grayscale screenshots of the game window, resized to 640x360
  - Action Space: Three discrete actions: no action, press w (jump), and hold w (hold jump)
  - Reward Structure:
    - +1 for encountering new screen columns on the right side, encouraging progress in the level as time goes on
    - -0.05 penalty for pressing w and -0.5 for holding w, this was meant to discourage unproductive jumping and potentially speed up the learning process
    - -15 penalty upon dying, uses template matching with a pre-captured menu template to identify when the player is dead

Training:
This is handled by stable-baselines3 and really abstracts the process (yippee!):
  - Initializes the custom environment
  - Sets up the [PPO](https://en.wikipedia.org/wiki/Proximal_policy_optimization) model with a [convolutional neural network](https://en.wikipedia.org/wiki/Convolutional_neural_network) policy.
  - Defines some hyperparameters, these can be changed and I honestly should do some more testing with different options
  - And finally, trains the model for a specified number of timesteps

## Running it yourself
With the requirements satisfied:
  1. In options under gameplay, turn off auto retry
  2. In graphics, make sure the game is set to medium texture quality
  3. In advanced video settings, turn off vertical sync
  4. Ensure the game is in windowed mode running at 1080p, this is because of the menu template
    4.1. If you'd like to run the game at another resolution, take a screenshot of any level's death menu and replace the template, it should work regardless of level as the matching is only required to be 70%
  5. When ready, navigate to the level you wish to train the model on and start the training portion
    5.1. Sometimes, Windows really doesn't like us moving things around programatically so if SetForegroundWindow() is not working, try using SetActiveWindow() instead in the gym environment

## Acknowledgements
None of this project would have been possible without these libraries:

[gymnasium](https://gymnasium.farama.org/)
[stable-baselines3](https://stable-baselines3.readthedocs.io/en/master/index.html)
