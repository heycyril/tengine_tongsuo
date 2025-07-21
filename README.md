## 通过编译tengine和tongsuo ，使tengine支持国密ssl证书，  
centos需要提前安装这些命令，统信及其他环境因无arm环境请自行探索，  
#安装需要的依赖，本次安装为centos7.9环境和银河麒麟v10环境下测试成功，以及编译进去部分模块  
[root@161 ]#yum install -y gcc pcre pcre-devel zlib zlib-devel geoip-devel  perl-IPC-Cmd perl-Data-Dumper perl-IPC-Cmd perl-Text-Template  
[root@161 ]# tar -xf tengine-3.1.0.tar.gz  -C /usr/local/  
[root@161 ]# cd /usr/local/tengine-3.1.0/  
#注意此处--with-openssl=../Tongsuo-8.4.0/ 为解压后的源码目录，不是编译后的目录  
[root@161 Tongsuo-8.4.0]# ./configure --prefix=/usr/local/tengine  \  
--add-module=modules/ngx_tongsuo_ntls  \  
--with-openssl=../Tongsuo-8.4.0/   \  
--with-openssl-opt="enable-ntls"  \  
--with-http_ssl_module   \  
--with-http_v2_module  \  
--with-http_realip_module  \  
--with-http_stub_status_module   \  
--with-http_gzip_static_module   \  
--with-http_sub_module  \  
--with-http_addition_module   \  
--with-http_flv_module   \  
--with-http_mp4_module   \  
--with-http_secure_link_module  \  
--with-http_auth_request_module  \  
--with-http_geoip_module  \  
--with-cc-opt="-I/usr/local/include" \  
--with-ld-opt="-L/usr/local/lib"  \  
--with-http_slice_module  \  
--with-mail_ssl_module  \  
--with-stream \  
--with-stream_ssl_module \  
--with-stream_sni \  
--add-module=./headers-more-nginx-module-master  
  
  
[root@161 Tongsuo-8.4.0]# make -j  
[root@161 Tongsuo-8.4.0]# make install  
  
  
## 一、Tengine 编译常用模块分类  
## 1. 核心功能模块  
- 这些是 Tengine 自带的核心模块，建议默认启用：  
## 模块名称	说明  
--with-http_ssl_module	支持 HTTPS/SSL  
--with-http_v2_module	支持 HTTP/2  
--with-http_realip_module	获取真实客户端 IP（如在反向代理后）  
--with-http_stub_status_module	获取服务器状态信息  
--with-http_gzip_static_module	支持预压缩的 .gz 文件  
--with-http_gunzip_module	支持不兼容 gzip 的客户端  
--with-http_sub_module	支持内容替换（如替换页面中的关键字）  
--with-http_addition_module	支持前后内容插入（如插入 JS/CSS）  
--with-http_flv_module	支持 FLV 流媒体  
--with-http_mp4_module	支持 MP4 流媒体  
--with-http_concat_module	支持 JS/CSS 文件合并（Tengine 特有）  
--with-http_sysguard_module	系统保护模块（如负载过高时限制请求）  
--with-http_upstream_check_module	健康检查模块（Tengine 特有）  
--with-http_lua_module	支持 Lua 脚本（需 LuaJIT）  
--with-http_slice_module	支持大文件分片下载（HTTP Range）  
