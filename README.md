# Tips

# NoTe
에러해결 및 가상환경 설정/Quarto/Github 에 대해 정리해놓은 repo.

## 목차
- [Section 1: Error](#section-1)
- [Section 2: Tip](#section-2)
  - [2-1. 가상환경](#section-2-1)
  - [2-2. Quarto-Blog](#section-2-2)
  - [2-3. Github](#section-2-3)
- [Section 3: Quarto Dashboard](#section-3-1)

- [Section 4: Ubuntu](#section-4)
  - [4-1. 가상환경](#section-4-1)
  - [4-2. JupyterHub](#section-4-2)
    
<a name = "section-1"></a>
## 💥 Section1: Error
### `-` matplotlib에서 한글깨짐 

`(Window)`

```python
import matplotlib.pyplot as plt
plt.rcParams['font.family'] = 'Malgun Gothic'
plt.rcParams['axes.unicode_minus'] = False  # 마이너스 기호 깨짐문제 해결
```

`(Linux)`

```cmd
# 나눔글꼴 설치
sudo apt-get install fonts-nanum
```

```python
import matplotlib.pyplot as plt
from matplotlib import font_manager

# 나눔글꼴 경로 설정 (Ubuntu 기준)
font_path = '/usr/share/fonts/truetype/nanum/NanumGothic.ttf'
font_prop = font_manager.FontProperties(fname=font_path)

plt.rcParams['font.family'] = font_prop.get_name()

# 이후에 그래프를 그리는 코드 작성
```

<a name = "section-2"></a>
## 🤗 Section2: Tip

<a name = "section-2-1"></a>
### 가상환경

`-` 가상환경 생성 및 활성화

```python
conda create -n py310 python=3.10
conda activate py310
conda install -c conda-forge jupyterlab # 주피터랩설치
conda install -c conda-forge notebook # 노트북설치
conda install pytorch torchvision torchaudio cudatoolkit=11.3 -c pytorch # pytorch 설치


conda env remove –-name [py310] –-all # 가상환경 삭제 
```
`-` R + Python 사용가능한 개발환경

```python
(base) conda create -n rpy 
(base) conda activate rpy
(rpy) conda install -c conda-forge r-essentials
(rpy) conda install -c conda-forge plotly
(rpy) conda install -c conda-forge rpy2
```

- 여기에서 `conda install -c conda-forge r-essentials` 로 인하여 R, Python, Jupyter 가 모두 최신버전으로 설치된다.
- 또한 R에 이미 `tidyverse`, `IRkernel` 등의 패키지가 기본으로 깔려있다.


<a name = "section-2-2"></a>
### Quarto-Blog

`-` 블로그 만드는 방법

> **공식 홈페이지** : <https://quarto.org/docs/websites/website-blog.html>

0. 윈도우에서 설치시 하단의 주소에서 quarto 다운로드
- <https://quarto.org/docs/download/>

1. github에서 새로운 repo 생성

2. settings > pages > branch(main, root)로 설정 후 save // 약간의 시간이 지나면 링크가 생성될 것.

3. 링크가 생성되면 이제 powershell 창에서 다음과 같은 코드를 입력한다.

```python
cd Desktop # 바탕화면에 할 경우.
git clone [github quarto blog address] # 블로그 주소가 아니라 repo 주소를 의미하는 것.
cd [blog name] # 이렇게 해야 블로그에 필요한 파일들이 [blog name]이라는 폴더로 들어간다.
quarto create-project --type website:blog
```

4. 해당 repo에 들어가서 branch가 gh-pages로 되어있는지 확인. (처음 설정할 때는 main과 root밖에 없지만 후에 생긴다.)

5. 미리보기
```python
quarto preview
```

6. Github에 push
```python
git add .
git commit -m .
git push
```

7. 배포
```python
quarto publish gh-pages # github pages로 배포시
quarto publish quarto-pub # quarto-pub으로 배포

```

`-` 블로그의 수정사항이 반영되지 않을 경우 

```python
# 1. main의 모든 내용을 깃헙에 백업
git add .
git commit -m .
git push ## main의 모든 파일(IPYNB)은 깃헙에 백업되어 있음.

# 2. gh-pages로 이동 후 모든 파일 삭제 후 푸쉬
git switch gh-pages  ## gh-pages에는 블로그를 보여주기 위한 html 파일이 있음.
rm -rf * # 윈도우의 경우 수동으로 모든 파일 삭제
git add .
git commit -m .
git push   ## 삭제 내용을 github에 업데이트 -> 모든 html을 삭제했으므로, 블로그의 내용(html)은 모두 삭제.

# 3. 다시 main으로 돌아와서 quarto-publish
git switch main 
quarto publish --no-prompt
```

<a name = "section-2-3"></a>
### Github

```python
# github 유저등록 (다중 유저 등록 시에는 비추)
git config --global user.email "내 깃허브 이메일"
git config --global user.name "내 깃허브 닉네임"
# 아이디 비번 저장
git config credential.helper store
```

<a name = "section-3"></a>

<a name = "section4"></a>
## Section 4:Ubuntu terminal
Ubuntu server에서 가상환경 셋팅 및 JupyterHub 설치.

<a name = "section-4-1"></a>
### Ubuntu server에서 JupyterHub 설치

`1.` 시스템 업데이트
먼저, 패키지 목록을 업데이트하고 시스템을 최신 상태로 유지합니다.

```cmd
sudo apt-get update
sudo apt-get upgrade -y
```

`2.` Python 및 pip 설치
<a name = "section-4-2"></a>
### Ubuntu server에서 JupyterHub 설치.
JupyterHub는 Python으로 작성되었으므로 Python과 pip(패키지 관리 도구)를 설치해야 합니다.

```cmd
sudo apt-get install python3 python3-pip python3-dev -y
```

`3.` Node.js 설치
JupyterHub는 일부 프런트엔드 작업에 Node.js가 필요하다.
```cmd
# 아래코드 실행 전에 내 컴퓨터에 이미 설치되어있는지 확인. (나의 경우 Full Stack을 하면서 미리 깔아놓음.)
# node -v // v18.20.4
# npm -v // 10.7.0
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs
```
`4.` JupyterHUb 및 JupyterLab  설치
JupyterHub와 JupyterLab(또는 Jupyter Notebook)을 설칲한다.
```cmd
sudo -H pip3 install jupyterhub
sudo -H pip3 install notebook jupyterlab
```

--> 에러 발생
이 오류 메시지는 Python 환경이 외부에서 관리되고 있으며, 시스템 전역에 패키지를 설치하려고 할 때 발생하는 문제를 설명하고 있습니다. Ubuntu와 같은 시스템에서는 Python 패키지를 시스템 전역에 설치하는 대신, 가상 환경이나 pipx를 사용하는 것이 권장됩니다.

--> ***Solution1.*** 가상환경에 설치하기
```python
× This environment is externally managed
╰─> To install Python packages system-wide, try apt install
    python3-xyz, where xyz is the package you are trying to
    install.
```



`5.` Configurable HTTP Proxy 설치
JupyterHub는 기본적으로 `configurable-http-proxy`라는 npm 패키지를 사용한다.

```cmd
sudo npm install -g configurable-http-proxy
```

`6.` JupyterHub 설정 파일 생성
JupyterHub 설정 파일을 생성한다. 이 파일에서 JupyterHub의 다양한 설정들을 조정할 수 있다.

```cmd
sudo mkdir /etc/jupyterhub
cd /etc/jupyterhub
sudo jupyterhub --generate-config
```
`7.` 사용자 계정 추가
JupyterHub에서 사용할 사용자를 추가한다. 서버에서 기존 사용자 계정을 사용할 수도 있고, 새로운 사용자를 생성할 수도 있다.

```cmd
sudo adduser username
```
`8.` JupyterHub 서비스로 실행
JupyterHub를 시스템 서비스로 실행하려면 다음 단계를 따른다.

- 1. 서비스 파일 생성
```cmd
sudo nano /etc/systemd/system/jupyterhub.service
```
 
- 2. 서비스 파일 내용 추가
```cmd
[Unit]
Description=JupyterHub
After=syslog.target network.target

[Service]
User=root
Environment="PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
ExecStart=/usr/local/bin/jupyterhub -f /etc/jupyterhub/jupyterhub_config.py

[Install]
WantedBy=multi-user.target
```

- 3. 서비스 시작 및 활성화
```cmd
sudo systemctl daemon-reload
sudo systemctl start jupyterhub
sudo systemctl enable jupyterhub
```

`9.` JupyterHub에 접속
브라우저를 열고 `http://<my-server-ip>:8000`으로 접속하면 JupyterHub 로그인 화면이 나타난다.

이 과정을 통해 Ubuntu 서버에 JupyterHub를 설치하고 설정할 수 있습니다. 필요에 따라 SSL 인증서를 설정하거나, 더 복잡한 인증 메커니즘(LDAP, OAuth 등)을 구성할 수 있습니다.


<a name = "section-4-2"></a>
### Ubuntu server에서 가상환경 셋팅

가상 환경을 생성한 후, 그 안에서 JupyterHub를 설치하는 방법입니다. 이 방법은 시스템 전역에 영향을 주지 않고 패키지를 설치할 수 있는 안전한 방법입니다.
```cmd
python3 -m venv /path/to/your/venv
source /path/to/your/venv/bin/activate
pip install jupyterhub notebook jupyterlab
```
가상환경의 경우 프로젝트 별 가상환경을 생성하여 관리할 수도 있고, 홈 디렉토리 내부에 `.venvs` 폴더를 만들어서 전역적으로 관리할 수도 있다.

- 프로젝트별 관리: 프로젝트 디렉토리 내에 venv 폴더를 생성.
- 전역 관리: 홈 디렉토리 내의 .venvs 폴더 또는 시스템 경로에 생성.

- 1. 가상환경 생성
```cmd
python3.10 -m venv ~/.venvs/py310
```
- 2. 가상환경 활성화
```cmd
source ~/.venvs/py310/bin/activate
# 비활성화
# deactivate
```
***참고사항***

- 가상환경을 생성할 때, 사용하려는 Python 버전이 이미 시스템에 설치되어 있어야 합니다.
- 생성된 가상환경은 ~/.venvs/py310 경로에 저장됩니다. 이 경로는 필요에 따라 변경할 수 있습니다.
- 가상환경이 활성화된 상태에서 Python 패키지를 설치하면 해당 가상환경 내에 설치됩니다.

### 1. Python 3.10 설치
먼저, deadsnakes PPA(Personal Package Archive)를 사용하여 Python 3.10을 설치할 수 있습니다. 이는 Python의 여러 버전을 지원하는 외부 저장소입니다.

```cmd
sudo apt update
sudo apt install software-properties-common -y
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
sudo apt install python3.10 python3.10-venv python3.10-dev -y
```

```cmd
# 2. 가상환경 생성
python3.10 -m venv ~/.venvs/py310
# 3. 가상환경 활성화
source ~/.venvs/py310/bin/activate
# deactivate
```

