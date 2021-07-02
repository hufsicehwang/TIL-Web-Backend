🐘TIL-1🐘

# Framework란?
- 자주 사용되는 코드를 체계화하여 쉽게 사용할 수 있도록 도와주는 코드 집합이다.
- 라이브러리와 혼동 하지 않도록 주의하자. 

# Django
1. 모델 계층     2. 뷰 계층      3. 템플릿 계층

# django MTV 설정하기
1. 프로젝트 생성하기
    - django-admin startproject 프로젝트이름
    
    
2 앱 생성하기 
    - django-admin startproject 앱이름
    
3. 앱안에 templates 생성하기

4. 프로젝트(최하단 폴더)에 settings 파일에서 `INSTALLED_APPS`에 앱 이름 추가하기

- 기본 app은 MV를 가지지만 T는 가지지 않음으로 따로 templates 폴더를 만들어 준다.

