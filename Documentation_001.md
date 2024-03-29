# 1부 1장. 클라우드 컴퓨팅과 GCP

클라우드 컴퓨팅이란, 인터넷을 통해 가상의 컴퓨터 환경에서 작업할 수 있는 기술을 말합니다. 실제 물리적 서버의 리소스(정보처리장치, 저장공간 등)를 가상적으로 일부 할당받아 인터넷 접속 만으로 사용할 수 있는 것이죠. 정확히 원하는 수준 만큼의 리소스를 할당받을 수 있고, 심지어 변경 가능하며, 원격 접속이 용이하다는 점에서 상당히 매력적입니다. 특히 24시간 가동하는데 제약이 없고, 공간을 직접 차지하지 않는다는 장점이 크죠. Google Cloud Platform도 일종의 클라우드 컴퓨팅 플랫폼입니다. GCP에서는 가상 머신(VM, Virtual Machine)을 제공하는데, 이 VM이 일종의 가상 컴퓨터의 역할을 하게 됩니다. 다른 유명한 서비스로는 Amazon Web Service(AWS)가 있는데, GCP는 구글 계정 마다 최초 3개월에 한해 300달러 상당의 크레딧을 제공한다는 점에서 단기간, 또는 시험적으로 운영하기에 상당히 적합합니다. 비용 청구 또한 투명하고 직관적인 편이고요. 그래서 우리는 GCP를 사용해 웹 호스팅 서버를 구축해봅시다!

# 1부 2장. GCP VM 생성

GCP에서는 '크레딧'의 형태로 기본적으로 어느 정도의 리소스 사용을 무료로 체험할 수 있게 해줍니다. 이는 사용자의 유형에 따라 조금 다른데요, 개인 납세자는 300달러의 크레딧을, 사업자 납세자는 400달러의 크레딧을 지원받을 수 있습니다. 100달러 차이면 커 보이긴 하지만, 3개월의 제한 안에 300달러를 쓰는 것도 사실은 벅찬 일이랍니다. 따라서 GCP 무료 체험 때문에 사업자등록까지 하실 필요는 없어요.

GCP를 이용하기 위해서는 거주지, 전화번호, 그리고 해외결제 가능 카드까지 등록해야 합니다. 부담이 될 수 있는데요, 무료 체험 기간이 끝나면 자동으로 VM 작동이 중지되며, 결제 또한 동의 후에 이루어지기 때문에 자동결제 되는 일은 없습니다. 가볍게 체험하실 수 있어요.

회원가입을 마치고 나면 Compute Engine API를 활성화해줘야 합니다.(Menu > Compute Engine) 이 Compute Engine이 VM을 사용가능하게 해주는 주된 서비스입니다.

Compute Engine API를 활성화해주고 나면 VM 인스턴스를 생성해야 합니다.(Menu > Compute Engine > VM Instance) '인스턴스 만들기' 와 'VM 가져오기' 두 가지 버튼이 있는데요, 저희는 GCP에서 자체 제공하는 VM을 생성할 것이기 때문에 인스턴스를 새로 만들어줍니다.

여러 입력창이 나오는데, 다음과 같이 설정해줍니다.

- 리전 : asia-northeast3(서울) > 리전은 물리적 서버의 위치를 뜻합니다. 주로 서비스될 지역에 있으면 지연율이 낮아지니 서비스를 하기에 편하겠죠? 저희는 서울로 해줍시다.
 - 영역 : asia-northeast3-a > 서울 리전에는 a, b, c 3개의 영역이 있는데요, 각 영역에서만 사용할 수 있는 VM 종류가 있습니다. 저희는 a를 사용하면 되겠어요.
- 머신 구성: E2 > VM의 하드웨어 종류를 선택할 수 있습니다. 기본적으로는 E2를 사용하면 괜찮아요. 굉장한 성능은 아니지만, 소규모 서버를 만들기에는 충분합니다.
 - 머신 유형 : e2-micro > 코어와 램이 굉장히 작아보이지만, 웹 호스팅에는 저희가 사용하는 컴퓨터에 비해 굉장히 적은 리소스로도 운영이 가능합니다. 비용의 측면도 생각해야 하기에 웹 서버의 사용 가능성을 예측하고 그에 맞게 준비를 해야합니다. 그리고 주의할 점으로, 커스텀 설정을 사용하면 기본 설정들보다 가격이 더 높게 책정됩니다. 이 점 유의해주세요.
- 디스플레이 기기 사용 설정 : (체크 선택) > 디스플레이 출력을 내보낼지 설정하는 곳입니다. 기본적으로 VM은 리눅스 기반으로 된, 콘솔 형태의 운영체제를 가지고 있어요. 콘솔이란 한줄씩 명령어를 입력하는, UI가 없는 입력창으로, 윈도우의 CMD창을 생각하시면 쉬울 거에요. 그러나 이런 리눅스에서도 디스플레이를 사용할 수 있는 'X11'이라는 기술이 있는데, 이걸 사용할 수도 있으니 설정해주는 겁니다.

다음으로는 부팅 디스크를 변경해줍니다.

- 운영체제 : Ubuntu > 리눅스에는 Debian, CentOS, RedHat 등 많은 세부 OS가 있지만, 기본적으로 Ubuntu가 구글에서 정보를 얻기에 상당히 편하고 운영체제도 사용하기 편리하더라고요. 저희는 Ubuntu로 해줍시다.
 - 버전 : Ubuntu 20.04 LTS (x86/64) 기본적으로 웹 호스팅 서버는 성능 보다는 안정성이 훨씬 중요하기 때문에, 안정화 버전인 LTS를 사용하는 것이 매우 권장돼요. 또한, 최신 버전은 안정화되었더라고 오류가 있을 가능성이 있어서 좀 더 구형이지만 그만큼 신뢰성있는 20버전을 사용해줍시다. 저희는 인텔 기반 프로세서를 할당 받을 것이기 때문에 x86/64 OS를 받아줍니다. 
- 부팅 디스크 유형 : 균형 있는 영구 디스크 > 이건 일반적인 HDD/SSD의 구분과 다르게 GCP에서 책정한 디스크 구분이에요. 가격적인 부분도 생각하면 균형있는 디스크 정도면 충분해요!
 - 크기: 20GB > 기본적으로는 10GB로 책정되어있는데, 크기가 큰 모듈을 사용하거나 사이트의 크기가 커지면 조금 모자랄 수도 있어서, 20GB로 해줍시다.

디스크 설정을 완료해주시고, 나머지 설정도 해봅시다!

- ID 및 API 액세스 액세스 범위 : 모든 Cloud API에 대한 전체 액세스 허용 (체크 선택) > 이건 IAM이라고 해서 프로젝트 단위로 계정 군한 나눠서 관리할 때 체크 해제하는 옵션이에요. 저희는 개인이 모두 관리하니 체크 선택!
- 방화벽: HTTP 트래픽 허용 (체크 선택), HTTPS 트래픽 허용 (체크 선택) > 외부에서 HTTP 및 HTTPS 통신으로 VM에 접근할 때, 방화벽이 차단할지 여부를 확인하는 거에요. HTTP 통신이란? 인터넷에서 WWW(World Wide Web)을 통해 이루어지는 통신에 해당하는 통신 규약(프로토콜)이에요. HTTPS 통신은 HTTP 통신에 SSL 보안통신을 더해 보안을 강화한 거에요. 자세히는 복잡하니 생략! HTTP 통신은 포트번호 80, HTTPS 통신은 포트번호 443을 사용하는데, 해당 포트를 개방하면 외부에서 접속이 가능해져요. 체크를 선택해야 웹 서버 통신이 허용되니 둘 다 체크 선택!

이렇게 하면 VM을 생성하는데는 성공했어요! 조금 기다리면 생성될거에요 :>

# 1부 3장. GCP 개발 환경 최적화

