🐲TIL-3🐲

# template 상속
1. base가 되는 html 상단에 `{% load static %}` 추가
2. ` {%block content%}` , `{% endblock %}` 구간 지정하기
3. settings의 templates에서  `'DIRS': [os.path.join(BASE_DIR,'프로젝트이름/templates')]` 설정하기
4. 상속받은 html에서, `{%extends 'base.html'%}` , `{% block content%}` , `{% endblock%}` 설정하기

