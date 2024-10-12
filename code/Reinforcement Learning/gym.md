# gym 入门教程

## 安装

## 用 gym 进行实验

```python
import gym
env = gym.make('CartPole-v0')# 创建一个环境，括号里的内容是求解的对象，这里是小推车模型。
state = env.reset()#重置环境，这里是重置小推车和棒子的位置，用于记录初始状态。

for _ in range(1000):
    env.render()#渲染，生成一个窗口，用来显示当前迭代对象。
    print(state)

    action = env.action_space.sample()# 随机选择一个动作，这里是小推车向左或者向右移动。这里采用的是均匀抽样，并不符合实际情况，这里仅做示例。
    state, reward, done, info = env.step(action)#状态、奖励、是否结束、其他信息。

    if done:#done为True，则结束游戏，重置游戏。
        print("finished")
        break

env.close()

```
