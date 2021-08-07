🐲TIL-7🐲

# form 태그안에 토큰
- django에서는 크로스 도메인을 막기 위해서 form태그 바로 하단에 토큰을 지정해야한다.
```html
<form id="login" action="" method="POST">
    {% csrf_token %}
    <!-- 크로스 도메인을 막기 위해 사용하느 코드 -->
    <input type="email" id="email" name="email" placeholder="Email">
    <input type="password" id="password" name="password" placeholder="password">
</form>
```

# template 상속
1. base가 되는 html 상단에 `{% load static %}` 추가
2. ` {%block content%}` , `{% endblock %}` 구간 지정하기
3. settings의 templates에서  `'DIRS': [os.path.join(BASE_DIR,'프로젝트이름/templates')]` 설정하기
4. 상속받은 html에서, `{%extends 'base.html'%}` , `{% block content%}` , `{% endblock%}` 설정하기

# template 변수
- view에서 html파일을 반환해주면서 딕션어리 변수를 같이 반환 해주면 변수를 사용할 수 있다.

### view
```py
res_data = {}
res_data['username'] = "황한식"
return render(request, 'html파일', res_data)
```

### html
```html
<div> {{username}}</div>
```
- html 파일에서 `황한식`이 정상적으로 보여진다.

# template 조건문, 반복문
- django의 html 파일에서 조건문과 반복문을 사용할 수 있다.

```html
<div class="list-wapper">
    {% if ... %}
        # if 조건문 처리
        {% for ... in ... %}
            # for 반복문 처리
        {% endfor%}
    {% else %}
        # else 조건문 처리
    {%endif%}
</div>       
```
- if 조건문과 for 반복문이 끝나면 항상 `{%endif%}` , `{% endfor%}` 나타내야 한다.
