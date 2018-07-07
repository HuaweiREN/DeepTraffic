# DeepTraffic
This repository is for the deep traffic competition held by MIT open course: Deep Learning for Self-Driving Cars, taught by Lex Fridman.
The link to the course materials is: https://selfdrivingcars.mit.edu/, where all the course videos and slides, as well as the competition can be found.

The course includes four competitions, amoung which there are two competitions released to the public. DeepTraffic is one of them that the aim is to control a car driving as fast as possible in the traffic flow.

By setting different parameters and training the model iteratively, the car knows how to alleviate other cars (which are randomly controlled in the traffic) and when to accelerate/decelerate.

The bottom line to pass the competetion is to reach the average speed higher than 65 mph. Luckily, I achieved around 72 mph. The highest score I got is 72.56 mph. It is able to pass the competition, but still far away from those genius on the leaderboard.

The content below is my intuitions and methods to acheve 72.56 mph.

**lanesSide=4 / patchesAhead=40 / patchesBehind=10**

In my opinion, I would like to choose the lanesSide value as 4. The road has 7 lanes in total. when the lanesSide=4, it means that even if the car is on the most left/right lane, we still have a relative large vison. In other words, there are always more than 5 lanes (out of 7) in the vision of the agent, which makes the car faster and easier to find the "exit" to some local congestion situation. Actually I started my model with a relative smaller lanesSide value, the agent appeared to behave poorly because of the lack of the perception on the lanes far away from the current lane. Instead of changing the lane swiftly, the agent chose to decelerate and wait for the opportunity.

Let's assume that there is a talent driver in the car. In my opinion, a real human driver will look to the front as far as possible, so that he is able to have a basic view of what is happening and what would possibly happen in next a couple of time frames. Although the behaviors of other cars are random, some basic information (such as the possibility to have the traffic jam between several lanes) is still captured by the driver if he looks far front all the time. Therefore, I chose patchesAhead=40. It is not very high. In my view, because the driver is looking at the road consistently, it is not necessary to look so far beyond the scope where the agent cannot reach in a very short time.

As for the patchesBehind, I decided to set it as 10. Although the safety system does not include the area behind the agent. It is still necessary to look backwards to reach a higher average speed. It is a good approach to notice if there is other cars (with a higehr speed) surpassing the agent in a couple of time frames. The agent is expected to learn how to alleviate this situation during training.

**trainIterations = 500000**
It is good to have a huge number of iteration during training. I haven't implement with a smaller number to check the performance and time efficiency. It seems that 500,000 is a proper value.

**otherAgents = 0**
This parameter is added to deep traffic competition in 2018. If the number is positive (max 10.), the deep traffic will be trained with some other agents in parallel. Unlike the other cars on the road, the behavior of the agents become predictable by other agents.

**temporal_window = 0**
In my view, if there is only one agent on the road, temporal window should be selected as 1. However, if there are more than one agents, this parameter comes to its role. If several agents are trained in paralell, the action made by other agents are becoming predictable along with the time. In other words, it is able to gain more information instead of facing the situation with pure-random-behavior cars. If the temporal window is still set to 1, we will lose this kind of info.

Last year, when the parameter **otherAgents** is not introduced to the competition, almost all the best results were given by setting temporal window as 0. If the actions are all in random, it is useless to have the temporal window, because even continuous time frames are not able to give the clue on the actions of those pure-random-behavior cars.

**NN_layout: FC30-tanh-FC30-tanh-FC20-tanh-FC20-tanh-FC20-tanh**
