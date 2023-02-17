#### requests module declaration

Author: Onceday	Date: 2022年12月10日

##### Module basic information

1. Based on webclient.c development, temporarily support the simplest get request and post request.
2. Additional support for simple URL concatenation on get requests.
3. Ability to specify additional request header keywords.
4. The returned data includes the status code, payload length, and payload content.


##### install 

requestment.txt join
```
requests
```

##### usage

1. Import module first

   ``` python
   import requests
   ```

2. Then enter the method and url address

   ``` python
   result = requests.request("GET", "http://pikascript.com/")
   ```

3. If everything succeeds, the result will contain the following information

   ``` python
   content_length: int	Returns the length of the text content	
   text: str				The text content returned
   state_code: int		get Indicates the status code of the request	
   headers: str			The response header returned
   url: str				get Indicates the url of the request
   ```

   text is the core returned data.
   For the request in (2), it can be shown as follows:

   ```yacas
   print(result.text)
   ```

   So that's the web page `http://pikascript.com/`

4. If this `request` Failure, result will be an empty object, so you need to determine whether result is empty.

`request` The available parameters of the module are as follows:

``` python
request(method: str, url: str, params=None, headers=None, data=None) -> Response:
```

- `method`, optional `GET`、`POST`，The two most basic operations
- `url`, that is, the standard url field. Note that the length of the field is limited. It is recommended that the field not exceed 2Kb.
- `params`，Optional. Used to concatenate parameters after a given url field. The characters are automatically escaped, or you can concatenate parameters in the url manually.
- `headers`，This parameter is optional. The keyword, such as`Host `, is used to specify the request header. This parameter is optional.
- `data`，Load data used to transmit in `POST`, note that it is of string type.
- `Response`，The returned response object is returned only when the response to the sent request is successful. Otherwise, it is `None`。

##### Concatenate URL

The extra support for the get method is to concatenate urls, which also involves some character conversions. Because there are some special characters in the URL that cannot be displayed directly, they must be escaped.
It is simple to use, as follows:

``` python
result = requests.request("GET", "http://pikascript.com/package", params = {"name":"get-test", "id":"23"})
```

The url is concatenated as follows:

``` python
http://pikascript.com/package?name=get-test&id=23
```
Then use this to send an `http` request. There is no complicated operation here, but simply concatenate the parameters in the dictionary.
If the returned data is displayed below, the result will not be empty until the response is successfully received. Therefore, it is necessary to determine:


``` python
if result not None:
	print(result.status_code)
	print(result.content_length)
	print(result.text)
```

##### post port

This interface is primitive, and if you want to upload the data yourself, you need to manually concatenate the content. Here's why:
1. The http protocol is very complex and there is no need to implement it again.
2. Embedded requirements are fixed.
Here is a typical use:

``` python
import requests
print("test")
form_data = '------WebKitFormBoundaryrEPACvZYkAbE4bYB\r\nContent-Disposition: form-data; name="file"; filename="test_file.txt"\r\nContent-Type: text/plain\r\n\r\nhello, pikascript!\r\n------WebKitFormBoundaryrEPACvZYkAbE4bYB\r\nContent-Disposition: form-data; name="id"\r\n\r\n1670666272201\r\n------WebKitFormBoundaryrEPACvZYkAbE4bYB\r\nContent-Disposition: form-data; name="uploadFileNum"\r\n\r\n1\r\n------WebKitFormBoundaryrEPACvZYkAbE4bYB--\r\n'

print(form_data)
header = {"Content-Type": "multipart/form-data; boundary=----WebKitFormBoundaryrEPACvZYkAbE4bYB"}

a = requests.request("POST", "http://pikascript.com/upload", headers=header, data=form_data)

if a not None:
	print(a.headers)
	print(a.content_length)
	print(a.text)
```

The normal output is as follows:

``` python
HTTP/1.1 200 OK
309
{"files":{"file":[{"fieldName":"file","originalFilename":"test_file.txt","path":"html/upload/pJwNgC8fobWOLN_l9qmAk-Oi.txt","headers":{"content-disposition":"form-data; name=\"file\"; filename=\"test_file.txt\"","content-type":"text/plain"},"size":18}]},"fields":{"id":["1670666272201"],"uploadFileNum":["1"]}}
```

Here's an explanation of the above behavior:

1. First specify additional header keywords, `Content-Type` indicates the type of load, `multipart/form-data` is a common form type, and `boundary` specifies the dividing line between different parts of the form.

   ```
   Content-Type:multipart/form-data; boundary=----WebKitFormBoundaryrEPACvZYkAbE4bYB
   ```

   `----WebKitFormBoundaryrEPACvZYkAbE4bYB` Is used to split the load content, this is simply some random characters, so can be mixed with some identifiers `WebKitFormBoundaryr` Inside.



You will notice that this separator may duplicate the content of the transmission! Yes, it could be repeated, which would cause the server to fail parsing and then have to pass it again. So it's a random string every time, and the probability of repeating it many times in a row is very low.
The above form data is the encoded string, which contains three kinds of data:
1. File name, file type, and file content "hello, pikascript!" .
2. The mapping ID.
3. Number of uploaded files.

The POST key requires the following two request keywords:
   ```yacas
   header buffer:POST http://pikascript.com/upload HTTP/1.1
   Content-Type:multipart/form-data; boundary=----WebKitFormBoundaryrEPACvZYkAbE4bYB
   Content-Length: 408
   ```

For a post, the request header is as simple as the above.
3. The return code is 200, indicating that the post request was successful. Of course, the post response also carries some information about the uploaded content.

**The most direct post transmission, only need the following call can**.  

``` python
a = requests.request("POST", "http://pikascript.com/upload", data=binary_data)
```

This transfers binary data, which is populated by default with the following:

``` python
Content-Type: application/octet-stream
Content-Length: (sizeof(data))
```

But this requires server corresponding analytical support, it is obvious that `http://pikascript.com/upload ` cannot parse the data.

##### Running process
The entire request code was developed based on webclient with simple changes.
When the following statement is run, the following process actually takes place:

``` python
result = requests.request("GET", "http://pikascript.com/package", params = {"name":"get-test"}, headers = {"Connection":"keep-alive"})
```
- First create a session object and apply for a 4Kb buffer to store the request headers.
- Write 'GET' to buffer.
- write "http://pikascript.com/package" into the buffer,
- Writes the concatenated part of the url 'params = {"name":"get-test"}' into buffer, then fills in any other characters as necessary.
- Writes the specified keyword 'headers = {"Connection":"keep-alive"}' to buffer.
- For POST, additional 'Content-Type' and 'Content-Length' contents are written.

- **This will start resolving URL addresses, such as domain names to actual IP addresses**。
- Write the default standard header section keywords, including
  1. `Host: (自行解析)`
  2. `User-Agent: PikaScript HTTP Agent`
  3. `Accept: */*`
- **Create a socket connection and start communication**。
-Send the request header portion before sending the data (for POST).
-Then wait to receive data, this time there is a possibility of timeout.
-Parses the data, writing content_length, text, header, status_code. 

Finally, you can view the above four data through the returned result object.