VM을 만들긴 했는데, 문제가 있어요. 사실 GCP 홈페이지는 들어가기에 번거롭기도 하고, 파일 업로드 및 다운로드에서 자주 오류를 일으켜요. 그래서 우리는 통신 프로그램을 통해 GCP에 원격 접속을 해 볼거에요!

우선 GCP에서 설정해줘야 하는게 있어요. 바로 VM의 외부 IP 고정인데요, 구글에서 갓 생성한 VM은 외부 IP가 고정되어있지 않아요. 외부 IP를 알아야 해당 VM에 접속할 수 있는데, 이게 변해버리면 골치아프겠죠? 그래서 우리는 이걸 변하지 않게 해줄거에요.

GCP의 VM 네트워크 설정으로 들어가보아요. (Menu > VPC Network > IP Address) 여기서 VM에 할당된 내부IP와 외부IP를 보실 수 있는데요, 외부IP가 바로 우리가 VM에 접속하기 위해 필요한 IP주소에요. 해당 IP주소를 "고정 IP 주소로 승급" 버튼만 눌러주면 쉽게 고정이 된답니다!

<img width="75%" src="https://github.com/Xero-L/WebHosting/assets/147680492/bc4d0e44-ba8a-44b3-b2f9-c30d9bdefa07"/>

