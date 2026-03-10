---
title: "A Beginner's Guide to Using cURL"
description: "An introduction to cURL command, one of the most important tools for any Penetration testers."
date: "2023-07-13"
headerImage: "https://www.booleanworld.com/wp-content/uploads/2018/12/curl-cover-picture.png?ezimgfmt=ng%3Awebp%2Fngcb20%2Frs%3Adevice%2Frscb20-1"
tags: []
---

![HeaderImage](https://www.booleanworld.com/wp-content/uploads/2018/12/curl-cover-picture.png?ezimgfmt=ng%3Awebp%2Fngcb20%2Frs%3Adevice%2Frscb20-1)

cURL is a versatile command-line tool that allows you to interact with various network protocols. It is commonly used for making HTTP requests, testing APIs, and downloading files. In this guide, we will explore some basic cURL commands and their applications.

## Making HTTP Requests

cURL provides a straightforward way to make HTTP requests and retrieve data from web servers. Here are some useful commands to get started:

- **GET Request**: To retrieve the content of a web page or file, you can use the following command:

```bash
curl http://example.com
```

- **Downloading a File**: If you want to download a file and save it locally, you can use the `-O` option to output it into a file with the same name as the remote file:
```bash
curl -O http://example.com/file.txt
```

- **Sending HEAD Request**: If you only need the headers of a response without the actual content, you can use the `-I` option:
```bash
curl -I http://example.com
```

- **Setting User-Agent**: Some websites may require a specific User-Agent header to allow access. You can set the User-Agent using the `-A` option:
```bash
curl -A 'Mozilla/5.0' http://example.com
```

- **HTTP Authentication**: If a website requires authentication, you can include the credentials in the request using the `-u` option:
```bash
curl -u username:password http://example.com
```
Alternatively, you can include the credentials directly in the URL:
```bash
curl http://username:password@example.com
```

- **Authorization Header**: If a website uses token-based authentication, you can include the authorization token in the request headers using the `-H` option:
```bash
curl -H "Authorization: Bearer <token>" http://example.com
```

- **POST Request**: To send data to a server using a POST request, you can use the `-X` option with the `-d` option to specify the data:
```bash
curl -X POST -d 'key1=value1&key2=value2' http://example.com
```

- **Cookies**: To include cookies in your requests, you can use the `-b` option to send cookies from a file or the `-H` option to set the `Cookie` header manually:
```bash
curl -b "PHPSESSID=<session_id>" http://example.com
```
or
```bash
curl -H "Cookie: PHPSESSID=<session_id>" http://example.com
```

- **JSON Requests**: If you need to send JSON data in your requests, you can specify the content type using the `-H` option and provide the JSON data using the `-d` option:
```bash
curl -X POST -d '{"key1":"value1","key2":"value2"}' -H 'Content-Type: application/json' http://example.com
```

## Working with a CRUD API

cURL is also useful for interacting with CRUD (Create, Read, Update, Delete) APIs. Here's how you can perform these operations using cURL:

#### Read
To retrieve data from an API, you can make a GET request with the desired endpoint:
```bash
curl http://example.com/api/city/london
```

You can format the output using tools like `jq` to make it more readable:
```bash
curl http://example.com/api/city/london | jq
```

To view the whole list of cities, simply omit the specific city name in the URL:
```bash
curl http://example.com/api/city/ | jq
```

#### Create
To add data to the API, make a POST request with the desired endpoint and provide the data in JSON format:
```bash
curl -X POST http://example.com/api/city/ -d '{"city_name":"new_city","country_name":"new_country"}' -H 'Content-Type: application/json'
```

#### Update
To update existing data, use the PUT method with the specific endpoint and provide the updated data in JSON format:
```bash
curl -X PUT http://example.com/api/city/london -d '{"city_name":"new_city","country_name":"new_country"}' -H 'Content-Type: application/json'
```

#### Delete
To remove data from the API, make a DELETE request to the specific endpoint:
```bash
curl -X DELETE http://example.com/api/city/new_city
```

cURL is a powerful tool for making HTTP requests, interacting with APIs, and testing network services. By understanding these basic commands, you can begin exploring the vast possibilities that cURL offers.
