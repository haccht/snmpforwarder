SNMP Trapを受け取り、TrapのOIDによってログ書き込み/スクリプト起動/Trap転送を行う。

## 使い方
```
trapproxy -config config.toml
```

162番ポートで待ち受ける場合はroot権限が必須。

## 設定ファイル

設定はTOML形式で記述する

```
[source]
version = "2c"
community = "public"
address = ":162"

# Log all traps to logfile
[[pipe]]
  oid = "." # "." matches all OIDs.
  [pipe.file]
  path = "/path/to/logfile"

# Drop OIDs that starts with ".1.3.6.1.6.3.1.1.5.4"
[[pipe]]
  oid = ".1.3.6.1.6.3.1.1.5.4"
  drop = true

# Drop OIDs that starts with ".1.3.6.1.6.3.1.1.5.3" and execute a command
[[pipe]]
  oid = ".1.3.6.1.6.3.1.1.5.3"
  drop = true
  [pipe.exec]
  command = "/path/to/command"

# Forward all traps except ".1.3.6.1.6.3.1.1.5.4" and ".1.3.6.1.6.3.1.1.5.3"
[[pipe]]
  oid = "."
  [pipe.forward]
  version = "1"
  community = "public"
  address = "10.10.10.10:161"
```