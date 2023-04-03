---
layout: default
title: Using APIs
parent: Python
grand_parent: Data science notes
permalink: /docs/data-science-notes/python/using-apis/
nav_order: 6
---

# Using APIs

## Definitions

### JSON

In computing, **JavaScript Object Notation** (**JSON**) is an open-standard file format that uses human-readable text to transmit data objects consisting of attribute–value pairs and array data types. JSON is a language-independent data format. JSON filenames use the extension `.json`. 

The process of encoding JSON is usually called **serialization**. This term refers to the transformation of data into a series of bytes to be stored or transmitted across a network. **Deserialization** is the reciprocal process of decoding data that has been stored or delivered in the JSON standard.

Example: 

```json
{
    "firstName": "Jane",
    "lastName": "Doe",
    "hobbies": ["running", "sky diving", "singing"],
    "age": 35,
    "children": [
        {
            "firstName": "Alice",
            "age": 6
        },
        {
            "firstName": "Bob",
            "age": 8
        }
    ]
}
```

### API

An **application program interface (API)** is a set of methods and tools that allows different applications to interact with each other. Programmers use APIs to query and retrieve data dynamically, for example, when:

* The data changes frequently (e.g. stock prices)
* Only a small piece of information is required (e.g. the geographical coordinates of an address)

Organizations host their APIs on **Web servers**. APIs work much the same way, except instead of your Web browser asking for a Web page, your program asks for data. The API usually returns this data in JSON format (sometimes also XML).

### Rate limiting

Preventing users of an API you from making too many requests in too short a time. It ensures that one user can't overload the API server by making too many requests too fast.

## JSON and Python

Python comes with a built-in package called [`json`](https://docs.python.org/3/library/json.html) for encoding and decoding JSON data.

```python
import json
```

### Serialization

```python
# python_object = lists and dictionaries
# json_string = JSON string
with open("where_to_serialize.json", "w") as json_file:
    json.dump(python_object, json_file) # json.dump() for writing to a file
# OR
json_string = json.dumps(python_object) # json.dumps() for writing to a string
```

### Deserialization

```python
# python_object = lists and dictionaries
# json_string = JSON string
with open("what_to_deserialize.json", "r") as json_file:
    python_object = json.load(json_file) # json.load() for reading a JSON file
# OR
python_object = json.loads(json_string) # json.loads() for reading a JSON string
```

### JSON viewer

Useful online tool for visualizing JSON: http://jsonviewer.stack.hu/

## API and Python

### Authentication

To authenticate with an API, an **access token** is usually required. An access token is a credential in the format of a string that the API can read and associate with an account.

Using a token is preferable to a username and password because: 

* you should not include your username and password in a script
* access tokens can be assigned a scope of specific restrictions.

Tokens are usually included in the `headers` of a request according to the format specified in the API documentation. For example, for GitHub: 

```python
headers = {"Authorization": "token 1f36137fbbe1602f779300dad26e4c1b7fbab631"}
response = requests.get("https://api.github.com/users/nchylak", headers=headers)
```

### GET requests

#### Make a GET request

In order to extract information from an API, you need to find the “get-address” relevant to the data you want to extract, e.g. http://api.open-notify.org/iss-now.json (ends with `.json`), and make a request from it using the `requests` library.

```python
import requests
response = requests.get("http://api.open-notify.org/iss-now.json",
                        params=required_param_according_to_API_doc)
response.status_code # status code of a request
```

#### Status codes

- `200` - Everything went okay, and the server returned a result (if any).
- `301` - The server is redirecting you to a different endpoint. This can happen when a company switches domain names, or an endpoint's name has changed.
- `401` - The server thinks you're not authenticated. This happens when you don't send the right credentials to access an API (we'll talk about this in a later mission).
- `400` - The server thinks you made a bad request. This can happen when you don't send the information the API requires to process your request, among other things.
- `403` - The resource you're trying to access is forbidden; you don't have the right permissions to see it.
- `404` - The server didn't find the resource you tried to access.

#### Content type

The server sends more than a status code and the data when it generates a response. It also sends metadata containing information on how it generated the data and how to decode it. This information appears in the *response headers*. We can access it using the `.headers` property that responses have.

The `content-type` within the headers is the most important key. It tells us the format of the response and how to decode it. 

```python
metadata = response.headers
metadata["content-type"] 
```

#### Pagination

API providers typically implement pagination to the response of API requests. This means that the API provider will only return a certain number of records per page. In order to access all of the pages, a loop is required.

Pages can normally be specified in the `params`, for example as follows: 

```python
params = {"per_page": 50, "page": 2}
```

#### Decode the response

We can get the content of a response as a Python object by using the `.json()` method on the response.

```python
content = response.json()
```

### Make a POST request

We use POST requests to send information and to create objects on the API server. For example, with the GitHub API, we can use POST requests to create new repositories.

```python
payload = {
  "name": "Hello-World",
  "description": "This is your first repository",
  "homepage": "https://github.com",
  "private": false,
  "has_issues": true,
  "has_projects": true,
  "has_wiki": true
}

response = requests.post("https://api.github.com/user/repos", json=payload, headers=headers)
response.status_code # A successful POST request will usually return a 201
```

Not all API endpoints accept POST request - read the documentation!

### Make a PATCH or PUT request

We use PATCH requests when we want to change a few attributes of an object, but don't want to resend the entire object to the server.

We use PUT requests to send the complete object we're revising as a replacement for the server's existing version.

For example, to change the description of the repo created above in GitHub, we would use:

```python
payload = {
    "description": "The best repository ever!",
    "name": "Hello-World"
}

response = requests.patch("https://api.github.com/repos/nchylak/Hello-World", json=payload, headers=headers)
response.status_code # A successful PATCH request will usually return a 200
```

### Make a DELETE request

The DELETE request removes objects from the server. 

For example, to delete a repository from GitHub, we would use:

```python
response = requests.delete("https://api.github.com/repos/nchylak/Hello-World", headers=headers)
status = response.status_code # A successful DELETE request will usually return a 204
```

# 