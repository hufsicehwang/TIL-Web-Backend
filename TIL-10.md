# 🦁TIL-10🦁

# template 변수
- 형식 : {{변수}}
- view 파일에서 res_data의 key 값을 render 해주고 사용 가능

# template 태그
- {%for%} {%endfor%}   : 반복문 (보통 queryset을 컨트롤 할때 사용)
  -    ![image](https://user-images.githubusercontent.com/67450413/142205258-383c1e98-7bc2-4446-9981-36042d66449c.png)

- {%if%} {%else%} {%endif%}  : 조건문
- {% with %}{% endwith%} : 변수 선언 가능
  - ```html
      {% with room_id = room.room_id%}
        {{room_id}} 
      {% endwith%}
    ```   
