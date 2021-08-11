🐲TIL-8🐲

# 모델 객체
- 장고 db의 row를 뜻한다. 
## 저장하기
### view.py
```py
elif request.method == 'POST':
          // POST로 받은 데이터들을 변수로 저장
        email = request.POST.get('email', None)
        username = request.POST.get('username', None)
        password = request.POST.get('password', None)
        re_password = request.POST.get('re-password', None)
        
        // 각 변수를 모델 클래스의 매개변수=인자값 으로 넣어줌
        user = User(email=email, username=username,
                    password=make_password(password))  
        user.save()  // 저장
          return render(request, 'home.html', res_data)
```
- View의 POST 처리에서 form의 input 태그들의 값을 가져와서 저장한다. 항상 마지막에 `객체.save()` 를 해야 할 것을 유념하자.
## 가져오기
### view.py
```py
email = request.POST.get('email', None)
user = User.objects.get(email=email)
email = user.email
password = user.email
```
- 해당 객체를 특정 조건으로 가져와서 db의 row의 colum을 접근 할 수 있다.
#### get
```py
user = User.objects.get(email=email)
```
- email 변수와 같은 것만 반환 한다. get은 해당 객체가 2개 이상일 경우 error가 발생한다.
- 보통 객체 마다 부여되는 고유의 숫자(번호, 순서)인 `pk`를 통해 get 한다.
#### all
```py
user = User.objects.all()
```
- 해당 객체를 조건없이 모두 가져온다.
#### filter
```py
user = User.objects.filter(email=email)
```
- get과 같이 특정 조건으로 객체를 반환 받는데, get과 다르게 여러 객체를 반환한다.
#### exclude
```py
user = User.objects.exclude(email=email)
```
- 해당 조건을 배제하고 모든 객체를 반환한다.
#### count
```py
user = User.objects.count()
```
- row(객체)의 수를 반환 받을 수 있다.
```py
user = User.objects.filter(email=email).count()
```
- 위 처럼 fillter를 추가하면 특정한 조건으로 row(객체)의 수를 반환 받을 수 있다.
#### order_by
```py
user = User.objects.order_by('email','-make_date')
```
- `email`은 email을 기준으로 올림차순, `-make_date`은 make_date를 기준으로 내림차순 으로 정렬한다.
#### first
```py
user = User.objects.first(email=email)
```
- 제일 처음의 row 즉, 가장 높은 pk 값의 row(객체)가 반환된다.
#### last
```py
user = User.objects.last(email=email)
```
- 제일 마지막의 row 즉, 가장 낮은 pk 값의 row(객체)가 반환된다.

# session
- session이라는 변수에 저장되어 web의 cookie에 남게 된다.
- 한번 저장되면 다른 url 즉, 다른 view의 함수에서도 사용할 수 있는 데이터이다.
- session은 key, value를 가지는 딕션어리 변수이다
```py
request.session['user_email'] = user.email  # session 변수에 저장
```
- 세션 변수에 저장
```py
user_session = request.session.get('user_email')  # 저장한 session에서 값 가져오기
```
- 세션 변수에 user_email 이라는 key에 저장된 값을 가져옴
