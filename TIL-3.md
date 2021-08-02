🦄TIL-3🦄

# django MTV

# Model
### 1. model.py 설정
```python
class User(models.Model):
    email = models.EmailField(max_length=128, verbose_name="아이디")
    username = models.CharField(max_length=64, verbose_name="사용자명")
    password = models.CharField(max_length=64, verbose_name="비밀번호")
    registerd_date = models.DateTimeField(auto_now_add=True, verbose_name='등록시간')
    
    def __str__(self):
        return self.email
        
    class Meta:
        db_table = 'hasik_user'
        verbose_name = '사용자 명단'
        verbose_name_plural = '사용자 명단'  # 복수명 설정
```
- 클래스안에 클래스를 만듬으로써 db의 태이블 이름을 지정 할 수 있다.

### 2. admin.py 설정
```py
from django.contrib import admin
from .models import User
# Register your models here.

class UserAdmin(admin.ModelAdmin):
    list_display =('email','password','username','registerd_date')

admin.site.register(User, UserAdmin)
```
### 3. 명령어
- `py manage.py makemigrations` 를 통해 모델 만들기
-  `py manage.py migrate`를 통해 모델 등록하기

# Templates
- 각 앱마다 `templates`를 폴더를 직접 만들어야 한다.
- `templates` 폴더 안에서 html 파일을 만들어 작성한다.
- form 태그의 submit을 통해 view에서 자기 자신을 반환 했던 함수를 다시 실행 시킬 수 있다.
  - form의 button은 기본적으로 submit을 수행하기 때문에 버튼 클릭시 POST 요청이 되는 것이다.
- css 파일은 `static`폴더에서 관리한다.
- 반환 받은 딕션어리 변수를 `{{key_이름}}` 과 같이 key 값을 이용해 사용 가능하다.

# View
- 실행 시킬 각 함수를 작성해서 html 파일을 반환 시킬 수 있다.
- request가 GET, POST에 따라 다르게 반환 값을 부여 할 수 있다.
- html의 form태그에서 submit을 한다면 POST 요청으로 함수가 실행 될 수 있다.
- __key, value를 가지는 딕션어리 변수를 선언하여 html 파일로 반환 할 수 있다.__
