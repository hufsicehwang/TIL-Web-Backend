🐘TIL-1🐘

# Framework란?
- 자주 사용되는 코드를 체계화하여 쉽게 사용할 수 있도록 도와주는 코드 집합이다.
- 라이브러리와 혼동 하지 않도록 주의하자. 

# Django
1. (M)모델 계층 : 데이터 베이스 관리
2. (V)뷰 계층 : 서버와의 통신(request, response)
3. (T)템플릿 계층 : 레이아웃 담당, front 만의 정적인 언어에서 조건문, 반복분 사용 가능하게 해준다.
``` 
ex) {% csrf_token %} : form 형식 안에 필수적으로 포함해야하는 토큰
```

# django MTV 설정하기
1. 프로젝트 생성하기
``` 
django-admin startproject 프로젝트이름
```
    
2. 앱 생성하기
``` 
django-admin startproject 앱이름
```
    
3. 앱안에 templates 생성하기

4. 프로젝트(최하단 폴더)에 settings 파일에서 `INSTALLED_APPS`에 앱 이름 추가하기
    - 기본 app은 MV를 가지지만 T는 가지지 않음으로 따로 templates 폴더를 만들어 준다.

# Model 만들기
```python
class User(models.Model):
    username = models.CharField(max_length=64, verbose_name="사용자명")
    password = models.CharField(max_length=64, verbose_name="비밀번호")
    registerd_date = models.DateTimeField(auto_now_add=True,verbose_name='등록시간')

    class Meta:
        db_table = 'hasik_user'
```
- 클래스안에 클래스를 만듬으로써 db의 태이블 이름을 지정 할 수 있다.
- python manage.py makemigrations
- python manage.py migrate
- 모델이 바뀌면 위의 두단계를 다시 실행 해야함!!

# 간단 명령어
1. 서버 시작
 ```
 py manage.py runserver
 ```
2. 서버 끝내기
 ```  
  Ctrl+c -> runserver 끝내기
 ```
