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
  
## 기본 코드

### MultipartRequest

```java
  String uploadPath = request.getSession().getServletContext().getRealPath("/uploadFile");
	MultipartRequest multi = new MultipartRequest(request,uploadPath,5 * 1024 * 1024,"utf-8",new DefaultFileRenamePolicy());

  // 단순 파라미터 
	Enumeration params = multi.getParameterNames();

	while (params.hasMoreElements()) {
		String name = (String) params.nextElement();
		String value = multi.getParameter(name);
		out.println(name + " = " + value + "<br>");
	}

  // 파일 파라미터 
	Enumeration files = multi.getFileNames();

	while (files.hasMoreElements()) {
		String name = (String) files.nextElement();
		String filename = multi.getFilesystemName(name);
		String original = multi.getOriginalFileName(name);
		String type = multi.getContentType(name);
		File file = multi.getFile(name);

		if (file != null) {
			out.println(" 파일 크기 : " + file.length());
			out.println("<br>");
		}
	}
```

### DiskFileUpload

```java
	String uploadPath = request.getSession().getServletContext().getRealPath("/uploadFile");
  
	DiskFileUpload upload = new DiskFileUpload();

	upload.setSizeMax(1000000);
	upload.setSizeThreshold(4096);
	upload.setRepositoryPath(uploadPath);

	List items = upload.parseRequest(request);
	Iterator params = items.iterator();

	while (params.hasNext()) {
		FileItem item = (FileItem) params.next();

		if (item.isFormField()) {
			String name = item.getFieldName();
			String value = item.getString("utf-8");
			out.println(name + "=" + value + "<br>");
		} else {
			String fileFieldName  = item.getFieldName();				
			String fileName = item.getName();
			String contentType = item.getContentType();

			fileName = fileName.substring(fileName.lastIndexOf("\\") + 1);
			long fileSize = item.getSize();

			File file = new File(uploadPath + "/" + fileName);
			item.write(file);
		}
	}
```
