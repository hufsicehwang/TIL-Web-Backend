🐲TIL-3🐲

# template 상속
1. base가 되는 html 상단에 `{% load static %}` 추가
2. ` {%block content%}` , `{% endblock %}` 구간 지정하기
3. settings의 templates에서  `'DIRS': [os.path.join(BASE_DIR,'프로젝트이름/templates')]` 설정하기
4. 상속받은 html에서, `{%extends 'base.html'%}` , `{% block content%}` , `{% endblock%}` 설정하기

# 회원가입 구현 방법
1. model 작성 (TIL-2 참고)
2. admin.py 에서 
```py
class UserAdmin(admin.ModelAdmin):
    list_display =('username','password','registerd_date','email')

admin.site.register(User, UserAdmin)
```
: db에서 보여지는 순서 나열

3. __html의 form 태그에서 id와 name을 똑같이 설정하기!!!!__
```html
<input type="email" id="email" name="email" placeholder="Email" autocomplete="off">
<input type="text" id="username"  name="username" placeholder="이름" autocomplete="off">
<input type="password" id="password"  name="password" placeholder="비밀번호" autocomplete="off">
<input type="password" id="re-password" name="re-password" placeholder="비밀번호 확인" autocomplete="off">
```
: id와 name을 똑같이 설정 하지 않으면 not null 에러 발생!!

4. view.py에서
```py
    if request.method =='GET':
        return render(request,'home.html')
    elif request.method == 'POST':
        email = request.POST.get('email',None)
        username = request.POST.get('username',None)
        password = request.POST.get('password',None)
        re_password = request.POST.get('re-password',None)

        if password != re_password:
            return HttpResponse('비밀번호가 다릅니다!')

        user = User(email=email,username=username,password=password)
        user.save()

        return render(request, 'home.html')    
```
     
# 로그인 구현
- views.py 에서
```py
def login(request):
    if request.method =='GET':
        return render(request, 'login.html')
    elif request.method =='POST':
        email = request.POST.get('email',None)
        password = request.POST.get('password',None)
        res_data ={}   # 딕션어리 = key, value 값을 가지는 변수
        if not(email and password):
            res_data['error'] = '모든 값을 입력하세요.'
        elif not(email):
            res_data['error'] = '이메일을 입력하세요.'
        elif not(password):
            res_data['error'] = '비밀번호를 입력하세요.'
        else:
            try:
                user = User.objects.get(email=email) # 필드명 = 값 이면 user 객체 생성
            except User.DoesNotExist:
                res_data['error'] = '존재하지 않는 아이디 입니다.'    # 아이디가 없는 예외 처리
                return render(request,'login.html',res_data)

            user_password = user.password
            if user_password == password:
                request.session['user'] = user.id  # session 변수에 저장
                return redirect('/main')
            else:
                res_data['error'] = '비밀번호가 틀렸습니다.'
        return render(request,'login.html',res_data) 
```
