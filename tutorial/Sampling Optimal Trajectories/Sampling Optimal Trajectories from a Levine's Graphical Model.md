
## Sampling optimal trajectories 
Now that we have established the corrosponding classicical methodologies (max entropy RL with GAE), we can build to using variational inference to determine optimal policies. In order to do this, we assume that we can sample optimal policies by interacting with the enviroment. In practice, this just means that we create an agent that performs uniformly random actions within the space, generate trajctories from this agent, and then sample those trajectories based upon the pdf. This pdf is given in https://arxiv.org/abs/1805.00909, and efffectively creates a product of exponential rewards associated with each state. In this way, trajectories that yeilded higher rewards are more likly to be sampled. In order to sample these trajectories, I apply the Metropolis-Hastings Algorithm, however in the future I might impliment Metropolis-in-Gibbs or Hamiltonian-Monte-Carlo. The first chunk of code below is the random walk policy discussed above.


```python
%run running_codebase.ipynb
```


```python
class RANDOM_WALK(torch.nn.Module):
    def __init__(self, state_size, action_size):
        super(RANDOM_WALK, self).__init__()
        self.state_size = state_size
        self.action_size = action_size
        
    def sample_action(self, state):
        probabilities = torch.ones(self.action_size) / self.action_size
        action = torch.distributions.Categorical(probabilities)
        return action.sample()
        
    def logprob_action(self, state, action):
        probabilities = torch.ones(state_size)/state_size
        action = torch.distributions.Categorical(probabilities)
        log_prob = min(0, torch.log(probabilities[action.long()]+0.0001))
        return log_prob
        
```

## Sampling full trajectories
Now that we have a random walk policy we can start sampling trajectoriess. We will start by attempting to sample full trajectories of the expert based upon the reward function prior to moving onto state-action pairs. In order to do this, all we need is we will apply the random walk policy and sample trajectories that yeilded good rewards. An interesting point would be to sample states with a high value function instead reward. and then sample based upon that. This might be somewhart easier for state action sampling as it adds a chain of causality and wont allow the true distribution density so senter around the mean.


```python
def sample_optimal_trajectory(samples, T, grid, reward_shape, iterations, burn_in):
    
    # initialize random policy 
    rw = RANDOM_WALK(2,5)
    policy = lambda state: rw.sample_action(state)
    
    # initialize joint pdf function Liklyhood*prior
    log_pdf = lambda action_size, rewards: torch.tensor([ \
        torch.log(torch.tensor(torch.exp(reward)*(1/action_size))) \
        for reward in rewards])
    
    # log joint pdf 
    log_joint_pdf = lambda action_size, rewards: sum(log_pdf(action_size, rewards))
    
    # get initial sample info
    state_tensor, action_tensor, reward_tensor = grid.simulate_trajectory(T, 1, policy)
    state_samples = torch.zeros((2,T,iterations))
    action_samples = torch.zeros((1,T,iterations))
    initial_sample_pdf = log_joint_pdf(2, reward_tensor)
    
    # initial state
    state_samples[:,:,0] = state_tensor.reshape(2,T)
    action_samples[:,:,0] = action_tensor.reshape(1,T)
        
    # sample trajectory from the posterior
    for i in range(1, iterations):
        
        # get sampled trajectories
        state_tensor, action_tensor, reward_tensor = grid.simulate_trajectory(T, 1, policy) 
        
        # reshape rewards 
        reward_tensor = torch.tensor([[reward_shape(reward_t) for reward_t in reward_ts] \
                                      for reward_ts in reward_tensor])
        
        # get pdf of new sample
        new_sample_pdf = log_joint_pdf(2, reward_tensor)
        
        # get acceptance ratio
        A_ratio = torch.tensor(min(0.0, new_sample_pdf - initial_sample_pdf))
        
        # create uniform rv
        unif = torch.rand(1)
        
        # accept / reject
        if unif <= torch.exp(A_ratio):
            
            # update current pdf
            initial_sample_pdf = new_sample_pdf
            
            # append new tensors to the current set of samples
            state_samples[:,:,i] = state_tensor.reshape(2,T)
            action_samples[:,:,i] = action_tensor.reshape(1,T)
        
        else: 
            
            # append old tensors to the current set of samples
            state_samples[:,:,i] = state_samples[:,:,i-1].reshape(2,T)
            action_samples[:,:,i] = action_samples[:,:,i-1].reshape(1,T)
            
    # now we burn in
    
    return state_samples[:,:,burn_in:], action_samples[:,:,burn_in:] 

```


```python
pre_determined_grid = [[-1,   -1,  -1,   -10,   -1, 10**4],
                       [-100, -1,  -10,  -1,   -10, -10],
                       [-1,   -10, -100, -10,  -1, -1],
                       [-1,   -1,  -100, -10,  -1, -1],
                       [-10,  -1,  -100, -100, -100, -1],
                       [-1,   -1,  -100, -100, -100, -1]]

# hyper parameters
T = 100
samples = 1
reward_shape = lambda reward: reward*100

# markov chain parameters
iterations = 3000
burn_in = 1999

#initializations
start_state = [5,0]
policy_obj = POLICY(2, 5)
grid = GRID_WORLD(pre_determined_grid, start_state)

# sample optimal trajectories
state_samples, action_samples = sample_optimal_trajectory(samples, T, grid, reward_shape, iterations, burn_in)
print(state_samples[:,-1,-1])
print(state_samples[:,-3,-1])
print(state_samples[:,-9,-1])
print(state_samples[:,-13,-1])

```

    tensor([3., 0.])
    tensor([4., 0.])
    tensor([3., 1.])
    tensor([2., 1.])


