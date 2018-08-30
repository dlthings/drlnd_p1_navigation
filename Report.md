# Project 1 Report

See [README.md](README.md) for instructions.

## Problem

The problem involves a robot driving around an square room filled with randomly placed yellow and blue bananas.  Picking up a yellow banana is +1 points and picking up a blue banana is -1 points.  The states is 37-dimensional including the robots position and velocity along with perception data of the bananas in the vicinity.  The action space is 4-dimensional: (move forward, move backward, turn left, and turn right).

## Method

I solved this problem by implementing a Double Deep Q Network. I started from the [example DQN code](https://github.com/udacity/deep-reinforcement-learning/tree/master/dqn) from the DRLND lessons.  I adapted this code to interact with the unity env and upgraded learn() function to do [Double DQN](https://arxiv.org/abs/1509.06461).

DQN works by processing a buffer of experiences (a.k.a. "experience replay") and updates an estimation of the cost to go, Q(s,a), where s is the state and a is the action.  This function is approximated using a deep neural network where s is a continuous input and the Q-values for each discrete a is the output.  Each experience is represented by the tuple (s,a,r,s') where r is the one-step reward and s' is the next state achieved by taking action a at start s.  Given a buffer of experiences, the Q(s,a) approximation is updated in order to minimize the TD-error (a.k.a. "temporal difference error").

Ultimately we will update our estimate of Q(s,a) via gradient descent:

Q'(s,a) = Q(s,a) + alpha * TD-error

where TD-error is computed for each experience tuple.  The TD-error is the difference between the target cost-to-go and the expected based on the current function estimate:

TD-error = Q_target - Q_expected
Q_target = r + gamma*max_a Q(s',a)
Q_expected = Q(s,a)

In Double DQN we use a separate Q network to model the Q_target calculation that is updated more slowly in order to avoid using a noisy Q estimate during training.

## Model

I used a deep neural network with two hidden layers of 64 units each.  Each layer is fully connected with ReLU activations.

The replay buffer length, BUFFER_SIZE, was 100000 tuples long and the learning was performed on a BATCH_SIZE of 64.

The local network was updated every (UPDATE_EVERY) 4 steps.

The TD-error was calculated with a discount GAMMA = 0.99 and the learning rate used for gradient descent was ALPHA = 5e-4.

## Results

My 100-episode average return is shown below.  The algorithm solves the problem (>=13) in around 500 episodes and produces the plot below:

![Image](Result.png)

## Future Work

Implementing a [Prioritized Experience Replay](https://arxiv.org/abs/1511.05952) would almost certainly improve the score quicker.  However the implementation isn't a simple one-line change like it was to go from DQN to Double DQN.  Also, efficient implementation requires a special data structure called a Sum Tree.  I found a good implementation for reference [here](https://github.com/rlcode/per).

An interesting alternative approach would be [Distributed Prioritized Experience Replay](https://arxiv.org/abs/1803.00933) which has been shown to drastically improve the efficieny and score in an arcade game environment setting.