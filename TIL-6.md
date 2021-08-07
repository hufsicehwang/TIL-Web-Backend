🐲TIL-6🐲

# 두개의 POST 따로 처리하는 방법
- 한 탬플릿 안에 두개의 form이 존재하여 원하는 POST 요청이 두개일 경우 어떻게 처리 할 것인가?

## 해결 방안
- 두개의 form에서 submit을 담당하는 버튼의 value 값을 읽어 와서 처리하자!
- view에서 submit 버튼의 post 값은 value 이다!

## html
```html
<form method="POST" id="makeroom" name="makeroom"  enctype="multipart/form-data">
  {% csrf_token %}
        <!-- 크로스 도메인을 막기 위해 사용하느 코드 -->
        <label for="">Room name
            <input type="text" value="{{string}}" class="room-name" id="room-name" name="room-name" readonly> 
        </label>
        <label for="">Password
            <input type="text" placeholder="Room 비밀번호를 입력하세요." class="room-password" id="room-password" name="room-password" autocomplete="off">
        </label>
</form>

<form method="POST" id="edit" name="edit" enctype="multipart/form-data">
    {% csrf_token %}
    <label for="user-img-change" class="user-img-change">이미지 설정하기</label>
    <input type="file" id="user-img-change" name="user-img-change" accept=".png, .jpeg, .jpg">
    <button class="user-img-btn" id="user-img-btn" name="user-img-btn" value="모달">변경하기</button>
  </form>
```
- 이와 같이 두개의 폼이 존재 한다면, 예를들어 하나의 sumit 버튼에 `value="모달"` 과 같이 value를 부여한다.

## view
```py
if request.method == 'GET':
     return render(request, 'makeroom.html', res_data)
elif request.method == 'POST':
    post_type = request.POST.get('user-img-btn')  # POST를 시작할때 value="모달"의 id를 통해서 value를 얻는다.
    if post_type == "모달":  # 버튼 값을 읽어 POST 구분한다.
        # value = "모달"인 POST 처리
        
    else: 
        # 다른 POST 처리
```
- view의 함수에서 POST가 시작하자마자 버튼의 value를 읽어 POST를 구분한다.
