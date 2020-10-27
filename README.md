# Table of Contents

* [How to install](#How-to-install)
* [How to use](#How-to-use)
* [How to deploy](#How-to-deploy)

## How to install

### 1. Clone project
> *git clone https://git.southtelecom.vn/research-projects/cmnd-validate*

### 2. Install requirements
```sh
$ cd cmnd-validate
$ pip3 install -r requirements.txt
```
```diff
- If an error ModuleNotFoundError: No module named 'skbuild' occur.
- Try to fix it with command:
$ pip3 install scikit-build
```

### 3. Start server

```sh
$ python3 server.py
```
![](https://i.imgur.com/PkuGAgc.png)


## How to use
**Now let's test with two examples:**

> A valid image ``cmnd-test.jpg``
![](https://i.imgur.com/tdoVIIs.jpg)

> An invalid Image was cut 1 corner ``cmnd-test1.jpg``
![](https://i.imgur.com/QYQZ5BB.jpg)


### 1. Test with Postman
#### Request
    
![](https://i.imgur.com/QMhLaKq.png)

    Body: form-data
    Key: Image (with type: File)
    Value: pick a photo to test
    
#### Response
> If image id card is valid. Server will response look like that:
```json
{
    "boxes": [
        [
            14,
            554,
            74,
            611
        ],
        [
            768,
            547,
            823,
            603
        ],
        [
            9,
            75,
            68,
            128
        ],
        [
            766,
            68,
            821,
            119
        ]
    ],
    "labels": [
        "bottom_left",
        "bottom_right",
        "top_left",
        "top_right"
    ],
    "message": "Identify Card was accepted",
    "status": true
}
```
> If image id card is lack of any corner(s). Server will response look like that: 
```json
{
    "boxes": [
        [
            14,
            554,
            74,
            611
        ],
        [
            4,
            74,
            70,
            129
        ],
        [
            766,
            68,
            821,
            119
        ]
    ],
    "labels": [
        "bottom_left",
        "top_left",
        "top_right"
    ],
    "message": "Identify Card was injected. It has been cut 1 corner(s)",
    "status": false
}

```

> If server does not recognize image
```json    
{
    "message": "Not found Identify Card"
}
```

> There are some comments for server response


    boxes: corners recognize (array 2D)
    lables: corners name (array)
    message: message response (string)
    status: accept or not (boolean)


### 2. Test with Web test

> After [start server](#3-Start-server) go to http://localhost:8080/test/ and pick a photo you want to check. After that you will want to click 'Test Image'
![](https://i.imgur.com/6ayV2UH.png)

**Result of this test you can reference to *[this](#Response)***


### 3. Use with API CALL
> if this is a way you choose, you must **encode base 64** this image and **decode uft8**
> Bellow that is a example code use python:
```python=
import base64
import json

import requests

api = 'http://localhost:8080/test'
image_file = 'cmnd-test1.jpg'

with open(image_file, "rb") as f:
    im_bytes = f.read()
im_b64 = base64.b64encode(im_bytes).decode("utf8")

headers = {'Content-type': 'application/json', 'Accept': 'text/plain'}

payload = json.dumps({"image": im_b64)
response = requests.post(api, data=payload, headers=headers)
try:
    data = response.json()
    print(data)
except requests.exceptions.RequestException:
    print(response.text)

```
**Result of this test you can reference to *[this](#Response)***

## Deploy to server
***[updating]***

