**개인용 도커 베이스이미지 저장소**

----

 **내용**
  + 개인 프로젝트 및 기타 용도 도커 베이스 이미지 저장소. 

 **사용법**
  + git clone 후 빌드할 이미지가 있는 폴더로 이동. (ex cd nginx)
  ```bash
    docker build --force-rm=true -t seansin/{nginx/php5-fpm/ubuntu}:latest .
  ```