**Nginx 1.12.0**

----

 **내용**
  + Https 확장을 염두로 443 port EXPOSE.

 **TroubleShot**
  + OpenSSL 1.1.1 version은 현재, Nginx에서 지원을 하지않는다. 따라서 1.0.2q버전으로 모듈 설치.
  