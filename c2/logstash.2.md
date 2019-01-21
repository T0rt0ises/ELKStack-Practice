# 2.2.2 GROK介绍
## 语法介绍

## 在线调试
https://grokdebug.herokuapp.com/
## 常见介绍
### Nginx Error日志
**grok**
```
grok {        
    match => ["message", "(?<timestamp>%{YEAR}[./]%{MONTHNUM}[./]%{MONTHDAY} %{TIME}) \[%{LOGLEVEL:severity}\] %{POSINT:pid}#%{INT:threadid}: (\*%{INT:connectionid})?%{GREEDYDATA:msg}"]
    remove_field => "message"
}
```
### 自定义Nginx Access日志1
```nginx
log_format jsonlog '$time_local|$remote_addr "$request_method $http_host $request_uri" $content_length/$body_bytes_sent $status/$upstream_status $request_time/$upstream_connect_time/$upstream_response_time "$upstream_addr" "$http_user_agent" "$cookie_SESSION" "$http_kc_api_key" "$http_x_forwarded_for"';
```
**grok**
```
grok {
        match => ["message", "%{HTTPDATE:time}\|%{IP:client} \"%{WORD:method} %{DATA:http_host} %{DATA:uri}\" (-|%{INT:length})/(-|%{INT:resp_length}) %{INT:status}/%{INT:upstream_status} %{NUMBER:request_time}/%{NUMBER:upstream_connect_time}/%{NUMBER:upstream_resp_time} \"%{DATA:upstream}\" \"%{DATA:agent}\" \"%{DATA:session}\" \"%{DATA:api_key}\" \"%{DATA:xff}\""]
}
mutate {
        convert => {
                "length" => "integer"
                "resp_length" => "integer"
                "status" => "integer"
                "upstream_status" => "integer"
                "request_time" => "float"
                "upstream_connect_time" => "float"
                "upstream_resp_time" => "float"
        }
        remove_field => [ "message" ]
}
```

