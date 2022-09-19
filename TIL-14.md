## 🦄Swagger란
- 개발한 Rest API를 편리하게 문서화 해주고, 이를 통해서 관리 및 제 3의 사용자가 편리하게 API를 호출해보고 테스트 할 수 있는 프로젝트
- API를 테스트, 검증하는데 편리함
- frontend와 backend 사이에 협업에 편리함을 제공

## 🦊Swagger 연동
#### 1. main `urls.py`에서 설정
```py
  schema_view = get_schema_view(
    openapi.Info(
        title="Statchung API",
        default_version='v1',
        description="Test description",
        terms_of_service="https://www.google.com/policies/terms/",
        contact=openapi.Contact(email="contact@snippets.local"),
        license=openapi.License(name="BSD License"),
    ),
    public=True,
    permission_classes=[permissions.AllowAny],
)

if settings.DEBUG:
    urlpatterns += [
        re_path(r'^swagger(?P<format>\.json|\.yaml)$', schema_view.without_ui(cache_timeout=0), name='schema-json'),
        re_path(r'^swagger/$', schema_view.with_ui('swagger', cache_timeout=0), name='schema-swagger-ui'),
        re_path(r'^redoc/$', schema_view.with_ui('redoc', cache_timeout=0), name='schema-redoc'),
    ]
```

#### 2. API를 작성하는 function위에 `@swagger_auto_schema()` 어노테이션을 통해 작성
- request header 정의
```py
    # header
    authorization_customer = openapi.Parameter(
        'Authorization-Customer',
        openapi.IN_HEADER,
        description='HTTP_AUTHORIZATION_CUSTOMER - 고객용 쇼핑몰 토큰(클레이풀)',
        type=openapi.TYPE_STRING
    )
```
- swagger 어노테이션
```py
  @swagger_auto_schema(
        operation_summary="[황한식] 고객 자녀 관계 추가",
        operation_description="고객 자녀 관계 추가",
        operation_id='CUSTOMER_011',
        tags=['CUSTOMER'],
        manual_parameters=[authorization_customer],
        request_body=openapi.Schema(
            type=openapi.TYPE_OBJECT,
            properties={
                'customerId': openapi.Schema(type=openapi.TYPE_STRING, example="1", description="추가하고자 하는 고객 ID"),
                'customerFamilyId': openapi.Schema(type=openapi.TYPE_STRING, example="", description="추가하고자 하는 자녀 ID"),  
            }
        ),
        responses={
            200: openapi.Schema(
                type=openapi.TYPE_OBJECT,
                properties={
                    'data': openapi.Schema(type=openapi.TYPE_STRING, example=""),
                }
            ),
            400: openapi.Schema(
                type=openapi.TYPE_OBJECT,
                properties={
                    'errMsg': openapi.Schema(type=openapi.TYPE_STRING, example=""),
                }
            )
        }
    )
```
> request_body에 POST 메소드의 data를 담음
> > responses에서 응답 표준 정의

- request 예시



![image](https://user-images.githubusercontent.com/67450413/190990305-2b6aaf71-d982-4436-86e6-9abb774aa887.png)


- response 예시
![image](https://user-images.githubusercontent.com/67450413/190990452-e9d4f47d-b8ab-4310-a6f8-3cae805b08f4.png)

