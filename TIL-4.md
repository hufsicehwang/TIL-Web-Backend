🐶TIL-4🐶

# render
- render를 유용하게 사용하기 위해선 request, html파일 뿐만 아니라 딕션어리 변수를 반환 해야 한다.

### view
```py
def study(request):
  res_data = {}
  res_data['name'] = '황한식'
  return render(request, index.html, res_data)
```
- html 파일에 딕션어리 변수인 res_data를 함께 반환 한다.

### templates (index.html)
```html
<a class="test">{{name}}님 안녕하세요.</a>
```
- 반환된 딕션어리 변수 res_data의 key 값인 name을 `{{name}}` 과 같은 형태로 변수에 접근 할 수 있다.

# redirect
- `from django.shortcuts import redirect` import 한다음
- `return redirect('/login')` 과 같이 특정 url을 지정 할 수 있다.
- render는 해당 함수에서 render를 통해 반환 하지만 redirect는 url을 지정하기 때문에 해당 url 경로에 지정된 view의 다른 함수에서 반환한다.

