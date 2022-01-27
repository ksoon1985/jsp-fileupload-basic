# jsp-fileupload-basic

jsp fileupload basic  
  
  
## 정리

* fileupload01 - **MultipartRequest**를 사용한 단순 파일업로드
* fileupload02 - **MultipartRequest**를 사용한 다중 파일업로드
* fuleupload03,04 - **DiskFileUpload**를 사용한 단순 파일업로드 

## 업로드 위치

```
String uploadPath = request.getSession().getServletContext().getRealPath("/uploadFile");
```
  
