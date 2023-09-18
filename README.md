# PPO_Lander_Implementation

By Matt Stammers

18/09/2023

After now spending 3 months learning re-inforcement learning it is now time to write out a full PPO implementation for the lunar-lander environment. 

This is based on Costa Huang's excellent tutorial [PPO_Tutorial](youtube.com/watch?v=MEt6rrxH8W4)

## Starting Out
1. Step 1

Firstly I made sure that everything was working correctly using CartPole-v1 and 100,000 training steps

This gave me the following result:

<video src="videos/CartPole-v1__ppo__1__1695047933/rl-video-episode-125.mp4" controls title="Title"></video>

and a score of 257.70+/- 91.25 with the following very jumpy training: [Training](images/CartPole-v1.JPG)