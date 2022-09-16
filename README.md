# Context-Aware-Task-Oriented-Grasping-with-Multistage-Reinforcement-Learning


### Concept ####
Perceive object features with multiple sensors attached to the robot arm. Task designations are represented as embeddings extracted from task instructions. Grasp topologies are generated based on object affordance and task designations with a grasp selection network. The predicted grasp topology is deployed adaptively using a multistage reinforcement learning approach.
<img src="./images/concept.png" width="50%">

### Framework ####
Human instructions are parsed with the Controlled Robot Language, and the "action" and "object" are extracted. The "object" identifies the target, and then the object affordances are gathered with the MagicHand Platform. The "action" is processed with word2Vec and embeded as 300 dimensional vector. A multi-label MLP network trained with the task-oriented grasping dataset is desigened to map object affordances and task designations to grasp topologies. The grasping task is decomposed into a series of consecutive sub-tasks and deployed with the multistage reinforcement learning model.
<img src="./images/framework.png" width="80%">

Some example of the task-oriented grasping dataset used in this work is shown below

<img src="./images/dataset.png" width="50%">

### Multistage Reinforcement Learning ####
Reinforcement learning is structured with Markov Decision Processes (MDP) that can be presented as a tuple of five components M(S, A, p, R, γ). The goal is to find an optimized policy π that maximizes the return G from every state

<img src="./images/g.png" width="30%">

For standard reinforcement learning model R is a single funciton provides feedback for an action. In the multistage learning model, we design the reward function as a stage-wise function, each piece corresponding to a stage (sub-task) of the problem

<img src="./images/multi-r.png" width="30%">

The advance of the proximal policy optimization (PPO) algorithm now can be modeled as 

<img src="./images/advance.png" width="40%">

and the proximal policy optimization algorithm can be expressed as

<img src="./images/ppo.png" width="30%">

### Solving a Grasping Task ####

We decompose a grasping task into 4 stages: the initial stage, the approaching stage, the grasping stage, and the termination stage. The initial stage represents the initial configurations of the environment, and every grasping task starts at this stage. In the approaching stage, the robot hand leaves the initial position and approaches the object. Once the robot hand is in the grasp location, the grasping stage begins. In the grasping stage, the movement
of the arm is limited, and the robotic hand starts moving the fingers and attempting to grasp the object. Three possible situations could terminate the current task, thus three possible termination stages. The first situation could happen both in the approaching stage and the grasping stage when the object is out of the workspace. The second and the third termination stages present the result of the grasping which could be either a success or a failure, and can only happen after the grasping stage. After the termination stage, the system will terminate the current task, reset the environment to initial configurations, and start a new grasping task

<img src="./images/multistage.png" width="50%">

The stage-wise reward function is modeled as 

<img src="./images/reward.png" width="50%">

The observation space includes norm contact force, distace between the hand and the target, the relative position and orientation
between the hand and the object, and dimensions and shape of the object. Different stages also have different action spaces. In the approaching space the action space include the amount of change of the coordinates of the palm along the x, y, and z axis. In this grasping stage, control of fingers is added to its action space. 


### Experiments ####

#### Grasp results in simulation environment

A total of 16,000 grasping tasks with eight grasp topologies on random unseen objects were proformed in the simulation. The accuracy average accuracy was 90.8%

<img src="./images/simu.png" width="70%">
Video1: https://user-images.githubusercontent.com/63925943/190689351-8aa4cd06-2cf9-4c75-aa5c-b1ce567b8d7e.MP4

Video2: https://user-images.githubusercontent.com/63925943/190689219-273b2330-0a52-4491-abc0-b3e3c41b3a93.MP4

#### Grasp results on Real Robot 

A total number of 320 grasping tasks were performed and the average success rate achieved 80.6%.

<img src="./images/real.png" width="70%">
Video1: https://user-images.githubusercontent.com/63925943/190689595-aa72e9b2-5f8c-41cd-b417-52e4485ff609.mp4

Video2: https://user-images.githubusercontent.com/63925943/190689611-5fd3997b-cdef-4633-b31e-7492fbfa8e30.mp4

#### Comparsion results

The multistage grasp deplyment model is also compared with planning-based method and standard reinforcement learning, the comparsion results show that the proposed method outperforms the planning-based method and standard reinforcement learning in in accuracy, data efficiency, robustness, and generality.

##### Comparsion with Standard PPO

<img src="./images/single.png" width="70%">


##### Comparsion with Planning-Based Model

<img src="./images/plan.png" width="70%">


