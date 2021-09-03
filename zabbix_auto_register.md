### 変数
- login_user: ログインユーザID
- login_password: ログインパスワード
- zabbix_host: ホスト名
- auth: 認証トークン

- hostgroup_name: ホストグループ名
- hostgroup_id: ホストグループID
- host_name: 登録ホスト名
- type: 監視タイプ(agent,snmpなど)
- ipaddress: IP
- port: ポート

### 認証コード取得

```
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

```
curl -s -d '
{
    "jsonrpc": "2.0",
    "method": "hostgroup.get",
    "params": {
        "output": "extend",
        "filter": {
            "name": [
                "テスト"
                  ]
        }
    },
    "auth": "'${auth}'",
    "id": 1
}
' -H "Content-Type: application/json-rpc" http://${zabbix_host}/zabbix/api_jsonrpc.php | jq -r .result[].groupid
```

### ホストを追加する

```
curl -s -d '
{
    "jsonrpc": "2.0",
    "method": "host.create",
    "params": {
        "host": "'$HostName'",
        "interfaces": [
            {
                "type": '$IFType',
                "main": '$IFdefaultORnot',
                "useip": '$UseIP',
                "ip": "'$IPaddress'",
                "dns": "'$DNSname'",
                "port": "'$Port'"
            }
        ],
        "groups": [
            {
                "groupid": "'$HostGroupID'"
            }
        ]
    },
    "auth": "'${auth}'",
    "id": 1
}
' -H "Content-Type: application/json-rpc" http://${zabbix_host}/zabbix/api_jsonrpc.php | jq -r
```

### ホスト追加

```
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
                "ip": "192.168.137.10",
                "dns": "",
                "port": "10051",
                "main": 1
            }
        ],
        "groups": [
            {
                "groupid": "15"
            }
        ]
    },
    "auth": "'${auth}'",
    "id": 1
}
' -H "Content-Type: application/json-rpc" http://${zabbix_host}/zabbix/api_jsonrpc.php | jq -r

```

### リストファイル

```
hostname,type,IPaddress,port
 Host1;1;192.168.137.10;10051
```

type:
    > 1:agent
    > 2:SNMP
    > 3:IPMI
    > 4:JMX


### アイテム追加

https://kyou.hatenadiary.jp/entry/2018/01/09/232555

