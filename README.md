API mocker [![Build Status](https://travis-ci.com/forceedge01/api-mocker.svg?branch=master)](https://travis-ci.com/forceedge01/api-mocker) [![Latest Stable Version](https://poser.pugx.org/genesis/mock-api/v)](//packagist.org/packages/genesis/mock-api) [![License](https://poser.pugx.org/genesis/mock-api/license)](//packagist.org/packages/genesis/mock-api) [![Latest Unstable Version](https://poser.pugx.org/genesis/mock-api/v/unstable)](//packagist.org/packages/genesis/mock-api)
------

Have an application that depends on an API but is too much of a burden on testing? Replace your API with a mock API using this package and mock requests on the fly.

How to install
-------

```
composer require genesis/mock-api
make -f ./vendor/genesis/mock-api/Makefile build
make mockapi-install
```

Start mock-api:

```
make mockapi-up
```

Test that the mock API is running:

```
curl http://0.0.0.0:8989/alive
```

Stop mock-api:

```
make mockapi-down
```

Find more commands in the generated Makefile.

Static mocks
------

You can add your static routes (ones that will be available as soon as you boot up the mock API) in the staticMocks folder. The data format is defined below and example request already exists.

Dynamic mocks
------

To mock a request `POST` your mocking request to `/mocks` with `mockData` json. Example mock request:

```json
# POST /mocks
{
    "mockData": {
        "url": "/user/abc123",
        "get": [{
            "body": {
                "id": "abc123",
                "name": "Wahab Qureshi"
            }
        }]
     }
}
```

You can override a static mock with a dynamic one. Purging the mocks will revert the back to the static mock.

Full mock data options:

```json
{
    "mockData": {
        "url": "/user/abc123",
        "get": [{
            "with": "/abc123/",
            "response_code": 301,
            "headers": {"lola": "123", "baby boo": "dudu"},
            "body": {
                "id": "abc123",
                "name": "Wahab Qureshi"
            },
            "proxy": {
                "url": "http://google.com",
                "headers": {
                    "app-token": "88374783847"
                }
            },
            "consecutive_responses": [{
                "response_code": 205,
                "body": {
                    "id": "abc123",
                    "name": "Wahab Qureshi"
                }
            }, {
                "response_code": 500,
                "body": "internal server error"
            }]
        }]
     }
}
```

`mockData (object)`: Contains mock request information.

`mockData.url (string)`: The URL to mock, can be an existing statically mocked URL.

`mockData.<METHOD> ([]object)`: The method to mock for the URL.

`mockData.<METHOD>.with (?string)`: A regex pattern to be applied to the URL optionally.

`mockData.<METHOD>.response_code (?int)`: The response code to return optionally.

`mockData.<METHOD>.headers (?object)`: The headers to return.

`mockData.<METHOD>.body (mixed)`: The response content.

`mockData.<METHOD>.consecutive_responses (?[]object)`: On consecutive calls return one after the other. Supports response_code, headers and body.
    
`mockData.<METHOD>.proxy (?object)`: Proxy the response through another URL.

View mocked endpoints
------

View existing mock requests by visiting `/mocks`

Purge dynamic mocks
-----

To purge all dynamic mocks created send `GET` request to `/mocks?purge=true`

Development:
-------

Running tests is just a case of running the behat tests:

```
./vendor/bin/behat
```