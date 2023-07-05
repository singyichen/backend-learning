---
title: Docker Install
description: Docker學習筆記
published: true
date: 2023-05-17T01:48:08.903Z
tags: docker, devops
editor: markdown
dateCreated: 2022-04-29T01:11:05.872Z
---

# Docker Install
 
# 環境說明
**作業系統**：Ubuntu 20.04 Server LTS
**安裝套件**：除了 OpenSSH 外，其餘套件不需要安裝，以便維持系統的乾淨。

# 安裝環境
## 下載 VirtualBox
> [VirtualBox download info](https://www.virtualbox.org/wiki/Downloads)
{.is-info}

## 下載並安裝 OS : Ubuntu 20.04 Server LTS 
> [Ubuntu download info](https://ubuntu.com/download/server)
{.is-info}
- wait for few minutes until [ Reboot Now ] appear 
- :wq! => save and exit

> [Ubuntu install info](https://sofree.cc/virtualbox-os/)
{.is-info}

- 新建虛擬磁碟：設定為 100 g

![vm 新建虛擬磁碟.png](http://192.168.25.60:8000/files/file_storage/67261425.png)

## 設定 SSH
因為某些指令過長，輸入時極容易輸入錯誤，為了後續透過 Terminal 來進行操作，故安裝完作業系統後的第一件是就是先來設定 SSH Server。

1. ssh 設定檔調整
```bash
sudo nano /etc/ssh/sshd_config
```

2. 進入到文字編輯器中，調整以下設定
```bash
Port 22
PasswordAuthentication yes
PermitRootLogin yes
```

3. 重啟SSH Service
```bash
sudo /etc/init.d/ssh restart
```

## 作業系統時區設定
為了要讓作業系統顯示正確的時區，所以我們必須重新設定作業系統使用的時區

1. 作業系統時區設定
```bash
sudo dpkg-reconfigure tzdata
```

2. 選擇區域( Asia )

![docker_set_1.png](http://192.168.25.60:8000/files/file_storage/2305e81f.png)

3. 選擇地點( Taipei )

![docker_set_2.png](http://192.168.25.60:8000/files/file_storage/4f515e3f.png)

4. 設定完成後會在 Console 畫面顯示，目前的時區與時間
``` bash
Current default time zone: 'Asia/Taipei'
Local time is now:      Thu Oct  7 10:02:08 CST 2021.
Universal Time is now:  Thu Oct  7 02:02:08 UTC 2021.
```

## 下載並安裝 Putty
> [Putty download info](https://www.putty.org/)
{.is-info}

## 使用 SSH 協定連線至 VirtualBox 中的 Ubuntu 作業系統
> [VirtualBox 設定 Port 轉發功能教學-以 SSH 和 HTTP 為例](https://www.kjnotes.com/devtools/77)
{.is-info}

### 查看 Ubuntu IP
```bash
ifconfig
```
```
10.0.2.15
```
![ubuntu get VM IP.png](http://192.168.25.60:8000/files/file_storage/0c3c970f.png)

### 查看 SSH 服務是否啟動
```bash
sudo service ssh status
```

![ubuntu 查看 SSH 服務是否啟動.png](http://192.168.25.60:8000/files/file_storage/cfcf818b.png)

### 將 Ubuntu 關機
```bash
sudo poweroff
```

### 設定 VirtualBox 上的 Port 轉發功能（Port forwarding）
#### 1. 在 VirtualBox 虛擬機器上，選擇要設定 Port 轉發的作業系統，然後點選『設定值』

![ubuntu 選擇要設定 port 轉發的 OS 並點選設定.png](http://192.168.25.60:8000/files/file_storage/be340523.png)

#### 2. 網路 ➔ 介面卡1 ➔ 附加到 NAT ➔ 進階 ➔  連接埠傳送

![ubuntu 網路介面卡設定.png](http://192.168.25.60:8000/files/file_storage/4d649dd1.png)

#### 3. 右邊綠色加號 + ➔ 新增連接埠傳送規則

![ubuntu 新增連接埠傳送規則.png](http://192.168.25.60:8000/files/file_storage/7c2b1756.png)

**名稱**：可以填日後能看得明白名稱
```
ssh
```
**協定**：以SSH服務來說就選擇預設的『TCP』
```
TCP
```
**主機**：想要從本機的主系統連上去，所以就填了『127.0.0.1』的 IP 位址
```
127.0.0.1
```
**主機連結埠**：填寫要使用到的 Port 號，以 SSH 服務來說，會給他設『22』的 Port 號
```
22
```
**客體 IP**：就填寫剛剛在上面已經查好的 Ubuntu 作業系統 IP 位址，如：剛剛查到的IP位址是『10.0.2.15』；
```
10.0.2.15
```
**客體連接埠**：以 SSH 服務來說，預設的 Port 號就是『22』
```
22
```

### 使用 SSH 工具 Putty 連上 VirtualBox 虛擬機器中的作業系統
#### 1. VirtualBox Ubuntu 啟動但不須登入

![Ubuntu 啟動但不須登入.png](http://192.168.25.60:8000/files/file_storage/6d01a747.png)

#### 2. 使用 Putty 設定連線 session

![putty session configuration.png](http://192.168.25.60:8000/files/file_storage/58ba1045.png)

**Host Name**：輸入剛剛在前面 Port 轉發規則中設定『主機 IP』的 IP 位址，如設定『127.0.0.1』本地主機的 IP 位址
```
127.0.0.1
```
**Port**：設定『主機連結埠』中的 Port 號，如設定 SSH 預設所使用的 Port 22，所以可以不用改
```
22
```
**Connection type**：選擇預設的『SSH』選項
```
SSH
```
**Saved Sessions**：為這個工作階段命一個日後好辨識的名稱
```
ubuntu-localhost
```
設定好之後，可以儲存當前的工作階段（Session），點選『Save』儲存
在 Putty 上完成此次新的連線設定後，就可以點選『Open』來進行連線至虛擬機器中的作業系統

#### 3. 首次連線至新的主機會出現如下圖所示的『PuTTY Security Alert』視窗是正常的，點選『yes』繼續連線

![putty Security Alert.png](http://192.168.25.60:8000/files/file_storage/d7b26c25.png)

#### 4. 成功使用 SSH 服務來連線與登入 VirtualBox 中的作業系統畫面，接下來就可以直接使用 SSH 遠端服務來控制作業系統，例如複製貼上

![putty login.png](http://192.168.25.60:8000/files/file_storage/cbc7ec14.png)

#### 5. 使用 load 開啟已經儲存過的 session
ubuntu-localhost ➔ load ➔ open

![putty load session.png](http://192.168.25.60:8000/files/file_storage/0a3038ba.png)

# 安裝 Docker
## 安裝 Docker
1. 作業系統熱機更新
```bash
sudo apt-get update && time sudo apt-get dist-upgrade
```

2. 安裝 Docker 必要套件
```bash
sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release
```

3. 把 Docker Official GPG 金鑰，加入到伺服器中，以便後續下載 Docker image
```bash
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

4. 設定要下載 Docker image 的遠端 Docker Hub 儲存庫的位址
```bash
sudo echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

5. 再次更新系統套件
```bash
sudo apt-get update
```

6. 安裝 Docker CE 相關套件
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

7. 驗證 Docker 相關套件是否安裝成功 <mark>(非必須)</mark>
```bash
sudo docker run hello-world
```

![docker install successfully.png](http://192.168.25.60:8000/files/file_storage/5bda0165.png)

## 設定 Docker
1. 建立 docker 群組 <mark>(現行 Docker 版本已經會自動增加，非必須)</mark>
```bash
sudo groupadd docker
```

2. 將目前的帳號增加到 docker 群組中，以便後續執行 Docker 時不需要提升使用者權限
```bash
sudo usermod -aG docker $USER
```
```bash
sudo usermod -aG docker mandychen
```

3. 接下來重新啟動 Linux 或是 先登出目前的帳號後，再重新登入，或者是執行下列語法，以便套用權限設定
```bash
newgrp docker
```

4. 驗證是否可以在 沒有提升權限的情況下，執行 Docker 
```bash
docker run hello-world
```

## 服務設定
1. 在 Debian 和 Ubuntu 上，Docker 服務預設的設定會在啟動時啟動。如果是採用其他 Linux 發行版時，希望自動啟動 Docker 和 Container ，請使用以下命令<mark>(非必須)</mark>
```bash
sudo systemctl enable docker.service

sudo systemctl enable containerd.service
```

2. 使用 systemd 設定 docker.service
```bash
sudo systemctl edit docker.service
```

3. 進入文字編輯器中，將以下內容覆寫到該文件中
```plaintext
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://127.0.0.1:2375
```

4. 重新載入 systemctl 設定
```bash
sudo systemctl daemon-reload
```

5. 重啟 Docker 服務
```bash
sudo systemctl restart docker.service
```

6. 安裝 net-tools 以便監看 Docker 網路狀態
```bash
sudo apt-get install net-tools
```

7. 透過 netstat 指令，以便確認 dockerd 是否正在以我們所設定的埠號執行中
```bash
sudo netstat -lntp | grep dockerd
```

## 安裝 Docker Compose
Docker Compose 是為了協助定義和共用多容器應用程式而開發的工具。

1. 執行下列命令以下載 Docker Compose 的目前穩定版本：
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
> 目前 Docker Compose 穩定版為 1.29.2，可至 [Compose repository release page on GitHub](https://github.com/docker/compose/releases) 隨時查閱目前 Docker Compose 穩定版

2. 對二進制文件套用可執行權限
```bash
sudo chmod +x /usr/local/bin/docker-compose
```

3. 將剛剛建立的執行路徑，指向 /usr/bin 或路徑中任何其他目錄的符號鏈接
```bash
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

4. 測試 Docker Compose 是否安裝完成
```bash
docker-compose --version
```

> 若成功則會出現 docker-compose version 1.29.2, build 5becea4c

# 安裝 WinSCP
## 下載 WinSCP
> 使用 WinSCP 在 VirtualBox 與 Windows 之間進行檔案傳輸

> [WinSCP download info](https://winscp.net/eng/download.php)
{.is-info}

## 設定站台
### 新增站台 ➔ 登入

![winscp 新增站台.png](http://192.168.25.60:8000/files/file_storage/3804d533.png)

**主機名稱**：『主機 IP』的 IP 位址
```
127.0.0.1
```
**連結埠**：設定『主機連結埠』中的 Port 號，如設定 SSH 預設所使用的 Port 22
```
22
```
**使用者名稱**：ubuntu 登入使用者名稱
```
mandychen
```
**密碼**：ubuntu 登入使用者密碼

### 將專案傳送到 VirtualBox 中

![winscp 檔案傳輸.png](http://192.168.25.60:8000/files/file_storage/f5b1dc3c.png)


# Why Use Docker
- 虛擬主機
- 容器化模式：把應用程式包成一個映象檔，再放到容器裡面執行，有問題時可以重啟也不會影響到其他容器
1. 程式碼設定、安裝指令、環境建置：包成映象檔，可跨平台部屬
2. 可建立乾淨的測試環境:因為 volume 持久化，狀態可更新成原本一開始的樣子，可開不同容器進行不同的測試
3. 
 
