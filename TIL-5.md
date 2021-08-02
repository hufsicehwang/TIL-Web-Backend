🐱TIL-5🐱

# GET
- requst가 GET일 경우는 url을 새로 지정하거나 처음으로 html 파일이 반환 되었을 때이다.
- 보통 GET일 경우 특별한 반환을 가지지 않고 기본 html을 반환한다.

# POST
- requst가 POST일 경우는 form 태그 안에서 submit 하거나 새로고침(F5) 했을 경우이다.
- 일련의 과정들이 POST를 통해서 이루어지는 만큼 중요하다.

# POST 요청에 input의 값 가져 오기
## html
```html
<form method="POST">
  {% csrf_token %}
  <!-- 크로스 도메인을 막기 위해 사용하느 코드 -->
<input type="email" id="email" name="email" placeholder="Email">
<input type="password" id="password" name="password" placeholder="password">
</form>
```
- form의 method는 `POST`로 설정 해야한다.
- 크로스 도메인을 막기위해 `{% csrf_token %}` 를 폼 태그 아래에 반드시 작성 해야한다.
- __input태그의 id와 name 값을__ 둘다 같게 설정 해야한다.

## view
```py
def test(request):
    res_data = {}
    if request.method == 'GET':
        return render(request, 'home.html')
    elif request.method == 'POST':
        email = request.POST.get('email', None)
        password = request.POST.get('password', None)
```
- request의 메소드가 POST일때 `request.POST.get('email', None)` 와 같이 ID 값으로 input 태그의 값에 접근 할 수 있다.