## State-Action based sampling 
Now that we have successfully implimented the full trajectory code, we can work with the local sampling code from before. As can be seen above, when we deal with full trajectories, the agent tends to only consider trajectories that are "safe", as the cumulative reward over long periods of time can vary widely. In order to gaurentee that we will see optimal states that wont be eclipsed by low intermediary state rewards, we will now only look at individual state action pairs. Origininally I was somewhat worried that the setup would over sample values that were high reward without incorporating intermediary states, however if we apply metropolis hastings for each timestep (and in doing so sample states that also have an associated time), we avoid this issue for the most part. It does however only sample values along the trajectory that have the lowest value for that time step, which gives us effectively non-continuous trajectories! Is this an issue? Not for what we will be using these trajectories for (at least for the simple grid world)! 


```python
def sample_optimal_state_actions(samples, T, grid, reward_shape, burn_in):
    
    # initialize random policy 
    rw = RANDOM_WALK(2,5)
    policy = lambda state: rw.sample_action(state)
    
    # initialize pdf function Liklyhood*prior
    log_pdf = lambda action_size, reward: \
        torch.log(torch.min(torch.tensor([torch.exp(reward)*(1/action_size), 1.])))
    
    # initialize sample info
    state_samples = torch.zeros((2,T,samples))
    action_samples = torch.zeros((T,samples))
    reward_samples = torch.zeros((T,samples)) 
    
    # get sampled trajectories
    state_tensor, action_tensor, reward_tensor = grid.simulate_trajectory(T, samples, policy) 
    
    # shape reward tensor
    reward_tensor = torch.tensor([[reward_shape(reward_i) for reward_i in rewards] for rewards in reward_tensor])
    
    # iterate through all timesteps
    for t in range(T):
        
        # initial state at time t
        state_samples[:,t,0] = state_tensor[:,t,0]
        action_samples[t,0] = action_tensor[t,0]
        reward_samples[t,0] = reward_tensor[t,0]
        
        # initial log pdf at time t
        initial_sample_pdf = log_pdf(2, reward_tensor[t,0])
        
        # sample trajectory from the posterior at time t
        for i in range(1, samples):
            
            # get pdf of new sample
            new_sample_pdf = log_pdf(2, reward_tensor[t,i])

            # get acceptance ratio
            A_ratio =   torch.tensor(min(0.0, new_sample_pdf - initial_sample_pdf))
            
            # create uniform rv
            unif = torch.rand(1)

            # accept / reject
            if unif <= torch.exp(A_ratio):
                
                # update current pdf
                initial_sample_pdf = new_sample_pdf
    
                # append new tensors to the current set of samples
                state_samples[:,t,i] = state_tensor[:,t,i]
                action_samples[t,i] = action_tensor[t,i]
                reward_samples[t,i] = reward_tensor[t,i]
                
            else: 

                # append old tensors to the current set of samples
                state_samples[:,t,i] = state_samples[:,t,i-1]
                action_samples[t,i] = action_samples[t,i-1]
                reward_samples[t,0] = reward_tensor[t,i-1]
        
    # now we burn in
    return state_samples[:,:,burn_in:], action_samples[:,burn_in:], reward_samples[:,burn_in:]

```


```python
pre_determined_grid = [[-1,   -1,  -1,   -10,   -1, 10**4],
                       [-100, -1,  -10,  -1,   -10, -10],
                       [-1,   -10, -100, -10,  -1, -1],
                       [-1,   -1,  -100, -10,  -1, -1],
                       [-10,  -1,  -100, -100, -100, -1],
                       [-1,   -1,  -100, -100, -100, -1]]
# hyper parameters
T = 100
samples = 2000
reward_shape = lambda reward: reward*10 

# markov chain parameters
burn_in = 999

#initializations
start_state = [5,0]
policy_obj = POLICY(2, 5)
grid = GRID_WORLD(pre_determined_grid, start_state)

# sample optimal trajectories
state_samples, action_samples, reward_samples = sample_optimal_state_actions(samples, T, grid, reward_shape, burn_in)

# check a couple of the last states to be sampled
print(state_samples[:,-1,-1])
print(state_samples[:,-3,-1])
print(state_samples[:,-9,-1])
print(state_samples[:,-13,-1])
```

    tensor([0., 5.])
    tensor([0., 5.])
    tensor([0., 5.])
    tensor([0., 5.])


## Whats next?
Now that we have a slick way to sample optimal trajectories within our space, what are we going to do with this information? Well ideally we want to be able to create a policy that can solve this maze, and now that we can sample these (albeit discontinuous) trajectories why not just try to match those! Next time we will set up the variational inference problem, and see if we can train a variational distribution to match the sampled one that we just create!
