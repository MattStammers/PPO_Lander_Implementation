# PPO_Lander_Implementation

By Matt Stammers

18/09/2023

After now spending 3 months learning re-inforcement learning it is now time to write out a full PPO implementation for the lunar-lander environment. 

This is based on Costa Huang's excellent tutorial [PPO_Tutorial](youtube.com/watch?v=MEt6rrxH8W4)

## Starting Out
1. Step 1

Firstly I made sure that everything was working correctly using CartPole-v1 and 100,000 training steps

This gave me the following result:

![CartPole](https://github.com/MattStammers/PPO_Lander_Implementation/blob/main/video/CartPole-v1.gif)

and a score of 257.70+/- 91.25 with the following very jumpy training:

![Training](https://github.com/MattStammers/PPO_Lander_Implementation/blob/main/images/CartPole-v1.JPG)

## Tuning Up (or down!)
2. Step 2

To fix the jumpy training I reduced the learning and clip rates while increasing the total number of timesteps to 1 million. I kept the decay on the learning rate as it was. Training as would be expected took quite a bit longer this time but after about 300,000 timesteps the model was clearly superior although still jumpy. By about 700,000 timesteps the algorithm had only a very low learning rate but was still quite inconsistent although sometimes it did manage a perfect score. Given the focus was on other environemnts I chose to move on at this point with an imperfect CartPole implementation. You can see the finally deployed model but far from perfect model here - (a policy only algo would have outperformed this!)

[CartPole-TestDeployment](huggingface.co/MattStammers/ppo-Cartpole-v1-fullcoded)

## Another Environment
3. Step 3

Now before going back to lunar-lander I want to try and make this work on the MountainCar-v0 environment because I have never tried this environment before and will need to think carefully to make it work.