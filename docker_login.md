
* docker hub 에서 다운로드 받을수 있는 리미트에 도달하게 되면 더이상 다운로드가 되지 않으므로
도커허브에 자신의 아이디로 인증을 한후 도커 이미지를 다운로드 받아야 합니다.

kubernetes 에서 도커허브 인증방벙븐 아래와 같이
secret object 를 자신의 아이디 및 암호, 이메일로 생성해서 사용하면 됩니다.

kubectl create secret docker-registry mysecret --docker-username=kildong --docker-password='mypass' 
<!-- --docker-email=kildonge@naver.com -->

* 암호에 특수문자가 포함되어 있는경우 따옴표를 붙여주는것을 권장합니다.
* yaml 파일에는 맨 아래에 생성한 secret 을 추가하여야 합니다.

--------------------

---
apiVersion: v1
kind: Pod
metadata:
  name: apache-pod
  labels:
    myapp: myweb
spec:
  containers:
  - name: apache-container
    image: nginx:1.21
    ports:
    - containerPort: 80
  imagePullSecrets:
  - name: mysecret
...

---
apiVersion: v1
kind: Service
metadata:
  name: myweb-service
spec:
  selector:
    app: myweb  # 👈 This should match the label in the Pods
  ports:
    - protocol: TCP
      port: 80  # Service Port
      targetPort: 8080  # Pod containerPort
