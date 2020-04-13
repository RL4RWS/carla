
CARLA의 주요한 기능들을 다음과 같습니다.
    서버 다중 클라이언트 아키텍처를 통한 확장성: 동일한 노드 또는 다른 노드의 여러 클라이언트가 서로 다른 행위자를 제어할 수 있음

    플렉시블 API : CARLA는 Traffic generation, 보행자 행동, 날씨, 센서 등 시뮬레이션과 관련된 모든 측면을 사용자가 제어할 수 있는 강력한 API를 공개한다.

    자율주행 센서 제품군: 사용자들은 LIDAR, 다중 카메라, 깊이 센서, GPS 등 다양한 센서 제품군을 구성할 수 있다.

    계획 및 제어를 위한 빠른 시뮬레이션: 이 모드는 렌더링을 비활성화하여 그래픽이 필요하지 않은 교통 시뮬레이션과 도로 동작을 빠르게 실행할 수 있도록 한다.

    지도 생성: 사용자는 RoadRunner와 같은 도구를 통해 OpenDrive 표준에 따라 자신의 지도를 쉽게 만들 수 있다.

    트래픽 시나리오 시뮬레이션: 당사 엔진 ScenarioRunner로 사용자는 모듈식 동작에 기반한 다양한 트래픽 상황을 정의하고 실행할 수 있다.

    ROS 통합: CARLA는 ROS-Bridge를 통해 ROS와의 통합을 제공 한다.
    자율 주행 기준선: 우리는 자율 주행 기준선을 AutoWare 에이전트 및 조건부 모방 학습 에이전트를 포함하여 칼라에서 실행 가능한 에이전트로 제공한다.




#Introduction - Self-driving cars with Carla and Python part 1

안녕하십니까, Carla를 다루는 튜토리얼 시리즈에 오신 것을 환영합니다, Carla는 Python API와 함께 제공되는 오픈소스 자율주행 환경입니다.
칼라의 주된 생각은 환경(서버)과 에이전트(클라이언트)를 갖는 것이다. 이 서버/클라이언트 아키텍처는 물론 서버와 클라이언트를 동일한 시스템에서 로컬로 실행할 수 있지만, 하나의 시스템에서 환경(서버)을 실행하고 다른 여러 시스템에서 여러 클라이언트를 실행할 수도 있다는 것을 의미하는데, 이것은 꽤 멋진 것 같습니다.
칼라와 함께 우리는 그 차를 운전할 수 있는 환경인 (분명히) 차를 갖게 되고, 그리고 우리는 실제의 자율주행차 센서를 모방하기 위해 차 위에 놓을 수 있는 많은 센서를 갖습니다. 라이다(LIDAR), 카메라, 가속도계와 같은 센서들 말입니다.
우리의 첫 번째 순서는 실제로 칼라를 다운로드 받는 것입니다. 아주 간단합니다. http://carla.org/ 을 클릭한 다음 최신 릴리스로 스크롤하십시오.

이 튜토리얼의 목적을 위해 Linux에서는 0.9.6, Windows에서는 0.9.5를 모두 사용하고 있다.
둘 다 압축된 파일만 다운로드하면 된다. Linux에 있는 경우 0.9.6을 클릭한 다음 CARLA_0.9.6.6tar.gz를 클릭하십시오.
Windows(윈도우)에 있는 경우 0.9.5로 이동하여 CARLA_0.9.5.zip을 선택하십시오.

나중에 최신 버전을 더 많이 얻을 수 있을 것이다. 그렇게 해도 괜찮으니, 코드 구문을 포함하여 시간이 지남에 따라 상황이 달라질 것이라는 것만 알아두십시오. 내가 사용하고 있는 버전과 동일한 버전으로 학습한 후 나중에 업그레이드하는 것이 가장 좋을 수 있습니다.
일단 당신이 압축된 파일을 얻으면, 당신은 당신이 실행하기 위해 필요한 것을 얻을 것이다. 방금 추출한 파일의 메인 디렉토리에는 Linux에 있는 경우 CarlaUE4.sh이 있거나 Windows에 있는 CarlaUE4.exe가 있을 것이다.
Windows에서 이 옵션을 실행하려면 .exe를 두 번 클릭하십시오. Linux에서 해당 디렉토리에서 터미널을 열고 다음을 수행하십시오.
    
./CarlaUE4.sh

