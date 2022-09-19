# 🦁Django 효율적인 packaging 방법
### 1. 특정 App내에서 동작에 맞게 views file을 분리하여 source code를 작성한다. 



 ![image](https://user-images.githubusercontent.com/67450413/190945716-e0ac28f2-6862-456a-9c08-34a590d7fca5.png)
 
 
 
### 2. `views.py`에서 모든 view 내용을 `import` 한다.
```py
  from .views_check import *
  from .views_user import *
  from .views_permission import *
```
### 3. `urls.py`에서 경로마다 특정 Class의 function에 `GET`, `POST`를 mapping 한다.
```py
    path('email/confirm', VwUser.as_view({"get": "confirm_email_verification"})),
    path('email/verify', VwUser.as_view({"post": "request_email_verification"})),
    path('users/permissions', VwUserPermission.as_view({"get" : "get_user_permissions" , "post": "register_user_permission"})),
```

## TIP) Model 관리
- App 내부 별로 model.py를 작성하지 않고 model을 따로 packaging한다.



![image](https://user-images.githubusercontent.com/67450413/190946324-3a00d7e6-97af-47d5-bc48-da7790ff0c0e.png)
