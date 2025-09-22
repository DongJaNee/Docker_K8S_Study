## Docker Compose 
### Dockerfile 생성 
```
nano Dockerfile
```
```
ex)

FROM drupal:10-apache

RUN apt-get update && apt-get install -y git \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /var/www/html/themes

RUN git clone --branch 8.x-3.x --single-branch --depth 1 https://git.drupal.org/project/bootstrap.git \
    && chown -R www-data:www-data bootstrap

WORKDIR /var/www/html

```

<img width="856" height="229" alt="image" src="https://github.com/user-attachments/assets/0f1cb17d-06f6-49b8-b015-2da988eab460" />

---
### docker-compose.yaml 파일 생성 
```
nano docker-compose.yaml
```
```
ex)
version: '2'

services:

  drupal:
    image: custom-drupal
    build: .
    ports:
      - "8080:80"
    volumes:
      - drupal-modules:/var/www/html/modules
      - drupal-profiles:/var/www/html/profiles       
      - drupal-sites:/var/www/html/sites      
      - drupal-themes:/var/www/html/themes
 
  postgres:
    image: postgres:12.1
    environment:
      - POSTGRES_PASSWORD=mypasswd
    volumes:
      - drupal-data:/var/lib/postgresql/data

volumes:
  drupal-data:
  drupal-modules:
  drupal-profiles:
  drupal-sites:
  drupal-themes:
<img width="499" height="792" alt="image" src="https://github.com/user-attachments/assets/81bd9ee5-1fd6-4593-ad48-574ef98a0976" />
```

### Docker compose 폴더 생성 후 이동
```
mkdir docker-compose-test
cd docker-compose-test
```

### docker compsoe 폴더로 Dockerfile과 docker-compose.yaml 이동 
```
mv ../Dockerfile ./
mv docker-compose.yaml ./
```

<img width="1110" height="99" alt="image" src="https://github.com/user-attachments/assets/44c47196-51e6-4205-9866-bbd86aad2239" />

---

### docker compose 실행
```
docker-compose up -d    # 실행

docker-compose ps    # 확인

docker-compose stop    # 정지

docker-compose start    # 시작

docker-compose down    # 종료
```

### docker compose 실행 결과 확인
https://localhost:8080