이렇게 하면 서버가 실행된다. 결국 지도를 보게 될 것이다. 이 지도는 WASD 키와 마우스로 탐색할 수 있다. 물론 아직 아무것도 없다. 이것은 단지 우리의 환경일 뿐이다. 시작하기 섹션에는 PythonAPI 디렉토리에서 찾을 수 있는 예를 사용하는 몇 가지 예가 나와 있다.

계속하여 기본 칼라 디렉토리에서 예제로 이동하십시오. PythonAPI/examples 여기에 manual_control.py, dynamic_weather.py, sular_npc.py 등의 파일이 몇 개 있다. 우리는 이것들 중 일부를 확인할 수 있다. 단자/cmd를 열고 manual_control.py를 실행하십시오:

py -3.7 manual_control.py

참고: 여기에서 Python 3.7을 사용해야 하며, 그렇지 않으면 다른 버전의 Python을 위해 직접 Carla를 구축해야 할 것이다.

이렇게 하면 클라이언트/서버가 별도로 있으므로 다른 창이 생성된다. 지금 보시는 것은 Python API로 우리가 할 수 있는 일의 예 입니다. 여기서 당신은 WASD 키로 자동차를 제어할 수 있으며, Q는 역방향으로 혹은 역방향으로 바뀔 것이다.

물론, 이 안에서 수동 운전을 하는 것은 사실 우리가 여기 온 목적이 아니다. 내 목표는 파이썬이 운전하게 놔두는 거야! 시작하려면 다음 튜토리얼의 주제가 될 파이썬 API의 실제 작동 방식을 이해해야 한다.




# Controlling the Car and getting sensor data - Self-driving cars with Carla and Python part 2

이번 절에서는  CARLA의 Python API를 소개하고자 한다.

시작하자면 CARLA에는 여러 종류의 물체가 있다. 첫째, 당신은 당연히 "world"을 가지고 있다. 이것은 시뮬레이션 환경 전체를 가리킨다. 그리고, 이 시뮬레이션 환경에는 Actor들이 있다. Actor들은 당신의 차, 당신의 차에 달린 센서, 보행자 등과 같은 것들이다. 그리고 Blueprint가 있다. Blueprint는 actor들의 속성이다.

이 정보로 실제 코드를 작성해 보자. 우선, 그냥 차를 생성해서 앞으로 몰고 가자. 그리고 우리는 일반 RGB 카메라에서 자동차 후드에 놓을 정보를 보고자 한다.

코드의 처음 몇 줄, 예시 디렉토리에 있는 다른 스크립트 중 하나에서 복사하여 붙여넣고, 예시 디렉토리에 이 코드를 쓰려고 한다.


>import glob  
>import os  
>import sys  
>  
>try:  
>    sys.path.append(glob.glob('../carla/dist/carla-*%d.%d-%s.egg' % (  
>        sys.version_info.major,  
>        sys.version_info.minor,  
>        'win-amd64' if os.name == 'nt' else 'linux-x86_64'))[0])  
>except IndexError:  
>    pass  
  
>import carla   

위의 코드는 시도/예외 비트를 제외하고 꽤 간단하다. 이 모든 것은 carla egg 파일을 찾는 것이다. 이것이 우리가 carla 패키지 자체에 사용하는 것이다. 실제로 carla를 import하기 위해서는, 우리는 그것을 찾아야 한다. 그리고 그것이 우리가 단지 examples 디렉토리에 우리의 파일을 집어 넣는 이유이기도 하다. 또한 필요한 carla 파일을 Python의 사이트 패키지로 이동하여 이 방법으로도 가져올 수 있다.

다음으로, 우리는 몇 가지를 더 import 할 것이다:

>import random  
>import time  
>import numpy as np  
>import cv2  

필요하다면 아래 패키지도 설치해준다.
> pip install opencv-python  
> pip install numpy  

당장 챙겨야 할 첫 번째 일은 Actor의 리스트를 정리하는 것이다. 클라이언트와 서버를 모두 가지고 있음을 기억하십시오. 우리가 서버에서 클라이언트를 실행하기 시작하면, 우리는 서버에 Actor를 만든다. 우리가 그냥 나가버리면, 청소도 하지 않고, 우리 Actor들은 여전히 서버에 있을 것이다.
```python
actor_list = []
try:

finally:

    print('destroying actors')
    for actor in actor_list:
        actor.destroy()
    print('done.')
```
