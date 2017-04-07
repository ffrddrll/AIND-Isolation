## Research Review

### Background

All games of perfect information may be solved by recursively computing the optimal value function, which determines the outcome of the game, in a search tree containing possible sequences of moves.
But the exhaustive search is infeasible for Go.
There are two gerneral principles to reduce effective search space:

- Reduce the depth of the search by position evaluation
- The breadth of the search may be reduced by sampling action from a policy of possible moves.

Monte Carlo tree search (MCTS) is an algorithm based on above principles.
MCTS search to maximum depth by sampling long sequences of actions from a policy and evaluate the game state.
However, prior work has been limited to shallow policies or value functions based on a linear combination of input features.

### New Technique

In this paper, a new Go program AlphaGo was introduced with several new techniques:

- Training pipeline consisting of several stage of machine learning
  - Convolutional neural networks for Go
    Similar to the deep convolutional network for visual domain, AlphaGo use many layers of neurons and constructe the board position as a 19 × 19 image.
    By using the neural networks, the effective depth and breadth of the search tree can be reduced.
  - Supervised learning policy network
    A neural network (SL policy network) is trained to predict human expert moves using supervised learning.
    Another faster but less accurate rollout policy also be trained.
  - Reinforcement learning policy network
    To improve the policy network, new policy networks (RL policy network) are trained by reinforcement learning from games of self-play.
    Games are played by current policy network againts randomly selected previous version of policy network to avoid overfitting.
  - Value network
    AlphaGo predict the outcome of games using neuarl network.
    This network approximate the value function for the policy (using RL policy network) that converges asymptotically to optimal play.

- A new search algorithm combines Monte Carlo simulation with value and policy networks
  - Selection: traverse the tree according to the action value plus a search control function
  - Expansion: when the search depth exceeds a threshold, expand the leaf node using SL policy network
  - Evaluation: the leaf positions are evaluated by the value networks and the outcome of rollout using fast rollout policy
  - Backup: at end of simulation, the action values are updated backward

### Result

- The SL policy network predicted expert moves with an accuracy of 57.0% compared to state-of-the-art from other research group of 44.4%
- The RL policy network won 85% of games against strongest open-source Go program Pachi. The previous state-of-the-art won 11% of games.
- AlphaGo has 99.8% winning rate against other Go programs
- Distributed version of AlphaGo won 77% of games and 100% against other Go programs
- AlphaGo defeated the human European Go champion by 5 games to 0

By combining tree search with deep neural network, AlphaGo reached a professional level in Go.
