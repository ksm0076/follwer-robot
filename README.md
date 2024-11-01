# follwer-robot
전북대학교 2024년 2학기 - 산학실전캡스톤에서 진행한 프로젝트

# 주제 : 모바일 로봇을 이용한 사람 추종 기술 개발
ROS를 기반으로한 로봇이 사람을 따라다니며 보조한다.

사용 로봇 기종 : LIMO Pro (WeGo 로보틱스)

개발기간 : 2024. 9. 26 ~ 2024. 00. 00
|기간|내용|
|:-:|:-:|
|**9. 26 ~ 10. 10**|ROS2 기초 학습|
|**10. 11 ~ 10. 28**|로봇 구동, 객체 인식|
|**10. 29 ~ 11. 11**|사람 추종 알고리즘 개발|
|**11. 12 ~ 11. 31**||
|왼쪽정렬|오른쪽정렬|


# 개발환경
* 우분투 : 20.04
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
