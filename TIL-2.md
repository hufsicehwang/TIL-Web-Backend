🐴TIL-2🐴

# static 폴더관리
: CSS나 JS를 위한 폴더이다.
1. settings의 제일 하단에 `STATICFILES_DIRS = [os.path.join(BASE_DIR,'static')]` 작성
2. bootstrap 4.3 theme free 검색 후 bootswatch에서 원하는 테마 다운
3. static 폴더에 드래드&드랍하기
4. html에서 link 설정하기

# 페이지 변경의 흐름
### 1. urls.py
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

