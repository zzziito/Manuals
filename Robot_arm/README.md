# Robot Arm

## Intro

로봇팔에 대한 전반적인 내용과, 
Braccio 로봇팔로 수행하는 pick and place 를 다룹니다. 

## Videos

1. 박스를 쌓는 로봇팔

[![braccio stack Youtube](https://img.youtube.com/vi/yRtQsGaJM6Y/maxresdefault.jpg)](https://youtu.be/yRtQsGaJM6Y)


#### 1,2,3 Section 별로 keyboard 입력 받아서 움직이는 Gazebo Simulator 

아래쪽 판자를 왼쪽에서부터 1,2,3 이라고 하여 <br>
키보드 입력으로 (1 2) , (1 3) 등을 입력하면 1번 판자로부터 2번 판자로, 1번 판자로부터 3번 판자까지 pick and place 를 수행하는 simulator 입니다. 

![ezgif com-gif-maker (1)](https://user-images.githubusercontent.com/52185595/155872869-de41b96b-9cb2-417c-86d2-3e8801ac0167.gif)

#### 1,2,3 Section 별로 keyboard 입력 받아서 움직이는 Braccio Arm 

![ezgif com-gif-maker (2)](https://user-images.githubusercontent.com/52185595/155873061-0f489262-2420-462e-86d9-2bef8fe288d3.gif)


#### xy 평면 상에서 시작점, 끝점 좌표 입력 받아서 움직이는 Gazebo Simulator

키보드 입력으로 start x, start y, end x, end y 값을 받아 움직이는 시뮬레이터입니다. 

![ezgif com-gif-maker (6)](https://user-images.githubusercontent.com/52185595/155873190-488e1548-3512-42d0-97f2-1168158cb01c.gif)

#### Simulator 와 동일하게 움직이는 Braccio Arm 

![ezgif com-gif-maker (9)](https://user-images.githubusercontent.com/52185595/155912569-702d7a64-c3b0-4719-9fe5-b43d1f7d7cc0.gif)

##### Joint State Publisher

ROS 의 Joint State Publisher 를 이용해서 Rviz 화면과 동일하게 움직이는 braccio 입니다.

![ezgif com-gif-maker (8)](https://user-images.githubusercontent.com/52185595/155873317-11bca673-788a-4112-aba9-2e3099d3f077.gif)


#### 물체를 들어올려 쌓을 수 있는 Gazebo Simulator



## 목차

1. 로봇팔 개요<br>
	1-1. 자주 나오는 용어 설명<br>
    1-2. Inverse Kinematics<br>
    1-3. MoveIt!<br>
    1-4. Braccio Robot Arm <br>
2. Braccio 로 pick and place 수행하기<br> 
	2-1. 브라키오에서 ROS 를 사용할 수 있게 세팅하기<br>
	2-2. 상자를 keyboard input 에 따라 옮기기<br>
    2-3. 상자를 쌓기 <br>

### 1. 로봇팔 개요

#### 1-1. 자주 나오는 용어 설명

- **URDF**
Unified Robot Description Format
로봇 모델에 대한 정보들을 기술해 놓은 규격서. 

- **MoveIt**
매니퓰레이터를 위한 통합 라이브러리. 
모션 플래닝을 위한 빠른 역기구학 해석, 매니퓰레이션을 위한 고급 알고리즘, 로봇 핸드 제어, 동역학, 제어기 등 다양한 기능을 제공하고 있다. 

- **Forward Kinematics**
로봇의 각 관절 각도가 주어졌을 때 말단 장치의 위치 및 자세를 구하는 것

- **Inverse Kinematics**
말단 장치의 위치와 자세가 주어졌을 때 각 관절 각도를 구하는 것. 

#### 1-4. Braccio Robot Arm

<img src="https://images.velog.io/images/zzziito/post/a9c5d984-5389-4d96-a58a-a5b74be6f33b/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202022-02-28%20%EC%98%A4%ED%9B%84%202.58.08.png" width="300" height="400">

브라키오는 6축 로봇팔입니다. 

### 2. Braccio 로 pick and place 수행하기 


#### 2-1. 브라키오에서 ROS 를 사용할 수 있게 세팅하기

<img src="https://images.velog.io/images/zzziito/post/2701d48d-7aec-4247-8c0a-bd0f8e8b2df3/IMG_9094.jpg" width="300" height="300">

브라키오는 다음과 같이 아두이노 우노와 부착된 Shield 로 구성되어 있습니다. 

![](https://images.velog.io/images/zzziito/post/2f776030-860c-4e47-895e-2855d9cfc9ae/HardwareArchitecture.jpeg)

브라키오는 다음과 같이 아두이노와 쉴드에 부착되어 있습니다. 이 아두이노가 ROS 를 사용할 수 있게 하려면, 따로 설정을 해 주어야 합니다. 
Setup 에 관해서는 [이곳](http://wiki.ros.org/rosserial_arduino/Tutorials/Arduino%20IDE%20Setup) 을 참고해 주세요. 
이후 braccio/braccio_ros.ino 파일을 아두이노에 업로드해주세요. 

```
Error opening serial : could not open port /dev/ttyACM0 Permission denied 
```
라는 에러가 떴다면 

```
$ sudo chmod 666 /dev/ttyACM0
```

를 입력하여 포트를 모든 사용자가 사용할 수 있도록 권한을 부여해 주세요. 


#### 2-2. 상자를 keyboard input 에 따라 옮기기

**Gazebo**

가제보는 가상 시뮬레이션 툴입니다. <br>
URDF, SDF, Xacro 등의 XML 문서로 사용자가 원하는 로봇을 가상 시뮬레이션에 삽입한 후, 삽입된 로봇과 사용자가 작성한 프로그램이 메시지를 주고 받으며 동작하는 모습을 확인할 수 있습니다.

launch 파일을 살펴 보면, 
```
<include file="$(find gazebo_ros)/launch/empty_world.launch">
  <arg name="world_name" value="$(find braccio_moveit_gazebo)/worlds/123_box.world"/>
  <arg name="debug" value="$(arg debug)" />
  <arg name="gui" value="$(arg gui)" />
  <arg name="paused" value="$(arg paused)"/>
  <arg name="use_sim_time" value="$(arg use_sim_time)"/>
  <arg name="headless" value="$(arg headless)"/>
</include>
```

이 프로젝트에서는 123_box.world 파일로서 world 를 생성한다는 것을 볼 수 있습니다. 
 
이 world 는 사용자가 코드를 통해 임의로 장애물들을 생성하고, 이들의 inertia, 중력가속도 등을 조절할 수 있습니다. 

world 파일 작성법에 관해서는 [이곳](http://playerstage.sourceforge.net/doc/Gazebo-manual-0.8.0-pre1-html/config_syntax.html)을 참고해 주세요. 

![](https://images.velog.io/images/zzziito/post/9a01246c-749c-4b31-8d74-26b3c7e3f46a/no_gravity.gif)

예를 들어 gravity 값을 0 0 0 으로 설정해준다면, 다음과 같은 움직임을 얻을 수 있습니다. 

**코드 설명**

**Libraries**

```python
import sys
import rospy
import moveit_commander
import time
from gazebo_msgs.msg import LinkStates, ModelState
from geometry_msgs.msg import Pose
from gazebo_msgs.srv import SetModelState
import numpy as np
import scipy.optimize
import cv2
import json
```

```python
```

**moveit_commander**

moveit_commander 파이썬 패키지는 moveit 에서 제공하는 기능에 대한 wrapper 을 제공합니다. 
모션 계획 (motion planning), 데카르트 경로 계산, 선택 및 배치에 대한 간단한 인터페이스를 사용할 수 있습니다. 

```python
moveit_commander.roscpp_initialize(sys.argv)
rospy.init_node('braccio_xy_bb_target', anonymous=True)
```

먼저 moveit commander 와 rospy 를 초기화합니다. rospy 초기화에서 ros 에 사용되는 노드의 이름을 입력합니다. 

```python
group_name = "braccio_arm"
self.move_group = moveit_commander.MoveGroupCommander(group_name)
self.gripper_group = moveit_commander.MoveGroupCommander("braccio_gripper")
```
MoveGroupCommander 를 통해서 로봇의 모션을 생성할 수 있으며, 로봇을 움직이는 명령을 보낼 수 있습니다. 


**rospy.Subscriber**

```python
self.states_sub = rospy.Subscriber("/gazebo/link_states", LinkStates, self.linkstate_callback)
```

/gazebo/link_states 를 subscribe 하여 link state 를 받아옵니다. 

**ModelState**

reset_link 함수를 통해 모델의 관절 각도를 직접 조정할 수 있습니다. 
이때 말하는 모델은 이 프로젝트의 경우 '빨간 박스' 입니다. 
즉, 이 함수를 통해 빨간 박스의 위치를 조정할 수 있습니다. 

```python
def reset_link(self, name, x, y, z):
    state_msg = ModelState()
    state_msg.model_name = name
    state_msg.pose.position.x = float(x)
    state_msg.pose.position.y = float(y)
    state_msg.pose.position.z = float(z)
    state_msg.pose.orientation.x = 0
    state_msg.pose.orientation.y = 0
    state_msg.pose.orientation.z = 0
    state_msg.pose.orientation.w = 0
    rospy.wait_for_service('/gazebo/set_model_state')
    try:
        set_state = rospy.ServiceProxy('/gazebo/set_model_state', SetModelState)
        resp = set_state( state_msg )

    except rospy.ServiceException, e:
        print "Service call failed: %s" % e
```
> What is rospy.ServiceProxy?
rospy.ServiceProxy(name, service_class, persistent=True) Creates a proxy to call a service and enable persistent connections. 

> 여기서 프록시 (Proxy) 란?
프록시란 '대리' 라는 의미로, 주로 직접 통신할 수 없는 두 점 사이에서 통신을 할 경우 그 사이에서 중계기로서 대리로 통신을 수행하는 기능을 가리켜 '프록시' , 그 중계 기능을 하는 것을 프록시 서버라고 부릅니다. 



```python
def reset_target_position(self):
  """reset block and bowl"""
  print 'reset block x='
  x = raw_input()
  print 'reset block y='
  y = raw_input()
  print 'reset block z='
  z = raw_input()
  self.reset_link('unit_box_0', x, y, z)
  self.reset_link('my_mesh', -0.15, -0.325, 0)
```

reset_target_position 함수를 통해 target 인 unit_box_0 의 position 을 키보드 입력을 통해 임의로 조정할 수 있게 됩니다. 

**Position**

get_link_position_box 함수를 통해서 box 의 x,y,z, 위치를 알 수 있습니다. 

```python
def get_link_position_box(self, link_names):
  x = 0
  y = 0
  z = 0
  n = 0
  for l in link_names:
    ind = self.linkstate_data.name.index(l)
    res = self.linkstate_data.pose[ind].position
    print(l)
    print(res)
    print(n)
    x += res.x
    y += res.y
    z += res.z
    n += 1
  return x/n, y/n, z
```

![](https://images.velog.io/images/zzziito/post/0f24be23-f72f-423a-a919-cb007b363af5/z%20index.png)

![](https://images.velog.io/images/zzziito/post/f6390895-e486-4b38-adf5-ba9ef8fa94bf/zindex_2.png)

다음과 같이 position 이 표시되는 것을 볼 수 있습니다. 

**Robot Arm Control**


**go_to_j 함수**

이 함수는 joint_goal 배열 내에 원하는 각도값을 각각 넣어주는 함수입니다. 

```python
def go_to_j(self, j0=None, j1=None, j2=None, j3=None):
  joint_goal = self.move_group.get_current_joint_values()
  if j0 is not None:
    joint_goal[0]=j0
  if j1 is not None:
    joint_goal[1]=j1
  if j2 is not None:
    joint_goal[2]=j2
  if j3 is not None:
    joint_goal[3]=j3
  self.go_to_joint(joint_goal)
```

joint_goal 이라는 배열 속에 순서대로 j0, j1, j2, j3 관절 각도값이 들어 있습니다.
이때 주의할 것은 Arm3Link 가 3축에 대한 역기구학 솔루션을 제공한다는 점입니다. 

이 프로젝트에서 j4 관절은 움직일 필요가 없기 때문에, 지면과 수평하도록 설정해놓고 이후에는 고려하지 않습니다. 
또한 j0 의 경우 물체가 있는 방향으로 움직입니다. 

따라서 역기구학에서는 j1, j2, j3 관절에 대한 움직임만을 계산합니다. 

**Arm3Link**

![](https://images.velog.io/images/zzziito/post/b46f959a-5a2b-4f85-9c50-080e6a99c5ce/xy_invkin-2022-03-03_12.41.42.gif)

따라서 이번 braccio 프로젝트에서는, 6축의 일부를 고정함으로써 3축 inverse kinematics 코드를 적용시킬 수 있었습니다. 

원본 코드는 [이곳](https://studywolf.wordpress.com/2013/04/11/inverse-kinematics-of-3-link-arm-with-constrained-minimization-in-python/)입니다. 

kinematics 에 관한 추가적인 내용은 [Kinematics Matlab simulation]()을 참고해 주세요. 

**go_to_joint 함수**



```python
def go_to_joint(self, joint_targets):
  joint_goal = self.move_group.get_current_joint_values()
  joint_goal[0] = joint_targets[0]
  joint_goal[1] = joint_targets[1]
  joint_goal[2] = joint_targets[2]
  joint_goal[3] = joint_targets[3]
  joint_goal[4] = 1.5708
  ret = self.move_group.go(joint_goal, wait=True)
  self.move_group.stop()
```



**Gripper**

```python
def gripper_close(self):
  self.go_gripper(1.2)

def gripper_open(self):
  self.go_gripper(0.2)

def gripper_middle(self):
  self.go_gripper(0.5)

def go_gripper(self, val):
  joint_goal = self.gripper_group.get_current_joint_values()
  joint_goal[0] = val
  joint_goal[1] = val
  self.gripper_group.go(joint_goal, wait=True)
  self.gripper_group.stop()
```

다음과 같은 함수들로 그리퍼를 컨트롤할 수 있습니다. 
