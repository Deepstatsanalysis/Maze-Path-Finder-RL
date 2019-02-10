# MMAI 894 RL Project:
Maze-path-finder

## Getting Started
These instructions will get you started on playing around with the environment and config the algorithm

### Environment:
from MazeEnv import MazeGen maze = MazeGen(num_x, num_y) maze.render()

random_start not in place yet.

Available functions are maze.reset(), maze.set_grid(), maze.set_reward(), and maze.render(). Use maze.set_reward() to change reward of Star (default 10), Trap (default -10), and Step (default -1). The default reward for Terminal is 100.

Use maze.set_grid() to add Star, Trap, or reset Player, and Terminal. Default num of Player and Terminal are one. !! Adding Stars will significantly increase the complexity of this problem. As adding stars will significantly increase dimensions of states. Adding traps will not cause this issue.

Use maze.render() to plot.

### Algorithm

#### MarteCarlo: 
alg = MonteCarlo(maze, discount = 0.9, epsilon=0.2) # epsilon controls the scale of exploration, discount controls how much return cares for future actions alg.render() alg.run_algorithm(10000)

alg.plot_last_episode() # use this to plot the last episode, not necessarily the best. Codes need modification to record all results.

#### SARSA: 
alg = SARSA(maze, discount = 0.9, epsilon=0.1, alpha = 0.9) # epsilon controls the scale of exploration, discount controls how much return cares for future actions, alpha controls the step size alg.render() alg.run_algorithm(10000)

alg.plot_last_episode() # use this to plot the last episode, not necessarily the best. Codes need modification to record all results.

#### Q-Learning 
Same as SARSA


### Limitations:

When Agent finds a star, it starts to use the completely new q_value sets (we call it scenario), with all zeros, hence starts to learn from scratch. Even with 10,000 episodes, how many times does it run into that scenario? Hence the learning could be weak.

### Analysis:

alg.loc_record, alg.reward_record, alg.action_record will keep record of results of each episode. To keep record of all episodes, one can add two lines in function run_algorithm:
eg.:

def run_algorithm(self,num_episode = 1,reduce_eps = False):
    self._eps_step = self._epsilon_copy/num_episode
    
    self._env.reset()
    
    self._env._star = deepcopy(self._star)
    self._epsilon = deepcopy(self._epsilon_copy)
    
    self._init_all_states()
    self._init_q_values()
    self._init_action_count()
    
    self.all_loc_record = []    <-  Add this

    
    for i in range(num_episode):
        if reduce_eps:
            self._epsilon -= self._eps_step
            
        self._env._star = deepcopy(self._star)
        
        self._run_episode()
        self._update_q_values()
        
        self.all_loc_record.append(self.loc_record).  <- Add this
