# follwer-robot
전북대학교 2024년 2학기 산학실전캡스톤에서 진행한 프로젝트

# 주제 : 모바일 로봇을 이용한 사람 추종 기술 개발
ROS를 기반으로한 로봇이 사람을 따라다니며 보조한다.

사용 로봇 기종 : LIMO Pro (WeGo 로보틱스)

개발기간 : 2024. 9. 26 ~ 2024. 00. 00
|기간|내용|
|:-:|:-:|
|**9. 26 ~ 10. 10**|ROS2 기초 학습|
|**10. 11 ~ 10. 28**|로봇 구동, 객체 인식|
|**10. 29 ~ 11. 11**|사람 추종 알고리즘 개발|
|**11. 12 ~ 11. 30**|러닝 메이트 기능 추가|
|왼쪽정렬|오른쪽정렬|


# 개발환경
* 우분투 : 22.04
* ROS2 humble
## [ROS2 humble 설치](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html#) (Robot Operating System)
```
sudo apt update
sudo apt upgrade
sudo apt install ros-humble-desktop
```
### ROS 설치 검증

터미널 실행시마다 source 입력을 해줘야함
```
source /opt/ros/humble/setup.bash
```
자동실행
```
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
```
* 터미널1
```
ros2 run demo_nodes_py talker
```
* 터미널2
```
ros2 run demo_nodes_py listener
```
통신이 되는지 확인

---

**1. yolo로 사람 객체인식, 토픽으로 이미지를 발행**

**2. 객체 인식된 이미지에서 바운딩 박스의 좌표를 이용해 로봇 월드의 좌표로 변환**

**3. navigation 기능을 이용, 목표 좌표로 주행**

---

## 1. segment_human.py
- 카메라에서 발행되는 이미지를 구독
- 그 이미지에서, yolo를 이용해 사람 객체 검출
- 검출한 사람의 bounding box 좌표를 이용해 3D 좌표로 변환 -> publish
  
**def image_listener_callback(self, msg)**

0. 사람을 표시한 boungding box가 존재하지 않는다면, No Human, 함수 종료
1. 존재한다면, target_pixel(bounding box의 가운데 좌표) 기록
2. bounding box를 그린 이미지 publish (필수x, 시각적으로 확인용)

**def depth_callback(self, msg)**

0. target_pixel이 image_listener_callback에서 기록되지 않았다면, 함수 종료 (= 사람을 발견하지 못했다면)
1. 기록되었다면 깊이를 이용해 z축을 포함한 3D 공간 좌표로 변환 -> ROS2 좌표계로 변환 (디폴트 옵티컬 좌표계)
2. cmd_robot.py에서 구독할 수 있도록 좌표 발행 (x, y, z)

**def imageDepthInfoCallback(self, cameraInfo)**

0. 3D 좌표로 변환시키기 위해 필요한 카메라 정보 저장

## 2. cmd_robot.py
- segment_human.py 에서 받은 3D 좌표 -> (x,y) 값 이용해서 각도, 거리 계산
- cmd_vel 발행해서 로봇 이동

**def time_callback(self)**
1. 1초마다 현재시간을 기록
2. 사람이 마지막으로 발견된 시간으로부터 n초 이상 지나면 rotate_to_find_person() 실행

**def pallet_callback(self, msg)**
1. segment_human.py 에서 발행된 좌표 구독
2. 좌표 받은 시간 기록
3. (x, y) 피타고라스 정리를 이용해 거리 계산
4. 거리가 1m 초과라면
5. 각도 계산
6. 거리, 각도에 따른 속도 조절
7. cmd_vel 토픽 발행해서 로봇 이동
8. 거리가 1m 이하라면 stop_robot

**def rotate_to_find_person(self)**

0. 마지막으로 회전한 방향으로 회전

**def stop_robot(self)**

0. 로봇 정지 cmd_vel 토픽 발행

**def calculate_distance(self)**

0. 피타고라스 정리를 이용해 거리 계산

**def calculate_yaw_error(self)**

0. math.atan2 를 이용해서 각도 계산

**def odom_callback(self, msg)**

0. 현재 pose 저장
