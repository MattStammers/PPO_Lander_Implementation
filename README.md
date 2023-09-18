# PPO_Lander_Implementation

By Matt Stammers

18/09/2023

After now spending 3 months learning reinforcement learning it is now time to write out a full PPO implementation for the lunar-lander environment. 

This is based on Costa Huang's excellent tutorial [PPO_Tutorial](https://www.youtube.com/watch?v=MEt6rrxH8W4)

## Starting Out
1. Step 1

Firstly I made sure that everything was working correctly using CartPole-v1 and 100,000 training steps

This gave me the following result:

![CartPole](https://github.com/MattStammers/PPO_Lander_Implementation/blob/main/video/CartPole-v1-run1.gif)

and a score of 257.70+/- 91.25 with the following very jumpy training:

![Training](https://github.com/MattStammers/PPO_Lander_Implementation/blob/main/images/CartPole-v1.JPG)

## Tuning Up (or down!)
2. Step 2

To fix the jumpy training I reduced the learning and clip rates while increasing the total number of timesteps to 1 million. I kept the decay on the learning rate as it was. Training as would be expected took quite a bit longer this time but after about 300,000 timesteps the model was clearly superior although still jumpy. By about 700,000 timesteps the algorithm had only a very low learning rate but was still quite inconsistent although sometimes it did manage a perfect score. Given the focus was on other environemnts I chose to move on at this point with an imperfect CartPole implementation. You can see the finally deployed model but far from perfect model here - (a policy only algo would have outperformed this!)

[CartPole-TestDeployment](huggingface.co/MattStammers/ppo-Cartpole-v1-fullcoded)

## Another Environment
3. Step 3

Now before going back to lunar-lander I want to try and make this work on the MountainCar-v0 environment because I have never tried this environment before and will need to think carefully to make it work.

Iniitally this one absolutely sucked but I gradually increased the learning rate and by 2.5e-3 after about 150,000 timesteps I started getting some better than base (-200) return. I jacked the training steps up to and started getting some very good results by 500,000 steps but they were inconsistent so I continued the training to 1 million.By this point it didn't seem to be getting any better so I stopped it.

It wasn't too bad and I got -124+/-- 35.38 which is not amazing but also not terrible, especially given that I have never even looked at this particular environment before.

Here is the video of it during training:

![MountainCar](https://github.com/MattStammers/PPO_Lander_Implementation/blob/main/video/MountainCar-v0.gif)

and here is the link to the final result:

https://huggingface.co/MattStammers/ppo-MountainCar-v0-fullcoded 

## LunarLander
4. Step 4

Now its time to take on the lunarlander. This is a more complex environment so I have increased the bactch sizes, reduced the training rate and started with 100,000 training steps. Let's see how we go!

Ok so sadly all on board have perished:

![LunarLanderv1](https://github.com/MattStammers/PPO_Lander_Implementation/blob/main/video/LunarLander-v1.gif)

Now let's try to increase the timesteps to a million and see if we get a better result:

![LunarLanderv2](https://github.com/MattStammers/PPO_Lander_Implementation/blob/main/video/LunarLander-v2.gif)

This time is marginally better but they still don't land. If we look at the training we can see that the training is very lumpy:

![Training1](https://github.com/MattStammers/PPO_Lander_Implementation/blob/main/images/Lumpy_Training.JPG)

I could at this point remove the random seeding in order to debug but I don't really want to do that. Instead I doubled the batch size to 2048 and made the network bigger doubling the nodes from 64 to 128.

However, I noticed while doing this that the losses/entropy dropped off really quickly which is a poor price to pay for stability so I terminated the run and tried the same architecture this time with a smaller learning rate of 2.5e-5 rather than 4. This kept the entropy much higher and also dropped the loss (purple run = 4). 

![Training2](https://github.com/MattStammers/PPO_Lander_Implementation/blob/main/images/First_Four_Trainings.JPG)

This also gave me better overall losses/value_loss returns. Training was still a bit spiky but seemed to be heading in the right direction as by 500,000 timesteps I was starting to get a few positive scores this time but the final result was still not good.

Next I wondered if perhaps increasing the number of environments running in parallel might improve the training experience so I increased these from 4 to 16 with now 2 million timesteps (run 5) and also increasing the clip-coefficient to make sure the policy updates properly (run 6) but this actually made things worse.

For run 7 I increased the learning rate again back to and halved the clip rate back to 2.5e-4. This seemed to help so I then reduced the step-size back to 1024. This time at 500,000 steps I started getting some consistently positive results and the training times started to jump accordingly.  I was reading this thesis while doing it: https://fse.studenttheses.ub.rug.nl/25709/1/mAI_2021_BickD.pdf which really helped me understand the PPO architecture and the clip function. This ultimately led to being able to unravel the puzzle in 8 attempts. The lander doesn't always land but there is at least a chance of survival this time ;D:

![LunarLanderv2](https://github.com/MattStammers/PPO_Lander_Implementation/blob/main/video/LunarLander-v3.gif)

Here is the training dasbhoard:

![Training3](https://github.com/MattStammers/PPO_Lander_Implementation/blob/main/images/First_Four_Trainings.JPG)

and here is the final huggingface repo:

![HuggingFace_Repo](https://huggingface.co/MattStammers/ppo-LunarLander-v2-fullcoded)