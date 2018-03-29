# Flappy bird : a reinforcement learning challenge

_This repository contains the work done during a reinforcement learning 
challenge where the goal was to learn to play 
[Flappy Bird](https://en.wikipedia.org/wiki/Flappy_Bird). It is a 
side-scrolling game where the agent must successfully nagivate through 
gaps between pipes. Only two actions in this game: at each time step, either 
you click and the bird flaps, or you don't click and gravity plays its role._


## 1 The goal of the challenge

The goal of this challenge was to:
<ol>
<li> Design a reinforcement learning algorithm to learn how to play the game, 
and train it ;
<li> modify 'FlappyPolicy.py' in order to implement the function 
`FlappyPolicy(state,screen)` used in the `run.py` file without modifying 
`run.py`. `FlappyPolicy(state,screen)` takes both the game state and the screen 
as input ;
<li> Add any useful material (comments, text files, analysis, etc.) ;
<li> Make a pull request on the original repository <b>when finished only</b>.
</ol>

The evaluation was done by running 'run.py' so that 'FlappyPolicy' called
an already trained policy and did not learn during evaluation.


## 2 Installation
### 2.1 Pygame environment
To have an environment for python gaming, we need to have `pygame` install. To do 
so you may use `pip` by running :

```
pip install pygame==1.9.3
```

### 2.2 Learning environment
Now we need to install the reinforcement learning environment on top of `pygame`
, with a specific version adapted for the challenge. It should be present in the
repository, and was originally taken from 
[here](https://github.com/ntasfi/PyGame-Learning-Environment). To install it,
go to the repository and make a local install :

```
cd ./PyGame-Learning-Environment/
pip install -e .
```

### 2.3 Learning requirements
Finally, some libraries are needed for processing during the learning phase. To
install them all, you may run :

```
pip install -r learning-center/requirements.txt
```

## 3 Usage
### 3.1 Simple run
To see the trained model that was used for evaluation in action, simply go to 
the `learning-center` folder and start the `run.py` :
```
cd ./learning-center/
python run.py
```

### 3.2 Advanced runs
Several other models are available in the `learning-center/models/` folder. To
run those models, you must run the `Train.py` file that was used for training.
You can also create new models with different parameters, and train them 
directly by using specific command line arguments. For available commands and 
the way to proceed, use the provided help :
```
cd ./learning-center/
python Train.py --help
```

**CAUTION: if you do not specify `--play` (or `-p`) or `--test` (or `-t`), the
athlete is going to start a new learning phase.**

## 4 Content
_In this part is explained the algorithm and the features of the code._
### 4.1 Feature engineering
The used technique is based on a simplification of the state vector to reduce
the state space and be able to explore it completely in a relatively short 
period of time. It is based on the following hypothesis : to win the game and go
on forever, the bird needs to know :
- its horizontal distance to the next pipe
- its vertical distance to next top pipe
- its velocity

Moreover, we consider that only an approximation of those values is enough for 
the algorithm. They can be rounded to reduce even more the state space.

With this in mind, the inputs of the Q-learning algorithm are :
- the distance to the next pipe, approximated by a factor called `X` or 
`X_reduce`
- the vertical distance to the bottom of the top pipe, computed from the 
vertical position of the bird and the vertical position of the bottom of the 
pipe, and approximated by a factor called `Y` or `Y_reduce`.
- the velocity, approximated by `V` or `V_reduce`

In the end, the initial state space was reduced from 8 variables with wide 
ranges to 3 variables with a reduced ranges and that can be explored.

### 4.2 Exploration and rewards

To explore the state space, the bird starts with a _always falling_ policy 
because after playing a while, it seems that to win the game, one is falling
more than they are jumping at each time step. On top of that, a little bit of 
random chosing is added at the beginning of the learning and disappears 
afterwards. Finally the rewards are the following :
- if the bird is still alive, the reward is `1`
- if the bird passes a pipe, the reward is `10`
- if the bird dies, the reward is `-100`

### 4.3 Learning
### 4.4 Results