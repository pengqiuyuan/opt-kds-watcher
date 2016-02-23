## 文件说明

#### Dockerfile
```
FROM 10.0.29.249:5000/logstash
RUN mkdir /opt/sincedb
RUN echo "Asia/Shanghai" > /etc/timezone && dpkg-reconfigure -f noninteractive tzdata
```
- `RUN mkdir /opt/sincedb`：logstash shipper容器创建目录，保存日志记录信息的文件
 
#### kds agent.conf
```
input {   
 file {   
   type => "kds_game_log"   
   path => ["/home/ftp/logs/*/*fatal*.log"]
   exclude => ["*.gz"]
   tags => ["kdstest"]   
   start_position => beginning
   sincedb_path => "/opt/sincedb/.sincedb_opt_kds_warcher" 
 }   
}
output {   
 redis {   
   host => "10.0.29.107"
   port => "6378"
   data_type => "list"   
   key => "kdstest"
   workers => 6   
 }   
}
```
#### kds index.conf
```
input {   
 redis {   
   host => "10.0.29.107"   
   port => "6378"    
   data_type => "list"   
   key => "kdstest"
   codec => "json"
   threads => "6"
 }   
}

filter {

        mutate {
            split => ["message", "|"]
        }
	json {
            source => "[message][9]"
	}
	
        if "fatalstart" in [message][1]{
		mutate {
		    replace => { "type" => "kds_fatal.log" }
		}
		mutate {
			remove_field => ["host","path","tags" ]
		}		
	}

}


output {
	if [type] == "kds_fatal.log"{
		email {
		        from => "pengqiuyuan@126.com"
		        subject => "Kds 107 Error Alert"
		        to => "370020694@qq.com,80387591@qq.com"
		        via => "smtp"
		        htmlbody => "<h2>Kds 107 Error Alert</h2><br/><br/><div
		        align='center'>%{message}</div>"
		        options => [
		               "smtpIporHost", "smtp.126.com",
		               "port", "25",
		               "domain", "smtp.126.com",
		               "userName", "用户名",
		               "password", "密码",
		               "authenticationType", "login",
		               "starttls", "true"
		       ]
		}
	}
}
```
