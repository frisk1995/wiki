## ZabbixAPIを用いたホストの追加

[前提条件](https://www.notion.so/305f211faf8f470ebcd8a243d4a56dd8)

### 環境変数定義ファイル

```bash
login_user=Admin
login_password=zabbix
zabbix_host=localhost
```

# 手順

## 変数

```yaml
login_user: ログインユーザID
login_password: ログインパスワード
zabbix_host: ホスト名
auth: 認証トークン

hostgroup_name: ホストグループ名
hostgroup_id: ホストグループID
host_name: 登録ホスト名
type: 監視タイプ(agent,snmpなど)
host_ip: IP
port: ポート

hostid: ホストID
interfaceid: インターフェースID
applicationid: アプリケーションID
```

### 認証コード取得

```bash
auth=`curl -s -d '{
    "jsonrpc": "2.0",
    "method": "user.login",
    "params": {
        "user": "'${login_user}'",
        "password": "'${login_password}'"
    },
    "id": 1,
    "auth": null
}' -H "Content-Type: application/json-rpc" http://${zabbix_host}/zabbix/api_jsonrpc.php | jq -r .result`
```

### ホストグループID取得

```bash
curl -s -d '
{
    "jsonrpc": "2.0",
    "method": "hostgroup.get",
    "params": {
        "output": "extend",
        "filter": {
            "name": [
                "'${hostgroup_name}'"
                  ]
        }
    },
    "auth": "'${auth}'",
    "id": 1
}
' -H "Content-Type: application/json-rpc" http://${zabbix_host}/zabbix/api_jsonrpc.php | jq -r .result[].groupid
```

### ホスト追加

```bash
curl -s -d '
{
    "jsonrpc": "2.0",
    "method": "host.create",
    "params": {
        "host": "Host1",
        "interfaces": [
            {
                "type": 1,
                "useip": 1,
                "ip": "'${host_ip}'",
                "dns": "",
                "port": "'${port}'",
                "main": 1
            }
        ],
        "groups": [
            {
                "groupid": "'${hostgroup_id}'"
            }
        ]
    },
    "auth": "'${auth}'",
    "id": 1
}
' -H "Content-Type: application/json-rpc" http://${zabbix_host}/zabbix/api_jsonrpc.php | jq -r
```

### リストファイル

```bash
hostgroup_hostname,type,IPaddress,port
 テスト;Host1;1;192.168.137.10;10051
```

type: > 1:agent > 2:SNMP > 3:IPMI > 4:JMX

- [ ]  リストファイルのフォーマット決める

## ホストID取得

hostname,hostid,interfaceid 取得

```bash
curl -s -d '{
    "auth": "'${auth}'",
    "method": "host.get",
    "id": 1,
    "params": {
        "output":"extend",
        "selectInterfaces":"extend"
    },
    "jsonrpc": "2.0"
}' -H "Content-Type: application/json-rpc" http://${zabbix_host}/zabbix/api_jsonrpc.php | jq -r '.result[]|.name+","+.hostid+","+.interfaces[].interfaceid'
```

applicationid取得

```bash
application_id=`curl -s -d '{
    "auth": "'${auth}'",
    "method": "application.get",
    "id": 1,
    "params": {
        "output":"extend",
        "selectInterfaces":"extend"
    },
    "jsonrpc": "2.0"
}' -H "Content-Type: application/json-rpc" http://${zabbix_host}/zabbix/api_jsonrpc.php | jq -r '.result[]|.name+","+.applicationid' | grep "HealthCheck" | awk -F',' '{print $2}'`
```

### アイテム追加

[https://kyou.hatenadiary.jp/entry/2018/01/09/232555](https://kyou.hatenadiary.jp/entry/2018/01/09/232555)

エクセル表からパラメータ取得

登録パラメータをリストファイル化

一部IをID化

```json
Params
{ "hostid":"Host1","name":"HealthCheck|Agent Ping","key_":"agent.ping","delay":"60","history":"396d","trends":"396d","type":"0","applications":"HealthCheck","status":"0","value_type":"3"}
```

```bash
curl -s -d '{
    "jsonrpc": "2.0",
    "method": "item.create",
    "params": [
        {
            "hostid": "'${hostid}'",
            "name": "HealthCheck|Agent Ping",
            "key_": "agent.ping",
            "delay": "60",
            "history": "396d",
            "trends": "396d",
            "type": "0",
            "applications": "'${application_id}'",
            "interfaceid": "'${interface_id}'",
            "status": "0",
            "value_type": "3"
        }
    ],
    "auth": "'${auth}'",
    "id": 1
}
' -H "Content-Type: application/json-rpc" http://${zabbix_host}/zabbix/api_jsonrpc.php | jq -r
```
