# log_server
日誌伺服器

## 資料夾結構
resources 用來存放軟體相關設定檔用

tasks 用來存放分類存放要安裝、設定的Ansible Playbook排程

.env.swp Ansible Playbook主機參數檔

install.yml 用來安裝所需之套件

config.yml 用來設定相關之設定

## Ansible Playbook 操作流程
1. 將.env.swp檔名改成.env
2. 設定.env中，對應的主機名與IP，可一次設定多台
3. 將./resources/.env.swp檔名改成.env，此為SonarQube連到資料庫所需之相關設定，請自行設定好
4. 運行以下指令安裝所需之套件
```
ansible-playbook -i .env install.yml --ask-become-pass
```
5. 輸入登入用密碼
6. 等全部的所需套件都安裝完成，並且沒有跳Fail
7. 運行以下指令設定服務器
```
ansible-playbook -i .env config.yml --ask-become-pass
```
8. 輸入登入用密碼
9. 等全部的所需套件都安裝完成，並且沒有跳Fail

## 服務與連接阜

5044 Port為Logstash服務

5601 Port為Kibana服務

9200, 9300 Port為Elasticsearch服務

本伺服器啟動之後，預設會開始剖析/var/log/dpkg.log服務，預設會將dpkg.log中的內容透過Filebeat扔到Logstash做剖析，最後格式化扔去Elasticsearch，供Kibana分析

你可以透過修改遠端主機底下的/opt/filebeat/filebeat.yml設定抓取檔案來源，與參考/opt/logstash/pipeline/dpkg.conf進行資料剖析

## Logstash 剖析設定相關連結

[grok 教學](https://blog.johnwu.cc/article/elk-logstash-grok-filter.html)

[grok debug](https://grokdebug.herokuapp.com/)