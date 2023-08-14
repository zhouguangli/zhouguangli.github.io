一、Apache代理https请求
修改httpd.conf 配置，引用ssl配置文件  Include conf/extra/httpd-ssl.conf
修改httpd-ssl.conf 配置，开启LoadModule ssl_module modules/mod_ssl.so
二、Apache代理websocket请求
tocc服务改成http协议，修改httpd.conf 配置，开启LoadModule proxy_wstunnel_module modules/mod_proxy_wstunnel.so
配置代理
#静态文件访问，如果静态文件访问不需要代理则静态文件的配置就不需要，下面第3步就不需要处理
ProxyPass /服务xxx/dist !
ProxyPass /服务xxx/ocx !

#websocket代理
ProxyPass /服务xxx/webSocketService balancer://wscluster/webSocketService
#http代理
ProxyPass /服务xxx/ balancer://orientsec.com.cn/
ProxyPassReverse /服务xxx/ balancer://orientsec.com.cn/

#http代理地址
<Proxy balancer://orientsec.com.cn/>
        BalancerMember http://IP:port/服务xxx/
</Proxy>
#websocket代理地址
<Proxy balancer://wscluster>
         BalancerMember ws://127.0.0.1:8080/tocc
</Proxy>
把静态文件复制到..\bin\apps\apache\htdocs\服务xxx目录下面
