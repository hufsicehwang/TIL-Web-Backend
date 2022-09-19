## 🐲 response logging
- response 내용을 terminal에 loggging 하고 응답
### 1. Django Rest Framework에서는 화면 rendering 없이 JSON 형태로 응답
- 응답 예시
```py
  return rtn_rsp(result=result_list, status_code=status.HTTP_200_OK)
```
```py
  return rtn_rsp(result=result_dict, status_code=status.HTTP_400_BAD_REQUEST)
```
```py
  return rtn_rsp(result=result_dict, status_code=status.HTTP_401_UNAUTHORIZED)
```
```py
  return rtn_rsp(result=result_dict, status_code=status.HTTP_500_INTERNAL_SERVER_ERROR)
```
> `INTERNAL_SERVER_ERROR`는 응답 실행하는 함수의 내용을 try-except으로 감싼다.

### 2. example of response function
```py
      def register_user_permission(self, request):
        try:
            result_dict = dict()

            # auth request user
            system_user_permission, err = authorization_system_user(request)
            if system_user_permission is None:
                result_dict['err_msg'] = err
                return rtn_rsp(result=result_dict, status_code=status.HTTP_401_UNAUTHORIZED)

            # create permission
            result_list, err = register_permission(request, system_user_permission)
            if result_list is None:
                result_dict['err_msg'] = err
                return rtn_rsp(result=result_dict, status_code=status.HTTP_400_BAD_REQUEST)
                
            # success
            return rtn_rsp(result=result_list, status_code=status.HTTP_200_OK)
            
        except Exception as ex:
            print('register_user_permission__ex:', str(ex))
            result_dict = dict()
            result_dict['err_msg'] = str(ex)
            return rtn_rsp(result=result_dict, status_code=status.HTTP_500_INTERNAL_SERVER_ERROR)
```

### 3. example of response logging
```py
  # for logging
  logger = logging.getLogger()
  
  
  def rtn_rsp(result={}, msg='', status_code=None):
    
    # response type 지정
    result_type = ["<class 'collections.OrderedDict'>", "<class 'collections.defaultdict'>", "<class 'dict'>","<class 'list'>"]
    rtn_status_code = status_code
    
    # rtn_status = 'fail'
    if not status_code:
        rtn_status_code = status.HTTP_400_BAD_REQUEST
        
    # rtn_status = 'success'
    elif status_code not in [status.HTTP_200_OK, status.HTTP_201_CREATED]:
        rtn_status_code = status_code

    logger.debug(f'rtn_rsp : result type check {str(type(result))}')

    if str(type(result)) not in result_type:
        result = {
            'error' : str(result),
            'msg': msg
        }

    logger.debug(f"response: {result}")
    return Response(data=result, status=rtn_status_code)
```
> `loggr.debug()`는 개발 단계에서 디버깅용으로 사용되며 build 되지 않아서 신경쓰지 않고 배포가능!
