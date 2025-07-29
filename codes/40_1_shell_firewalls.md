### **🧪 문제 1: 특정 IP 차단 상태 확인 후 차단 설정**

#### **✅ 실행 예시**

$ sudo ./problem1.sh

\[INFO\] 현재 rich rule 목록에 192.168.0.100 차단 룰이 존재하지 않습니다.

\[INFO\] 차단 룰을 추가합니다...

success

또는

$ sudo ./problem1.sh

\[INFO\] 192.168.0.100은 이미 차단되어 있습니다.

\[SKIP\] 추가 작업을 수행하지 않습니다.

---
```bash


#!/bin/bash

V_IP="$1"
V_ACC="$(firewall-cmd --list-rich-rules | grep "$V_IP" | cut -d" " -f5)"

if [ "$V_ACC" = "reject" ]; then
        echo "[INFO]$V_IP already blocked"
elif [ "$V_ACC" = "accept" ]; then
        echo "[INFO]$V_IP already accepted"
else
        echo "[INFO]$V_IP block"
        firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="$V_IP" reject"
        firewall-cmd --reload > /dev/null
fi

[seungjae@192.168.0.43 ~/quests/shell_fierwalls]$ sudo ./problem1.sh 192.168.0.46
[INFO]192.168.0.46 block
success
[seungjae@192.168.0.43 ~/quests/shell_fierwalls]$ sudo ./problem1.sh 192.168.0.46
[INFO]192.168.0.46 already blocked

[doyoung@192.168.0.46 ~]$ ping 192.168.0.43
PING 192.168.0.43 (192.168.0.43) 56(84) bytes of data.
From 192.168.0.43 icmp_seq=1 Destination Port Unreachable
From 192.168.0.43 icmp_seq=2 Destination Port Unreachable
From 192.168.0.43 icmp_seq=3 Destination Port Unreachable
From 192.168.0.43 icmp_seq=4 Destination Port Unreachable
^C
--- 192.168.0.43 ping statistics ---
4 packets transmitted, 0 received, +4 errors, 100% packet loss, time 3015ms

```


### **🔒 문제 2: 포트 8080이 열려 있다면 닫기**

#### **✅ 실행 예시**

$ sudo ./problem2.sh

\[INFO\] 포트 8080/tcp 이 열려 있습니다. 제거합니다...

success

또는

$ sudo ./problem2.sh

\[INFO\] 포트 8080/tcp 이 열려 있지 않습니다. 아무 작업도 수행하지 않습니다.

```bash

#!/bin/bash

V_PORT="$1"
V_ACC="$(firewall-cmd --list-ports | grep "$V_PORT")"

if [ "$V_ACC" = "" ]; then
        echo "[INFO]$V_PORT already blocked"
else
        echo "[INFO]$V_PORT is opened. delete."
        firewall-cmd --permanent --remove-port="$V_PORT"
        firewall-cmd --reload > /dev/null
fi


[seungjae@192.168.0.43 ~/quests/shell_fierwalls]$ sudo ./problem2.sh 8080/tcp
[INFO]8080/tcp is opened. delete.
success
[seungjae@192.168.0.43 ~/quests/shell_fierwalls]$ sudo firewall-cmd --list-ports

[mk@192.168.0.100 ~]$ telnet 192.168.0.43 8080
Trying 192.168.0.43...
telnet: connect to address 192.168.0.43: Connection refused

```

---

