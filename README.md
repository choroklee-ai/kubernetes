* kubernetes 1.31
아래는 쿠버네티스 1.31 버전이며 centos9s 운영체제 기반의 설치 파일입니다.
vagrant 이미지를 사용합니다.

wget http://containerlab24.com/k8s_install/k8s-1.31.tar -Outfile k8s-1.31.tar

ms windows 에 적당한 디렉토리를 하나 생성후 그 디렉토리에서 다운로드 받고
powershell 에서 tar -xf k8s-1.31.tar --->  Vagrant up 하면 설치완료가 됩니다
* kubernetes 버전은 항상 최신버전을 설치하도록 되어있습니다.
(대략 설치완료까지 10 ~ 15 분정도 시간소요됩니다)

*. 기존에 vagrant image 로 설치된 kubernetes 를 삭제하고 다시 설치하는 경우에는
아래처럼 vagrant 명령어를 사용하여 vm 을 깨끗이 삭제후 설치해야 새로 설치시  이름충돌이나 
ip 충돌을 방지할수 있습니다.

*. 기존에 vagrant image 로 설치된 kubernetes 를 삭제하고 다시 설치하는 경우에는
아래처럼 vagrant 명령어를 사용하여 vm 을 깨끗이 삭제후 설치해야 새로 설치시  이름충돌이나 
ip 충돌을 방지할수 있습니다.
--------------------------------------------------------------------
* 설치된 Vagrant vm image 삭제

Vagrantfile 이 있는 디렉토리안에서
vagrant halt  ; vagrant 가상머신 종료
vagrant status ; vagrant vm 상태 확인
vagrant destroy ; vagrant vm image 삭제
rm .vagrant  ; vm 생성될때 만들어진 .vagrant 디렉토리 삭제
--------------------------------------------------------------------
--- kubespray 설치를 위한 가상머신 준비 ---

mkdir kubespray ; cd kubespray
wget http://containerlab24.com/kubespray/Vagrantfile -Outfile Vagrantfile
wget http://containerlab24.com/kubespray/add_hosts.sh -Outfile add_hosts.sh
k8s install with kubespray
--- minikube v1.31(kubernetes v1.27) install based ubuntu 22.04 ---
minikube 참고사항

powershell 에서 임의의 디렉토리를 하나 생성후 그 디렉토리로 이동해서
wget http://containerlab24.com/minikube/Vagrantfile -Outfile Vagrantfile
* 아래는 리눅스로 login 해서 파일을 다운로드 받고 실행해야 합니다.
wget http://containerlab24.com/minikube/minikube_install.sh  (minikube start --driver=docker 로 실행됩니다)
chmod u+x minikube_install.sh
./minikube_install.sh  ; 5분정도의 시간이 소요됩니다.

아래는 minikube start --driver=none 으로 실행하기 위한 스크립트 파일입니다.

(참고로 아래의 스크립는  minikube start --driver=docker 로도 실행가능)
wget http://containerlab24.com/minikube/minikube_install_driver_none.sh
 kubernetes pdf
nfs storage volume 테스트를 위한 vm 생성
powershell 에서 mkdir nfs , cd nfs  그리고 아래의 파일을 다운로드
wget http://containerlab24.com/nfs_vm/nfs_vm.tar -Outfile nfs_vm.tar
tar -xf nfs_vm.tar  파일을 풀고 Vagrant 파일에서 ip address 를 kubernetes cluster ip 대역과
같은 대역으로 수정 한다음 Vagrant up

--------------------

- reclaim 정책

Retain : pvc 삭제 => pv 상태 released, 데이터는 보존됨
         pvc 실행을 하면 pv 가 binding 되지 않는다.
         pv 를 다시 사용하려면 수동으로 다시 시작해야 한다.

Recycle : pvc 삭제 => pv는 그대로 있으며 available 상태가 된다.
                      데이터는 삭제됨.
Delete : pvc 삭제 => pv가 삭제된다. 데이터는 그대로 있음

* host_path 인경우 reclaim 정책이 delete 인경우 /tmp 디렉토리에서만
적용된다.

아래와 같은 메시지가 출력되면
Oct 26 09:41:23 control-n1 kernel: watchdog: BUG: soft lockup - CPU#0 stuck for 30s! [containerd-shim:1497]
Oct 26 09:41:23 control-n1 kernel: watchdog: BUG: soft lockup - CPU#1 stuck for 30s! [containerd-shim:1916]

cat /proc/sys/kernel/watchdog_thresh => 이값이 default 로 10으로 되어 있음.
이 값을 아래처럼 올려주면 cpu stuck 현상이 해결되는것 같습니다.
echo "kernel.watchdog_thresh = 30" >> /etc/sysctl.conf
sysctl -p  ; 고친파일내용 적용