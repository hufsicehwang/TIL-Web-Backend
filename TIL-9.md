# 🦄TIL-9🦄

# 클래스 기반 listview
```py
class ClassList_t(ListView):
    model = Room
    template_name = 'myClass_t.html'
    def get_queryset(self): 
        QuerySet = Room.objects.filter(maker = self.request.session.get('user_email')).order_by('-make_date')  
        return QuerySet
```
- `model = Room` => 모델 지정
- `template_name = 'myClass_t.html'` => 반환 할 html 코드 지정
- `QuerySet = Room.objects.filter(maker = self.request.session.get('user_email')).order_by('-make_date')` => 반환할 queryset 지정

## 클래스 기반 listview의 html에서 queryset 컨트롤
```html
{%if object_list %}
    {% for room in object_list %}
            {{room.name}}, {{room.price}}
    {% endfor %} 
{% endif %}
```
- `object_list`로 컨트롤

# 함수 기반 listview
```py
def classList_s(request):
    page = request.GET.get("page",1)
    class_list = models.Enrol.objects.all()
    paginator = Paginator(class_list,10,orphans=5)
    res_data = {}
    
    try:
        classes = paginator.page(int(page))
    except EmptyPage:
        pass
    res_data["page"] = classes

    if request.method == 'GET':
        return render(request,'myClass_s.html',res_data)
    elif request.method == 'POST':
        return  render(request,'myClass_s.html',res_data)
```
- `class_list = models.Enrol.objects.all()`  => queryset을 내가 원하는 변수로 지정
- 'paginator = Paginator(class_list,10,orphans=5)' => list를 페이지 형태로 보여 줄때 page 당 보여줄 데이터 갯수 지정
- 함수 기반 listview의 장점 => post 처리를 내가 알던 대로 할 수 있다. (클래스 기반은 어떻게 하는지 모르겠음,,)
   
## 함수 기반 listview의 html에서 queryset 컨트롤
```html
{%if page.object_list %}
    {% for room in page.object_list %}
            {{room.name}}, {{room.price}}
    {% endfor %} 
{% endif %}
```
- `page.object_list`로 컨트롤
