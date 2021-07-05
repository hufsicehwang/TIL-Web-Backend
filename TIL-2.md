🐴TIL-2🐴

# static 폴더관리
: CSS나 JS를 위한 폴더이다.
1. settings의 제일 하단에 `STATICFILES_DIRS = [os.path.join(BASE_DIR,'static')]` 작성
2. bootstrap 4.3 theme free 검색 후 bootswatch에서 원하는 테마 다운
3. static 폴더에 드래드&드랍하기
4. html에서 link 설정하기

# 페이지 변경의 흐름
### 1. urls.py (project)
```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('user/', include('user.urls')),  # community url-> user url
]
```
: 만약 url이 `user/`로 시작하면 그것은 user의 urls.py로 간다.  (user는 app 이름)

### 2. urls.py  (app)
```python
urlpatterns = [
   path('login/', views.login)
]
```
: 만약 url이 `login/` 이면 views의 login 함수를 호출한다.

### 2. views.py
```python
def login(request):
    return render(request, 'login.html')
```
: login 함수는 login.html 파일을 반환한다.

### 3. login.html 파일이 실행되어 보여진다.

# 새로운 페이지 만들기
- 페이지 변경 흐름에 따라 만들기, import 주의
- project에서 한번 지정해주고 app에서 한번 지정해 주는것이 url 경로가 된다.
- 위의 경우 `http://127.0.0.1:8000/user/login`    
    
