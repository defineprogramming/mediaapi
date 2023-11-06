# Media Hosting/Upload API
View the Hosted Web Interface [here](https://hosting.definescripting.repl.co).

## Reference
This API allows you to upload, download, delete, and extend the expiry of files. It also provides file information and a list of all files on the server.

## Base URL
```
https://hosting.definescripting.repl.co
```

## Endpoints

### POST /upload
Upload a file to the server. Files can be up to 16 MB in size and must be of the following types: png, jpg, jpeg, gif, mp3, wav, mp4, mov, pdf, txt, doc, docx, xls, xlsx.

#### Request

```
POST /upload HTTP/1.1
Host: hosting.definescripting.repl.co
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="file"; filename="example.jpg"
Content-Type: image/jpeg

(file data)
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="visibility"

Private
------WebKitFormBoundary7MA4YWxkTrZu0gW--
```

#### Response

```
HTTP/1.1 200 OK
Content-Type: application/json

{
  "filename": "3b1f4b56-4e7d-4e2a-8c3a-8e4a7e2ed3b1_example.jpg",
  "token": "d4f24d5b-2baf-4d40-a3a6-19fd0e5c8e5c",
  "visibility": "Private"
}
```

### GET /uploads/{filename}
View a file. If the file is private, you must provide your token as a query parameter.

#### Request

```
GET /uploads/3b1f4b56-4e7d-4e2a-8c3a-8e4a7e2ed3b1_example.jpg?token=d4f24d5b-2baf-4d40-a3a6-19fd0e5c8e5c HTTP/1.1
Host: hosting.definescripting.repl.co
```

#### Response

```
HTTP/1.1 200 OK
Content-Type: image/jpeg

(file data)
```

### GET /download/{filename}
Download a file. If the file is private, you must provide your token as a query parameter.

#### Request

```
GET /download/3b1f4b56-4e7d-4e2a-8c3a-8e4a7e2ed3b1_example.jpg?token=d4f24d5b-2baf-4d40-a3a6-19fd0e5c8e5c HTTP/1.1
Host: hosting.definescripting.repl.co
```

#### Response

```
HTTP/1.1 200 OK
Content-Type: image/jpeg
Content-Disposition: attachment; filename=3b1f4b56-4e7d-4e2a-8c3a-8e4a7e2ed3b1_example.jpg

(file data)
```

### DELETE /delete/{filename}
Delete a file. You must provide your token in the `X-Auth-Token` header.

#### Request

```
DELETE /delete/3b1f4b56-4e7d-4e2a-8c3a-8e4a7e2ed3b1_example.jpg HTTP/1.1
Host: hosting.definescripting.repl.co
X-Auth-Token: d4f24d5b-2baf-4d40-a3a6-19fd0e5c8e5c
```

#### Response

```
HTTP/1.1 200 OK
Content-Type: application/json

{
  "message": "3b1f4b56-4e7d-4e2a-8c3a-8e4a7e2ed3b1_example.jpg deleted."
}
```

### POST /extend/{filename}
Extend the expiry of a file by 1 day. You must provide your token in the `X-Auth-Token` header.

#### Request

```
POST /extend/3b1f4b56-4e7d-4e2a-8c3a-8e4a7e2ed3b1_example.jpg HTTP/1.1
Host: hosting.definescripting.repl.co
X-Auth-Token: d4f24d5b-2baf-4d40-a3a6-19fd0e5c8e5c
```

#### Response

```
HTTP/1.1 200 OK
Content-Type: application/json

{
  "message": "3b1f4b56-4e7d-4e2a-8c3a-8e4a7e2ed3b1_example.jpg expiry extended."
}
```

### GET /info/{filename}
Get information about a file. You must provide your token in the `X-Auth-Token` header.

#### Request

```
GET /info/3b1f4b56-4e7d-4e2a-8c3a-8e4a7e2ed3b1_example.jpg HTTP/1.1
Host: hosting.definescripting.repl.co
X-Auth-Token: d4f24d5b-2baf-4d40-a3a6-19fd0e5c8e5c
```

#### Response

```
HTTP/1.1 200 OK
Content-Type: application/json

{
  "filename": "3b1f4b56-4e7d-4e2a-8c3a-8e4a7e2ed3b1_example.jpg",
  "expiry_date": "2023-06-18T10:12:08.123456",
  "extend_count": 1,
  "file_size": 123456,
  "content_type": "image/jpeg"
}
```

### GET /list
Get a list of all public files on the server.

#### Request

```
GET /list HTTP/1.1
Host: hosting.definescripting.repl.co
```

#### Response

```
HTTP/1.1 200 OK
Content-Type: application/json

[
  "3b1f4b56-4e7d-4e2a-8c3a-8e4a7e2ed3b1_example.jpg",
  "4d5e6f70-1a2b-3c4d-5e6f-7a1b2c3d4e5f_example2.jpg"
]
```

### GET /purge
Generate a purge code. This code can be used to delete all files on the server within 5 minutes.

#### Request

```
GET /purge HTTP/1.1
Host: hosting.definescripting.repl.co
```

#### Response

```
HTTP/1.1 200 OK
```

### DELETE /purge/{code}
Delete all files on the server using a purge code.

#### Request

```
DELETE /purge/6a7b8c9d-0e1f-2a3b-4c5d-6e7f8a9b0c1d HTTP/1.1
Host: hosting.definescripting.repl.co
```

#### Response

```
HTTP/1.1 200 OK
```

### GET /health
Check the health of the server.

#### Request

```
GET /health HTTP/1.1
Host: hosting.definescripting.repl.co
```

#### Response

```
HTTP/1.1 200 OK
```

## Error Codes

- 400: The server could not understand the request due to invalid syntax.
- 403: You do not have the necessary permissions to perform the requested operation.
- 404: The requested file was not found on the server.
- 410: The requested file is no longer available on the server and there is no forwarding address.
- 413: File size exceeds the 16 MB limit.
- 500: The server encountered an internal error and was unable to complete your request.