이제 접속 대상을 확정했으니 VM에 접속할 방법을 찾아야겠죠? Putty라는 프로그램을 사용하면 쉽게 VM에 접속해, 콘솔 창을 열고 명령을 입력할 수 있어요.(https://putty.org/) 

Putty를 다운로드하면 여러가지 패키지 프로그램이 같이 깔리는데요, 이 중 PuttyGen을 살펴보아요. PuttyGen은 RSA 암호 키를 생성해주는 프로그램이에요. RSA는 암호 생성의 한 방식으로, 공개 키와 비공개 키를 가지고 있어요. 공개 키가 등록되어 있는 서버(VM)에 클라이언트(Putty 등 사용자 층 프로그램)가 비공개 키를 제공하면, 서버에서 이를 확인해 접속을 승인해준답니다. 여기서 접속은 SSH라는 프로토콜을 통해서 이루어져요. SSH는 보안 쉘(콘솔)이라는 뜻으로, 비공개 키 파일을 가지고 있지 않으면 절대로 접속할 수 없답니다. 참고로 SSH 프로토콜의 통신 포트 번호는 22에요.

PuttyGen을 켜고, RSA 2048 방식(2048은 비트 수를 뜻해요)을 체크하고, Generate버튼으로 키를 생성해요. 여기서 Public key라고 출력되는 부분을 key.txt라는 텍스트 문서에 저장해줘야 해요. 이 텍스트는 공개 키가 포함되어있어서, 나중에 서버에 등록해야 하거든요. 그리고 "Key comment" 부분에는 사용할 사용자명을 입력해요. 저는 kimdawn으로 할게요. 그리고 "Key passphrase" 부분에서는 비공개 키 파일을 열 때마다 제한을 거는 암호를 부여할 수 있어요. 설정이 끝났으면, Save Public Key 버튼으로 공개키를 key.key 파일로 저장하고, 비공개키를 key.ppk 파일로 저장해줍니다. 이 key.ppk 파일은 VM에 접속할때마다 가지고 있어야 하니 잘 보관해두세요!

<img width="75%" src="https://github.com/Xero-L/WebHosting/assets/147680492/02f8c507-17df-4481-88b7-858c287f771f"/>

이제 GCP 서버에 공개 키를 등록해야 해요. (Menu > Compute Engine > Metadata > SSH Key) 여기에 아까 key.txt에 붙여놨던 텍스트를 복사해서 다시 붙여줍시다. 이제 VM에 원격 접속하기 위한 준비가 끝났어요!

Putty를 켜줍시다! 이제 비공개 키 파일을 등록해줘야 해요.(Connection > SSH > Auth > Credentials) 인증서 파일에서 SSH 키를 생성한 경우에는 인증서 파일도 필요한데, 저희는 그 경우는 아니니 PPK 파일만 등록해주면 되어요. 그리고 Session 항목에 들어가서, "Host Name" 항목에는 아까 확인했던 GCP VM의 외부 IP주소를 입력해주세요. "Connection type"은 그대로 SSH로 해주시고요. Putty는 간편한 기능으로 이렇게 설정된 것을 '세션'으로 저장할 수가 있어요. "Saved Sessions"의 빈칸에 적당한 세션명을 입력하고, Save 버튼으로 저장하면 된답니다! 다음에 Putty를 켤때는, 바로 Session 항목에서 저장된 세션을 Load 버튼으로 불러오면 돼요.

접속이 정상적으로 되었다면 경고창이 하나 뜰 수 있어요. 제가 접속하려는 컴퓨터가 제가 의도한 컴퓨터가 아닐 수 있다는 건데, IP주소에 이상이 있지 않는 이상 그럴 일은 거의 없어요. Accept 눌러서 그대로 진입해주시면 됩니다!

그 다음으로는 "login as: "라는 문구가 뜨는데, 여기서는 아까 RSA 키 만들때 썼던 사용자명을 입력하면 돼요. 저는 kimdawn 입력할게요. (아래 사진은 전에 테스트하던 것이라 다른 사용자로 되어 있어요.)

그러고 나면 "Passphrase for key: "라는 문구가 떠요. 여기서도 역시 아까 RSA 키 만들때, 비공개 키 파일(PPK)에 걸었던 비밀번호를 입력하면 되어요!

<img width="75%" src="https://github.com/Xero-L/WebHosting/assets/147680492/545bd262-9c98-47c6-b460-5e3cd4bcb154)"/>

이 과정이 모두 끝난다면 정상적으로 접속이 가능하답니다!

그 다음으로는 FileZilla를 사용해보아요.(https://filezilla-project.org/download.php) FileZilla는 로컬 컴퓨터(개발에 사용되는 사용자의 컴퓨터)와 리모트 컴퓨터(원격 VM) 사이에 파일의 업로드, 다운로드를 자유롭게 가능하도록 해주는 통신 프로그램이에요. 설치하다보면 중간에 Oracle browser 설치하라 하는데 Decline해주시는거 잊지 마시고요!

사이트 관리자로 진입해 (파일 > 사이트 관리자) 새 사이트를 만들어줍시다. 우측에 여러 입력 항목이 있는데 살펴볼게요.
- 프로토콜 : SFTP > 저희는 SFTP 프로토콜을 사용할 거에요. 일반적인 파일 전송 방식인 FTP 프로토콜에 보안 프로토콜인 SSH를 합친 거랍니다. 
- 호스트 : 34.xxx.xxx.xxx (VM의 외부IP주소) > 우리가 접속해야 할 대상을 지정하는 곳이에요. 보통 로컬 컴퓨터를 클라이언트, 리모트 컴퓨터를 호스트라고 부른답니다.
- 포트 : 22 > SSH를 사용하는 만큼 22번 포트를 사용합니다. 사실 SFTP를 특정해줬기 때문에 포트는 입력하지 않아도 상관없어요.
- 로그온 유형 : 키 파일 > 저희는 SSH 키를 통해 접속을 할 거기 때문에, 아까 Putty에서 그랬던 것처럼 키 파일을 제공해줘야 서버에서 접속을 승인해주겠죠? 키 파일 유형으로 해주세요.
- 사용자 : 키 파일을 만들때 입력했던 사용자명을 입력하면 됩니다. 저는 kimdawn이네요.
- 키 파일 : 아까 저장했던 key.ppk 파일을 찾아서 연결해주세요.

<img width="75%" src="https://github.com/Xero-L/WebHosting/assets/147680492/486d89dd-c83a-4f55-ac16-543c7e01ede8"/>

이렇게 설정을 해주시면, 연결할 수 있답니다! 이제 다음에 FileZilla를 사용할 때도 그냥 사이트 관리자에서 저장된 설정을 불러오면 돼요.

연결을 시도하면 마스터 비밀번호로 PPK파일 비밀번호를 저장할지(입력을 대신할지) 옵션을 정할 수 있어요. PPK파일 비밀번호가 매우 복잡한게 아니라면, 마스터 비밀번호도 까먹을 수 있으니 굳이 설정하지 않는 걸 추천드려요.

그 다음으로는 비밀번호를 입력해주면, 접속에 성공하면서 우측 창의 하얗던 '리모트 사이트' 영역에 디렉터리들이 로딩된답니다!

# 1부 4장. LAMP 스택 구축

LAMP란, 웹 서버 호스팅에 필요한 핵심 서버 시스템 프로그램들을 지칭해요. Linux, Apache, Mysql, Php의 앞글자를 따온 거라고 볼 수 있죠. Linux는 저희가 2장에서 설치했던 운영체제 종류이고, Apache는 HTTP(S) 통신을 가능하게 해주는 서버 역할을 해요. Php는 백엔드 프로그래밍 환경을 제공하고, Mysql은 Php로 활용 가능한 데이터베이스 서버 역할을 한답니다.

우선 이 패키지들을 VM에 설치해야 하니, Putty를 켜서 VM에 접속해줍시다.

VM을 생성 후에 최초로 접속하면 다음과 같은 명령을 입력해줘야 해요.
`sudo apt update`
`sudo apt -y upgrade`
위 명령어는 VM에 설치된 패키지와 라이브러리를 동기화하고 최적해준답니다. 

## Apache 설치
daemon(윈도우의 서비스 개념)명이 httpd로도 불리는 apache2, 설치해봅시다.
`sudo apt install apache2`
설치 후에는 `sudo service apache2 status` 명령어로 서비스 상태를 확인해볼 수 있어요.
아래 사진과 같이 "active(running)" 상태가 되는지 확인해주세요.

<img width="75%" src="https://github.com/Xero-L/WebHosting/assets/147680492/6c31078e-a798-49c3-8400-97bcf92d3dc7"/>

서비스가 정상적으로 작동되는지 확인되면, 다음과 같은 명령어를 입력해줍니다.
`sudo systemctl enable apache2` Apache는 기본적으로 VM이 재부팅될 때 같이 재시작되지 않아요. 그래서 마치 시작 프로그램에 등록하듯, Apache를 enable 상태로 설정해줘야 합니다.

Apache가 설치되고 나면, GCP VM의 외부IP를 인터넷 브라우저에 입력하면 Apache의 기본 창을 볼 수 있어요! 그렇다면 성공입니다!

## MySQL 설치
MySQL역시 apt 명령어로 설치해주고, systemctl 명령어로 활성화해주세요.
`sudo apt install mysql-server`
`sudo systemctl enable mysql`

## PHP 설치
PHP 라이브러리 자체, Apache와의 연동 라이브러리, MySQL과의 연동 라이브러리를 설치해줍니다. PHP는 최신 버전이 8.3까지 있지만, 저희가 쓰는 OS인 Ubuntu 20.04 LTS에서는 PHP 버전을 7.4까지만 지원해요ㅠㅠ 그래서 지원 버전에 맞게 깔겠습니다!
`sudo add-apt-repository ppa:ondrej/php`
`sudo apt update`
`sudo apt -y upgrade`
`sudo apt install php7.4 libapahce2-mod-php7.4 php7.4-mysql php7.4-mbstring`
여기서 add-apt-repository 명령어로 패키지 설치 목록을 등록한 후에 apt update 명령어로 리로드 해주는걸 잊지 마셔요!

설치되고 나면, 다음 명령어로 웹 서버의 메인 디렉터리에 phpinfo.php 파일을 생성합니다.
`sudo vi /var/www/html/phpinfo.php`

편집기를 연 상태에서, i를 눌러 입력 모드로 전환해주세요. 이 상태에서만 파일 수정이 가능해요.
그리고는 아래 PHP 코드를 입력해주세요.
```php
<?php
  phpinfo();
?>
```

Esc키를 눌러 명령 모드로 나오고, `:wq`를 입력해 저장해주세요.
저장이 완료되면, 브라우저를 켜고, 아래 URL을 입력해보아요.(xxx.xxx.xxx.xxx 는 VM의 외부IP를 의미해요!)
`http://xxx.xxx.xxx.xxx/phpinfo.php`
PHP 7.4 버전에 해당하는 상태 창이 뜬다면 정상적으로 설치된 거에요.

## phpmyadmin 설치
오잉! 사실 LAMP는 다 설치했지만, 하나가 더 남았답니다. phpmyadmin은 php 기반으로 작성된 UI(User Interface)를 통해 웹페이지에서 쉽게 MySQL을 관리할 수 있게 해주는 도구에요. 다음 명령어로 설치할 수 있어요.
`sudo apt install phpmyadmin`

설치하다 도중에 보라색 설정 창이 떠도 당황하지 마세요! 다음과 같이 선택해주면 되어요.

- 첫번째 화면 : 자동으로 재설정할 웹 서버 종류 선택 > apache2
- 두번째 화면 : phpmyadmin 패키지 설치를 위한 데이터베이스 공간 확보 여부 선택 > Yes
- 세번째 화면 : phpmyadmin을 통해 MySQL에 접속할 비밀번호 설정 > (개인 비밀번호)

이제 phpmyadmin에 접속해보아요! 아래의 URL을 브라우저에 입력해주세요.(xxx.xxx.xxx.xxx는 VM의 외부 IP를 의미)
`xxx.xxx.xxx.xxx/phpmyadmin`
접속이 되었다면, 로그인 화면이 뜰 거에요.

<img width="75%" src="https://github.com/Xero-L/WebHosting/assets/147680492/86d9a817-4fa8-45d0-81b8-d73f5dcc6a01"/>

이때 다음과 같이 입력해줍시다.

- 사용자명 : phpmyadmin
- 암호 : (phpmyadmin 설정할 때 썼던 비밀번호)

그런데 로그인을 하고 데이터베이스 항목을 클릭해보면, 권한이 없다고 뜹니다! MySQL은 기본적으로 'root' 유저에는 모든 권한을 부여하지만, 'phpmyadmin' 유저에는 보안상 권한 제한을 겁니다. 권한을 받아오기 위해, 다시 Putty로 VM에 접속해줍니다. 접속한 뒤에 아래 명령어로 MySQL 콘솔로 진입할 수 있어요.
`sudo mysql`

MySQL 콘솔에서, 다음 명령어를 실행합니다. 명령의 뜻은 이래요: "localhost에서 접속한 phpmyadmin 유저에게는, 모든 데이터베이스 및 하위 테이블의 모든 권한을 주겠다"
```sql
GRANT ALL PRIVILEGES ON *.* TO 'phpmyadmin'@'localhost';
```

권한 부여가 끝나면, 다음 명령어로 MySQL 콘솔에서 리눅스 콘솔로 빠져나와줄 수 있어요.
`quit;`

이렇게 하면 기본적인 웹 서버 세팅은 끝났답니다!

# 1부 5장. 도메인 DNS 설정

이제 웹 서버는 구축이 끝났지만, 아직 하지 않은 것이 있어요. 바로 도메인 설정! 우리가 URL을 입력할때 IP주소를 바로 입력할 수도 있지만, '도메인'을 입력할 수도 있답니다! 도메인이란 영문자 및 숫자로 이루어진 문자들이 IP주소를 대신 가리켜주는 것을 뜻해요. 어떤 도메인을 IP주소에 연결해놓으면, 우리는 도메인만 입력해도 해당 IP로 자동으로 연결되죠!

여기서 다시 한 번 비용 부담이 발생하는데요, 도메인은 무료 도메인도 있긴 하지만 가격이 상당히 저렴한 도메인도 많기 때문에 하나 사는것도 괜찮아요. 사용할 수 있는 기능도 많아지고요. 저는 Cloudflare 사에서 도메인을 하나 살게요.

.net, .com, .org 등 많은 도메인이 있는데요, 저는 가장 많이 사용하면서도 가격이 저렴한 .com을 주로 사서 쓰고 있어요. 가격은 1년에 10달러 정도랍니다! 구입하고 나면 연간 자동결제를 할지 체크할 수 있는데, 계획에 따라 해주시면 될 것 같네요!

DNS 설정을 해봅시다(Menu > DNS > Records)! DNS란, 간단히 말하면 하나의 URL이 다른 URL을 가리키게 해주는(Redirect) 기술이에요. 이걸 설정해야 비로소 도메인을 산 의미가 생기죠!

Cloudflare는 기본적으로 DNS 설정 몇 개를 권장하는데요, 다음과 같이 DNS 레코드를 생성해주면 돼요.

<img width="75%" src="https://github.com/Xero-L/WebHosting/assets/147680492/afb9ea01-6201-4082-93b4-5aae1deaffcf"/>

## SPF, DKIM, DMARC 레코드 생성
이름은 어려워 보일 수 있는데, 이 설정들은 DNS 레코드의 정보를 이메일로 전송해주는 역할을 해줘요. 설정 창을 따라가서 이메일만 입력하면, 자동으로 레코드가 추가된답니다.

## CNAME 레코드 생성
CNAME 레코드는 하나의 도메인을 다른 도메인으로 리디렉션 해주는 기능을 해요. 우리가 도메인 입력할때, 앞에 'www'라는 글자를 원래는 붙여야 하지만 안붙여도 되는 때가 많잖아요? 그게 모두 CNAME 레코드가 설정되어 있는 것이에요. 메인 도메인 앞에 점(.)으로 구분되어 나오는 부분을 '서브 도메인'이라고 하는데, 이는 여러개의 서브 도메인이 묶여져 하나의 메인 도메인을 이루는 것처럼 보이게 하는 효과를 가지고 있어요. 기본 사이트는 www 서브도메인을 쓰는게 보편적이라서, 저희도 그렇게 할게요. 아래처럼 입력해주세요.
- Type : CNAME
- Name : www
- Target : @ > 이 기호는 루트 도메인, 즉 메인 도메인을 뜻하는 표시로 쓰여요!
- Proxy status : Proxied > 프록시란 클라이언트와 호스트 사이에 정보를 통과시키거나 관리할 수 있는 통신 매개 서버에요. Cloudflare에서 제공하는 프록시를 제공 받을 수 있어요.

## A 레코드 생성
A 레코드는 도메인을 IP 주소로 연결시켜주는 실질적인 DNS 설정이에요. A 레코드는 IPv4 주소를, AAAA 레코드는 IPv6 주소를 연결할 수 있어요. IP주소에는 v4와 v6이 있는데, 우리가 대부분 사용하는 주소가 4단위의 3자리 숫자로 구성된 v4 주소랍니다. 설정 한 번 해볼까요?
- Type : A
- Name : @
- IPv4 address : (VM의 외부IP 주소)
- Proxy status : DNS only > GCP와 Cloudflare에서 제공하는 프록시가 통신 오류 또는 거부를 일으키는 경우가 있더라고요. 그래서 프록시는 꺼줄게요!
- TTL : Auto

이렇게 DNS 레코드까지 설정해주면, 우리는 도메인을 통해 VM 웹서버에 접속까지 할 수 있게 되었답니다!

리눅스 VM의 /var/www/html 위치를 기반 디렉터리로 해서 이제 파일과 폴더를 채워넣으면 돼요. 제가 가진 도메인이 www.kimdawn.com이라고 한다면, /var/www/html/account/login.php 위치에 있는 파일은 www.kimdawn.com/account/login.php 라는 URL로 기본적으로 접근이 가능하답니다!

# 1부 6장. 부록

## 1절. Linux 기본 명령어

### 접두사
- `sudo` : 관리자 권한으로 명령어 실행

### 명령어
- 패키지 관련
 - `apt update` : 패키지 목록(레포지터리) 업데이트
 - `apt upgrade` : 패키지 목록 업그레이드(업데이트가 가능한 경우 자동 설치)
 - `apt install` : 패키지 설치
 - `apt reinstall` : 패키지 재설치(오류 등으로 인해)
 - `apt purge` : 패키지 제거

- 디렉터리 관련
 - `cd ~` : 현재 폴더에서 ~ 경로로 진입
 - `cd /~` : 루트 폴더 기준으로 ~ 경로로 진입
 - `cd ..` : 현재 폴더의 한 단계 상위폴더로 이동
 - `cd` : 루트 폴더로 이동
 - `ls` : 현재 폴더의 하위 폴더 및 파일 목록 출력
 - `mv` : 파일(폴더)명 변경 또는 위치 이동
 - `rm` : 파일(폴더) 삭제

- 시스템 서비스 관련
 - `service start/stop/restart/status ~` : 시스템 서비스 시작/중지/재시작/상태 표시
 - `systemctl ~ start/stop/restart/status` : 시스템 서비스 시작/중지/재시작/상태 표시
 -` systemctl ~ enable `: 시스템 서비스 활성화(재부팅 시 자동시작)

- vi 편집기 관련
 - `vi/vim ~` : 파일 편집기 실행
 - (Esc키) : 명령 모드 전환
 - (i키) : 입력 모드 전환
 - `:q` : 명령 모드에서, 저장하지 않고 종료(편집을 했을 경우 불가능)
 - `:wq` : 명령 모드에서, 저장하고 종료

- 웹 다운로드 관련
 - `wget ~` : 현재 폴더에 해당 URL이 가리키는 파일 다운로드

- 압축 관련
 - `tar -zxvf ~` : tar.gz 파일 압축해제

## 2절. LAMP 추가 설정

### Linux
#### 루트 로그인 비허용
`/etc/ssh/sshd_config`에서 SSH 접속에 대한 설정을 관리할 수 있어요.
- `#PermitRootLogin prohibit-password` : SSH를 통해 루트 계정(모든 권한을 가진 관리자 계정)으로 로그인 하는 것을 어떻게할지 설정하는 부분이에요. 기본적으로는 비밀번호를 입력하는 것만 금지되어있는데, 이걸 보안을 위해`PermitRootLogin no`로 바꿔줍시다. 

### Apache
#### 웹 서버 정보 변경
`/etc/apache2/sites-enabled/000-default.conf`에서 웹 서버에 대해 인터넷에 제공되는 정보를 바꿀 수 있어요. 
- `ServerAdmin webmaster@localhost` : 웹서버 관리자의 이메일 주소를 남길 수 있어요. 서버에 오류가 생겼을 때 사용자로부터 연락이 올 수도 있으니 관리용 계정으로 설정해보아요.

#### 기타 주요 설정 파일
주요 설정파일 위치를 기록해놨으니, 추가 설정이 필요하거나 오류가 발생하면 한번 찾아보셔요!
- Apache2
 - `/etc/apache2/sites-enabled/000-default.conf`
 - `/etc/apache2/apache2.conf`
- MySQL
 - `/etc/mysql/mysql.conf.d/mysqld.cnf`
- PHP
 - `/etc/php/7.4/apache2/php.ini`
- phpmyadmin
 - `/etc/phpmyadmin/apache.conf`
 - `/etc/phpmyadmin/config.inc.php`

## 3절. 오류 해결
컴퓨터를 뚝딱거리는데 있어서 오류는 언제나 찾아오죠ㅠ 제가 이번에 포럼 준비하면서 맞딱드렸던 오류들을 한번 정리해볼게요!

### Putty를 통한 SSH 연결 안됨
Putty에서 접속을 눌렀는데 Connection timed out(연결하는데 너무 오래 걸림)이 뜨는... 분명 하라는대로 했는데 네트워크 상의 여러 문제로 인해 안될 수도 있어요. 한 번 점검해볼까요?

#### 클라이언트 기기 점검
윈도우 기본 방화벽이 SSH 연결을 막고 있을 가능성도 있어요. 방화벽 규칙에 들어가서(제어판 > Windows Defender 방화벽 > 고급 설정) 인바운드 규칙, 아웃바운드 규칙에 각각 TCP 22번 포트를 모든 IP에 대해 개방하도록 설정해주면 됩니다.

#### 네트워크 점검
사용하는 운영체제에 문제가 없다면, 네트워크 문제일 가능성도 높죠. 다른 라우터에 연결된 LAN이나 WiFi에 접속했을 때 문제가 해결되는지 살펴보세요. 네트워크 문제가 맞다면, 포트포워딩이 필요할 수도 있어요. 포트포워딩에 대해서 간단하게 설명할게요.
- 포트포워딩이란 공유기, 허브, 모뎀과 같은 라우터에서, 특정 기기(IP)의 특정 포트 번호에 대해 다른 모든 IP에서 연결을 시도하는 접속을 허용해주는 것을 뜻해요. 쉽게 말해 특정 포트를 완전히 개방하는거죠.
- 윈도우 cmd 창에 `ipconfig`를 입력하면 '기본 게이트웨이' 라고 출력되는 IP주소로 접속하면, 연결된 라우터의 관리 페이지가 나타나요. 기업마다 다르지만 검색을 통해 초기 로그인 방법을 찾아주시고, 로그인이 되면 네트워크 상태를 확인해주세요. 'WAN IP'가 `192.168.`로 시작한다면, 다른 라우터가 하나 더 연결된 이중 구조라는 뜻이에요. 그렇지 않다면, 포트포워딩 설정에서 SSH 프로토콜의 포트인 22번을, 현재 접속한 기기가 할당받은 내부IP에 맞게 개방해주세요.
- 만약 이중 라우터 구조라면, 포트포워딩을 두 번 해야한답니다. "클라이언트-내부라우터-외부라우터-인터넷망" 의 구조로 이루어져 있다고 생각해볼까요? 클라이언트의 기본 게이트웨이가 내부라우터의 관리 주소였죠? 마찬가지로 내부라우터의 기본 게이트웨이가 외부라우터의 관리 주소랍니다. 따라서 외부라우터에서 내부라우터로 포트포워딩을 한 번, 그리고 내부라우터에서 클라이언트로 포트포워딩을 한 번 이렇게 총 두 번 해야 합니다.

### PhpMyAdmin 페이지 오류(404, 500)
분명 LAMP스택과 phpmyadmin까지 정상적으로 설치했는데, 외부에서 접속을 하려니 안되는 기현상이 발생할 수 있어요. 제가 찾은 해결법은 다음과 같습니다!

#### Apache2 설정 추가
`/etc/apache2/apache2.conf` 파일의 맨 하단에 `Include /etc/phpmyadmin/apache.conf` 를 입력해줍니다. 이렇게 하면 아파치 웹 서버가 phpmyadmin을 인식할 수 있게 되어 404 오류를 해결할 가능성이 생깁니다!

#### PHP 버전 확인
리눅스 콘솔에서 `php -v` 명령어로 PHP버전을 확인해보세요. PHP 버전은 7.4 이하여야 합니다! 만약 8.0 이상의 상위 버전이 설치되었다면 500 오류를 일으킬 수 있습니다.


# 2부 1장. XAMPP의 개념과 필요성

XAMPP는 Windows 10, 11, 그리고 Windows Server OS를 위한 서버 구축 툴입니다. 24시간 가동 및 최적의 환경 구축을 위해 실제 가동하는 서버는 GCP나 AWS를 사용하고, 테스트 서버로 XAMPP를 사용해볼 수 있어요. VM으로 구축한 LAMP 환경은 웹 호스팅에는 적합하지만, 매번 파일을 업로드해야 적용되기 때문에 바로바로 개발할 때는 불편한 단점이 있어요. 따라서 XAMPP로 간단히 개발자의 컴퓨터에서 웹 호스팅을 테스트해볼 수 있답니다! 그렇게 되면 VSC와 같은 IDE에서 변경한 사항이 바로 웹 상에 반영이 되기 때문에, 개발이 상당히 편해집니다.

# 2부 2장. VHD 설정

XAMPP 역시 구동하는데 리소스가 필요하고, 저장 공간 역시 필요해집니다. 그런데 XAMPP를 일반적인 드라이브에서 운용하면 파일의 경로가 너무 복잡해지고, 디스크 관리가 어려운 문제점이 있습니다. 따라서 실제로는 물리적인 디스크에 저장되지만 가상의 경로를 별도로 사용하는 가상 디스크(VHD)를 사용하는 것이 좋습니다.

제어판 > 관리 도구 > 컴퓨터 관리 > 저장소 > 디스크 관리 > 동작 > VHD 만들기

VHD 만들기를 클릭하면 "파일 찾아보기"라는 설명의 버튼이 나오는데, 사실 이건 잘못된 설명이고, 우리는 이 경로로 VHD 파일을 만들 위치를 정할 수 있답니다. VHD 파일은 가상 디스크에 할당된 공간 만큼을 파일의 위치에서 사용합니다. 적당한 드라이브와 경로를 선택한 후, VHD 파일 이름을 지정 후 저장해줍시다. 그리고 나서 아래의 설정을 해주세요.
- 가상 하드 디스크 크기 : 20GB
- 가상 하드 디스크 포맷 : VHD
- 가상 하드 디스크 유형 : 고정 크기

확인 버튼을 누르면 아무 일도 일어나지 않는데, 우측 하단을 잘 보시면 VHD를 생성 중이라고 뜹니다. 만드는데 시간이 좀 걸리니 기다려보아요. 

VHD 생성이 끝나면, 디스크 목록에 "알 수 없음" "초기화 안 됨"이라고 표시된 디스크가 생성됩니다. 왼쪽 블럭을 우클릭해 디스크 초기화를 진행해주세요. 파티션 형식은 GPT 스타일을 사용합니다.

가상 디스크는 초기화 되었지만, 아직 파티션은 할당되지 못했습니다. 오른쪽 블럭을 우클릭해 새 단순 볼륨을 만들어줍니다. 단순 볼륨 크기는 최대로, 드라이브 문자 및 드라이브 이름은 원하는대로 설정해주시면 돼요. 포맷은 NTFS 파일 시스템을 이용하고 압축 역시 사용해줍시다.

설정이 모두 완료되었다면, 파란 그림의 디스크가 목록에 추가되며 VHD 설정이 끝납니다.

# 2부 3장. XAMPP 설치

이제 저장 장치가 준비되었으니, XAMPP를 설치해봅시다! (https://sourceforge.net/projects/xampp/files/XAMPP%20Windows/7.4.33/) PHP버전에 따라 환경이 달라지기 때문에, VM에 설치된 것과 같은 버전인 PHP 7.4.33 버전을 설치해줍시다.

설치 프로그램을 실행하면 "UAC 환경에서는 `C:\Program Files` 경로에 설치하지 말라"는 경고창이 하나 뜹니다. 저희는 가상 디스크에 설치할 것이므로 신경쓰지 않아도 됩니다.

설치할 구성요소를 선택할 수 있는데, 저는 Mercury, Tomcat, Perl, Webalizer, Fake Sendmail에는 체크를 해제했습니다.

설치 경로는 가상 디스크 내부에 빈 폴더를 하나 만들어서, 그 폴더 안에 설치하도록 해야 해요. 가상 디스크에 직접 설치하려 하면 경고창이 뜹니다... 설치 도중에 뜨는 Apache의 방화벽 설정 요청은 모두 허용해주시고요!

설치가 완료되면, XAMPP Control Panel을 "관리자 권한으로" 실행합니다. 관리자 권한이 없으면 일부 기능을 사용할 수 없어요. Modules 영역의 Service 탭의 X 표시된 각 체크박스를 눌러 윈도우 서비스에 등록해줍시다. 등록이 완료되면 체크박스의 표시가 바뀝니다.

다음으로, Config 버튼을 눌러 설정 창을 열고, Autostart of modules 영역의 Apache, FileZilla, MySQL 항목에 체크후 Save해줍니다. 이 설정은 XAMPP Control Panel 실행 시마다 자동으로 웹 서버가 시작되도록 해줘요.

> 필요한 경우, Config 창에서 파일 에디터를 수정할 수 있어요. 기본 설정은 NotePad로 되어있지만, 저는 Visual Studio Code로 설정했어요. 나중에 VSC로만 파일을 수정할 수 있는 경우가 있으니 가급적 에디터를 변경해주세요!

# 2부 4장. XAMPP 설정 (FileZilla)

설치된 XAMPP는 바로 사용할 수 없어요. 몇 가지 설정을 적용해줘야 제대로 된 웹 서버 호스팅이 가능하답니다! 설정을 시작하기 전에, 각 모듈을 Start 버튼을 눌러 시작해주세요.

FileZilla의 Admin 버튼을 눌러줍시다. 서버 설정은 다음과 같이 해주세요.
- Server Address : `127.0.0.1` > 이 주소는 `localhost`라고도 불리는데요, 컴퓨터 자기 자신을 가리키는 IP주소랍니다. 웹 서버가 지금 여러분의 기기에 설치되어 있으니 로컬호스트로 접속하는 거에요.
- Port : `14147` > 포트는 프로그램이 인터넷 연결(TCP)을 사용할 때 사용하는 컴퓨터의 연결 공간입니다. 0~65535의 포트가 있고, 포트는 한번에 하나만 사용이 가능하기에 프로그램들은 포트 번호를 다르게 해서 주로 사용해요. FileZilla는 기본적으로 14147 포트를 사용합니다.
- Administration password > 관리 탭에 진입할 때 사용할 비밀번호입니다. 각자 잘 설정해주세요!
- Always connect to this server : (체크 선택) > 저희는 항상 localhost 서버에 연결할거기 때문에, 체크를 해주겠습니다!

FileZilla Server 창이 뜨고, "Logged on"이라는 문구가 보이면 접속에 성공한 거에요. 이제 본격적으로 설정을 해봅시다.

유저 설정 먼저 할거에요. 유저 관리 창(Edit > Users)에서 Add 버튼을 눌러 유저를 추가해주세요. 이름을 묻는 창이 나오는데, 저희는 `admin`으로 할거에요. 그리고는 다음과 같이 설정해주세요.
- Password : (체크 선택) > 보안을 위해 암호를 설정해줍시다. 기억나는걸로 각자 잘 해주세요!
- Force SSL for user login : (체크 선택) > 역시 보안을 위해, 로그인할 때 SSL 보안통신을 사용하도록 합니다.

<img width="75%" src="https://github.com/Xero-L/WebHosting/assets/147680492/ee3f9272-207a-4509-8a8d-74fb2d70a642"/>

설정이 되었으면, 좌측 사이드바의 Shared folders 탭으로 들어가주세요. Shared folders 영역에서 Add 버튼을 눌러 "XAMPP를 설치할 때 생성했던 가상 디스크 폴더"를 선택해주세요. 그리고 우측 Files와 Directories 영역의 체크박스에 모두 체크선택 해주세요. 이렇게 하면 `admin` 유저는 XAMPP가 설치된 모든 폴더에 대해 모든 관리 권한을 가지게 됩니다! 이제 OK버튼을 눌러 저장해주세요.

<img width="75%" src="https://github.com/Xero-L/WebHosting/assets/147680492/ea3d67ef-ae60-4672-89da-300781e2642c"/>

다음 설정을 위해서는 인증서와 비공개 키 파일이 필요해요. 리눅스 콘솔에서 OpenSSL이라는 라이브러리를 통해 만들 수도 있고, AWS나 Cloudflare에서 발급발을 수도 있답니다. 저는 Cloudflare에 도메인을 보유하고 있기 때문에, 하나 발급받아 오겠어요.

Cloudflare 홈페이지(https://www.cloudflare.com/ko-kr) 에서 도메인을 선택한 후, 인증서 발급 탭(Menu > SSL/TLS > Client Certificates)으로 진입합니다. Create Certificate 버튼을 누르고, "Generate private key and CSR with Cloudflare"을 선택해주세요. Cloudflare가 자동으로 인증서를 생성해주게 됩니다. "Private key type"은 `RSA(2048)`로 설정해주시고요. "Certificate Validity"는 10년 또는 15년으로 해주세요.

다음 창으로 넘어가서는, Key Format으로 `PEM`을 선택해주세요. 그리고 "Certificate"와 "Private Key" 영역에 있는 인증서 본문을 각각 메모장을 켜 `Client_cert.txt`와 `Client_private.txt`에 복붙해주세요. 그리고 `Client_cert.txt`는 `Client_cert.crt`로, `Client_private.txt`는 `Client_private.pem`로 확장자명을 변경해주세요. 파일을 사용할 수 없게 될 수도 있다는 경고가 뜨는데, 무시해주세요! 저희는 사실 이걸 변경해줘야 파일을 사용할 수 있답니다!

인증서 파일(`Client_cert.crt`)을 실행시켜 보면 "정보가 부족하므로 이 인증서를 확인할 수 없"다고 하는데, 이는 루트CA 인증서(인증서 발급기관의 최상위 인증서)가 없어서 그렇습니다. 다만 FileZilla의 SSL 접속에는 엄격한 인증서 확인 절차는 필요하지 않아서, 그대로 진행할게요.

XAMPP Control Panel에서 실행한 FileZilla Server 프로그램으로 돌아갑시다. SSL/TLS 설정(Edit > Settings > SSL/TLS settings) 탭에서, FTPS를 활성화해주세요. "Private key file"에는 `Client_private.pem`파일을, "Certificate file"에는 `Client_cert.crt`파일을 링크해줍시다. "Key password"는 Cloudflare에서 암호화 키를 만들 때 비밀번호를 사용하지 않았으므로 따로 설정하지 않습니다. "Disallow plain unencrypted FTP"에 체크해주시고, "Force PROT P to encrypt file transfers in SSL/TLS mode"에도 체크해주세요. 파일 전송을 항상 암호화하도록 해 보안에 도움이 됩니다! 이제 OK 버튼을 눌러 설정을 저장해주시면 FileZilla Server 준비는 끝났어요!

이제 FileZilla Client에서 간단히 설정해봅시다. TLS 옵션(편집 > 설정 > 연결 탭)에서 최소 허용 TLS 버전을 1.0으로 설정해줍시다. 안전하지 않다는 설명이 있지만, TLS 버전이 호환되지 않으면 연결에 실패할 수 있습니다.

연결은 사이트 관리자에서 아래와 같이 설정해줍니다.
- 프로토콜 : FTP > 프로토콜은 SFTP(SSH)가 아니라 FTPS(FTP)를 사용합니다. 두 통신의 차이는 차후에 다뤄볼게요.
- 호스트 : 127.0.0.1 > 앞서 설명했듯이, localhost(현재 이 컴퓨터)의 IP주소입니다.
- 포트 : 990 > FileZilla Server에서 사실 설정할 수 있는 포트 번호로 하면 됩니다. 우린 기본으로 설정된 990 포트를 쓰겠어요.
- 암호화 : TLS를 통한 묵시적 FTP 필요
- 로그온 유형 : 일반
- 사용자 : admin
- 비밀번호 > FileZilla Server에서 설정한 admin 계정의 비밀번호

<img width="75%" src="https://github.com/Xero-L/WebHosting/assets/147680492/b007fab4-c276-4fa5-b939-b23e8840b084"/>

연결을 시도하면 "알 수 없는 서버 인증서"라고 경고합니다. 컴퓨터에 루트CA 인증서가 설치되어있지 않아서 그런 것인데, Cloudflare에서 제공하는 개인 인증서에는 루트CA 인증서가 제공되지 않기 때문에 이 문제는 해결할 수 없습니다. 사실 OpenSSL을 사용해 직접 인증서를 발급하면 되긴 하는데, 그 과정이 매우 길고 험난하기에 부록에서 다뤄보겠습니다. 따라서 저희는 "이후에도 이 인증서 항상 신뢰"를 선택하고 일단 연결할게요.

우측 리모트 사이트 창에 디렉토리가 뜬다면 성공입니다!

# 2부 5장. XAMPP 설정 (Apache DNS 설정)

이번에는 간단한 DNS 설정과 함께, 이 도메인에 대한 HTTP(S) 연결을 설정해 보아요.

## HTTP 연결 설정

Cloudflare 홈페이지에서 DNS 설정 탭(Menu > DNS > Records)으로 진입합니다. Add Record를 눌러 다음과 같은 설정을 추가해줍시다.
- Type : A
- Name : local
- IPv4 address : (여러분 컴퓨터의 외부IP 주소)
- Proxy status : DNS only

Save를 눌러, 서브도메인이 `local`인 도메인이 제 컴퓨터의 IP주소를 가리키도록 설정하겠습니다.

그런데 `local.kimdawn.com`으로 접속하면 접속이 안되네요! 사실 이건 내부에서 외부IP로 접속을 시도하는 것이기 때문에 기본적으로 불가능합니다. 그러니 다음과 같이 설정을 해봅시다.

`C:\Windows\System32\drivers\etc`경로의 `hosts` 파일을 수정해줍시다. 이 때, 메모장 대신에 VS Code로 열어주세요! VS Code는 파일 수정 시에 자동으로 관리자 권한을 부여해주기 때문에, VS Code로만 파일을 수정할 수 있어요.

`hosts`파일의 마지막 줄에 `127.0.0.1    local.kimdawn.com`코드를 추가해주세요! (도메인 주소는 자신의 것으로 바꿔주세요) 이는 일종의 DNS 설정으로, 컴퓨터의 외부IP를 내부IP로 바꿔주는 역할을 해요. 

## HTTPS 연결 설정

일부 Hypertext 문서와 파일은 HTTPS 연결(HTTP프로토콜의 보안 연결) 상태에서만 동작하도록 되어있어요. 따라서 테스트 용이라도 웹 호스팅에서 HTTPS 연결 설정을 해주는 것은 아주 중요하답니다. 앞서 GCP에서 웹 호스팅을 할 때는 사실 GCP에서 자동으로 HTTPS 연결을 지원해줬기 때문에 그럴 필요가 없었어요. 이번에는 저희가 직접 HTTPS 연결을 설정해봅시다!

Cloudflare에서는 웹 서버(Origin Server) 구축에 필요한 인증서 발급을 지원합니다. Origin Server 탭(Menu > SSL/TLS > Origin Server) 에서 Create Certificate를 눌러주세요. "Generate private key and CSR with Cloudflare"를 선택해 Cloudflare에서 암호 키를 생성하도록 하고, "Private key type"은 `RSA(2048)`로 맞춰주세요. 아래이 "Hostnames" 영역에 기본으로 메인 도메인이 있을 수 있는데, 이를 삭제해주시고 `local`서브도메인만 추가해주세요! "Certificate Validity"는 기본 설정은 15년으로 맞춰주세요.

다음 창에서, "Key Format"은 `PEM`으로 해주세요. 아래의 "Origin Certificate"와 "Private Key"는 각각 `Server_cert.crt`와 `Server_private.key`으로 복붙 후 저장해주세요. 이 때, "Origin Certificate"는 `Server_chain.pem`으로도 하나 저장해주세요! 이 파일은 나중에, 방금 발급받은 인증서와 루트CA 인증서의 관계를 증명하는 인증서 체인으로 사용됩니다.

인증서는 일반적으로 루트CA 인증서가 있어야 유효성 검증을 통과할 수 있어요. 따라서 다음 주소(https://developers.cloudflare.com/ssl/origin-configuration/origin-ca) 에서, 우리가 발급받은 서버용 인증서의 루트CA 인증서를 다운로드 해줍시다. 링크 문서 하단(Additional details > Cloudflare Origin CA root certifiacte)의 "Cloudflare Origin RSA PEM"을 클릭해 다운받아 주세요. 파일명은 `Server_root.crt`로 변경 후 실행해주세요.

루트CA 인증서 파일을 실행하면 "이 CA 루트 인증서를 신뢰할 수 없"다고 뜹니다. 인증서를 컴퓨터에 설치해주면 문제를 해결할 수 있어요.(인증서 설치 > 로컬 컴퓨터에 설치 > 모든 인증서를 다음 저장소에 저장 > 신뢰할 수 있는 루트 인증 기관) 설치가 완료되면, 루트CA 인증서와 서버 인증서를 실행했을 때 둘 다 이제 "인증서가 올바르다"고 인식될 거에요!

이제 파일들을 XAMPP가 인식할 수 있게 가상 디스크의 XAMPP가 설치된 경로로 이동시켜 줍시다. `Server_cert.crt`, `Server_cert.pem`, 그리고 `Server_root.crt`는 `~\apache\conf\ssl.crt`폴더에, `Server_private.key`은 `~\apache\conf\ssl.key`폴더에 저장해줍니다. 그리고 나서는, Apache의 SSL 설정을 위해 `~\apache\conf\extra\httpd-ssl.conf`파일을 Visual Studio Code로 편집해줍시다.

```
<VirtualHost _default_:443>

ServerName www.example.com:443

SSLCertificateFile "conf/ssl.crt/server.crt"
SSLCertificateKeyFile "conf/ssl.key/server.key"
#SSLCertificateChainFile "${SRVROOT}/conf/server-ca.crt"
#SSLCACertificateFile "${SRVROOT}/conf/ssl.crt/ca-bundle.crt"
```
위와 같이 기본적으로 설정되어 있는 부분을 찾아 아래와 같이 변경해줍시다.
```
<VirtualHost *:443>

ServerName local.kimdawn.com

SSLCertificateFile "conf/ssl.key/Server_cert.crt"
SSLCertificateKeyFile "conf/ssl.key/Server_private.key"
SSLCertificateChanFile "${SRVROOT}/conf/ssl.crt/Server_chain.pem"
SSLCACertificateFile "${SRVROOT}/conf/ssl.crt/Server_root.crt"
```

이제 XAMPP용 도메인(local.kimdawn.com)으로 접속하면 HTTPS 연결이 사용됨을 확인할 수 있어요!

설정은 모두 끝났고, XAMPP에서는 `~/htdocs`폴더를 웹 상의 루트 디렉토리로 사용하면 된답니다!

# 3부 1장. IP통신 네트워크 구조와 프로토콜

## IP 통신

IP 통신은 TCP/IP 통신이라고도 불리는, 네트워크 통신 기반의 하위 계층 통신 방식입니다. IP 통신을 하는 모든 기기에는 IP 주소가 부여되며, 이 주소를 통해 원하는 목적 기기 간에 연결이 이루어집니다. IP 통신 기기에는 0~65535번의 포트가 있는데, 한 기기는 여러 개의 포트를 통해 한번에 여러 기기와 통신할 수 있습니다. 단, 하나의 포트는 한번에 하나의 기기와만 통신이 가능합니다. IP 통신 기반의 상위 계층 프로토콜(통신의 세부 표준규격)들은 서로 충돌하는 일이 없도록 하기 위해 서로 다른 포트번호를 선언하고 있습니다. 대표적인 예시로 FTP 통신은 21번 포트, SSH 통신은 22번 퐅, HTTP 통신은 80번 포트를 사용합니다.

## SSL 통신

SSL 통신은 보안을 위한 목적으로 추가된 중간 계층 프로토콜로, FTP 프로토콜과 HTTP 프로토콜은 보안을 위해 SSL 통신을 함께 사용하는 것이 일반적입니다. 이렇게 SSL 통신을 같이 사용하게 된 프로토콜은 각각 FTPS 프로토콜과 HTTPS 프로토콜로 불리며, 기존 비암호화 프로토콜과는 다른 포트 번호를 사용하게 됩니다.(FTP > FTPS: 80 > 990, HTTP > HTTPS: 80 > 443)

SSL 통신의 기본적인 원리는 다음과 같습니다. 접속 대상이 되는 기기(호스트)는 RSA 비공개 키를 보유한 상태로, RSA 공개 키를 네트워크 상에 공개합니다. 그러면 접속을 하려는 기기(클라이언트)는 해당 공개 키에 정확히 대응되는 RSA 비공개 키를 호스트에 제공해야 합니다. 그렇지 않을 경우에는 접속이 차단되게 됩니다. 

## GCP 웹 서버의 네트워크 구조

<img width="75%" src="https://github.com/Xero-L/WebHosting/assets/147680492/08caf433-c405-4cf8-a69a-08455a3e83f3"/>

개발용 컴퓨터에서는 기본적으로 인터넷으로 GCP 서버에 접속해 VM 인스턴스에 연결할 수 있습니다. 이 때, 개발용 컴퓨터는 HTTPS 통신만 사용하지만 GCP 서버가 자동으로 VM 인스턴스와 SSH 연결을 이루기 때문에 사용자가 따로 RSA 비공개 키를 제공할 필요는 없습니다. 그러나 1부에서 진행되었던 것과 같은 설정을 마치게 되면, GCP 서버에 접속하지 않고도 바로 SSH 통신을 사용해 VM 인스턴스에 연결할 수 있게 됩니다.

사용자 컴퓨터에서는 HTTPS 통신을 통해 VM 인스턴스에 접근하게 되는데, 이 때 Cloudflare의 DNS 서버를 경유하게 됩니다. 사용자는 도메인 주소만을 입력하지만, DNS 서버가 이를 자동으로 IP 주소로 변환해 통신을 연결해줄 수 있습니다. SSH를 통해 VM 인스턴스에 접근할 때는 권한이 부여된 계정에 따라 리눅스 운영체제 자체에 접근할 수 있지만, HTTPS를 통해 VM 인스턴스에 접근할 때는 VM 인스턴스의 서비스인 Apache가 매개하는 통신에만 참여할 수 있는 제약이 있습니다.

## XAMPP 웹 서버의 네트워크 구조

<img width="75%" src="https://github.com/Xero-L/WebHosting/assets/147680492/6582465d-d75f-4be7-b4a9-c26b721866e5"/>

개발용 컴퓨터의 웹 브라우저에서 도메인을 입력하면, 운영체제는 외부와 통신하기 전에 vhosts 파일을 검사합니다. 이 때 vhosts 파일에서 입력된 도메인이 `127.0.0.1` IP를 가리키고 있으면, 운영체제는 일종의 프록시(HTTP 통신의 중개 서버)와 같이 행동해, 웹 브라우저가 보낸 통신의 내용을 웹 서버에 보내게 됩니다. 사실 개발용 컴퓨터의 브라우저에서 해당 컴퓨터의 웹 서버에 접속하는 경우에는 도메인을 구매할 필요가 없습니다. 애초에 외부와 통신하지 않기 때문이죠.

그러나 사용자 컴퓨터의 웹 브라우저에서 도메인을 입력하면, 해당 통신 내용은 웹 상의 DNS 서버에 전송되어, 결국 개발용 컴퓨터로 보내지게 됩니다. 이 때도 개발용 컴퓨터의 운영체제는 vhosts 파일을 바탕으로 웹 서버에 통신 내용을 보내줍니다.

사실 vhosts 파일의 역할은 한 컴퓨터 내에 여러 개의 웹 서버가 존재하는 경우, 컴퓨터로 들어오는 통신 내용이 적합한 웹 서버로 이동하도록 매칭시켜주는 것입니다. 자세한 내용은 생략하지만, vhosts 파일을 설정해줘야 개발용 컴퓨터의 외부와 내부가 연결될 수 있는 것은 분명합니다.

# 3부 2장. 공인IP가 할당된 모뎀의 네트워크 구조와 포트포워딩

<img width="75%" src="https://github.com/Xero-L/WebHosting/assets/147680492/ca41f1b9-f027-453a-bbf9-a9bb13d345c9"/>

## 라우터와 외부, 내부 IP 주소

라우터는 상위 라우터로부터 외부IP를 할당받으며, 역시 똑같이 하위 라우터에 외부IP를 할당하는 네트워크 기기입니다. 라우터에는 일반적으로 자주 사용하는 모뎀, 공유기 등이 있습니다. 모뎀은 통신 단자함에 주로 있으며 인터넷을 공급하는 핵심 열할을 하고, 공유기는 모뎀으로부터 공급받은 인터넷을 무선랜 신호로 변환하거나 다른 유선랜에 나누어 연결해주는 역할을 합니다. 일반적인 가정의 경우 "웹 서버 - 모뎀 - 공유기 - 단말 기기"의 네트워크 구조를 이루고 있습니다.

라우터는 상위 라우터로부터 외부IP를 할당받습니다. 최상위 라우터(모뎀)의 경우에는 ISP(인터넷 공급업체)에서 제공받은 공인IP를 할당받습니다. 이렇게 할당받은 외부IP는 해당 라우터와 수평적으로 연결된 다른 기기에서 연결할 때 사용됩니다. 라우터는 하위 네트워크 망을 가지고 있으며, 하위 네트워크에서 라우터에 연결할 때는 라우터의 내부IP를 사용합니다. 이를 '기본 게이트웨이'라고 합니다. `192.168.xxx.xxx`의 IP 대역은 사실 내부IP 전용으로 할당되어 있습니다. 이에 따라 라우터의 기본 게이트웨이는 `192.168.xxx.1`로 지정되며, 라우터는 `192.168.xxx.2`~`192.168.xxx.255`의 IP대역을 하위 네트워크의 기기 용으로 할당해줍니다. 이렇게 할당된 내부 IP 주소는 하위 네트워크 상 기기의 외부IP 주소가 됩니다.

## 포트포워딩

포트포워딩은 상위 라우터에서 설정하는 것으로, 특정 포트에 대해 상위 라우터와 하위 라우터의 연결을 허용하는 것을 의미합니다. 예를 들어 위 그림과 같은 구조의 네트워크 시스템이 있을 때, 외부IP가 `192.168.45.33`인 기기의 포트 22번을 웹 서버에 개방해 봅시다. 그럴 경우 우선 상위 라우터인 공유기 A에서 우선 포트포워딩을 해야 합니다. 포트포워딩이 완료되면, 공유기 A의 외부IP인 `192.168.75.33`에 접속하는 공유기 B의 하위 네트워크 상의 기기는 공유기 A의 하위 네트워크 상의 기기인 `192.168.45.33`에 접속할 수 있습니다. 다음으로 공유기 A의 상위 라우터인 모뎀에서 포트포워딩을 해야 합니다. 역시 설정이 완료되면, 모뎀의 외부IP인 `12.34.567.890`에 접속하는 웹 서버 상의 모든 기기는 `192.168.45.33` 기기에 접속할 수 있습니다. 포트포워딩은 라우터의 외부와 내부를 연결해주는 역할을 하는데, 이는 외부IP와 내부IP 두 개의 IP를 가지는 라우터의 특성에 따른 것입니다.

사실 같은 모뎀에 연결된 모든 라우터와 기기는 모뎀 밖에서 봤을 때 모두 같은 IP주소를 지닙니다. 모뎀은 공인IP를 단 하나만 할당받을 수 있고, 이를 '내부 IP'라는 개념으로 나눠서 네트워크를 이루기 때문입니다. 따라서 포트포워딩은 모뎀에 연결된 수많은 기기 중 어느 기기에 연결할지를 선택해주는 역할도 합니다.
