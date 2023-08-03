# REST(ful) API

- API: Application Programming Interface, a way that two pieces of software (that aren't built the same way/language etc.) can communicate with each other.
- A `client` consumes information given by the `server`.
- **JSON (Javascript Object Notation)** is the standard for APIs. It is the `language` for communication.
- **REST** is the `means` of communication, which is `over the Internet`. Similarly as requesting a website but instead of html you request in json.

## API Methods
- **GET**: Used to retrieve data from the server.
- **POST**: Write **new** data to the server. Used to `add` a resource. e.g. `POST /drinks` (don't know the id yet since the id is auto-incremented in the database), each time we add an object, there will be a new id assigned.
- **PUT**: Write **updated** data to the server. Used to `replace` a resource. e.g. `PUT /drinks/605`, each time we update, the object will remain the same id.
- **DELETE**: delete junk.
- **PATCH**: update different pieces of a resource (e.g. update one field), usually work with very large data.

## Python script to consume an API
Go to api.stackexchange.com/docs to see the documentation of the stackoverflow's apis
```
import requests
import json

response = requests.get("http://api/stackexchange.com/2.2/questions?order=desc&sort=activity&site=stackoverflow")
print(response.json())

for data in response.json()['items']:
    if data['answer_count'] == 0:
      print(data['title'])
      print(data['link'])
```


# Source
- [REST API Crash Course](https://www.youtube.com/watch?v=qbLc5a9jdXo)
