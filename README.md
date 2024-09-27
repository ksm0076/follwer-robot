# follwer-robot
전북대학교 2024년 2학기 - 산학실전캡스톤에서 진행한 프로젝트

# 주제 : 모바일 로봇을 이용한 사람 추종 기술 개발
ROS를 기반으로한 로봇이 사람을 따라다니며 보조한다.

개발기간 : 2024. 9. 26 ~ 2024. 00. 00

우분투 : 22.04

# 개발환경
* [ROS2 humble 설치](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html#install-ros-2-packages) (로봇구동을 위한 meta OS)
```
sudo apt update
sudo apt upgrade
```
```
sudo apt install ros-humble-desktop-full
```
설치 검증
* 터미널1
```
ros2 run demo_nodes_py talker
```
* 터미널2
```
ros2 run demo_nodes_py listener
```
통신이 되는지 확인

<details>
<summary> WSL 환경에서 ROS2 설치 (https://roytravel.tistory.com/378)</summary>


1. ROS 설치를 위한 universe 저장소 활성화
```
apt-cache policy | grep universe
```
2. ROS2 apt 저장소를 시스템에 추가 & GPG 키 승인
```
sudo apt update && sudo apt install curl gnupg lsb-release
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
```
3. ROS2 저장소를 sources.list에 추가
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(source /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```
4. 우분투 시스템 update
```
sudo apt update
sudo apt upgrade
```
5. ROS2 설치
```
sudo apt install ros-humble-desktop-full
```

터미널 실행시마다 source 입력을 해줘야함
```
source /opt/ros/humble/setup.bash
```
터미널이 실행될 때 자동으로 실행
```
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
```

</details>
