# Request JS [![Build Status](https://travis-ci.org/philipgiuliani/request-js.svg?branch=master)](https://travis-ci.org/philipgiuliani/request-js)

Request JS is a wrapper around `XMLHttpRequest` which simplifies its usage and makes it more readable.

Get the latest version from [Github Releases](https://github.com/philipgiuliani/request-js/releases).

## Usage
### Making a Request
```javascript
request = new Request({
  url: "/api/v1/users.json", // URL of the request
  method: "POST", // Default is `GET`
  async: false, // Default is `true`
  data: { firstName: "John" }, // Data to submit, default is `null`

  before: function(xhr) { true },
  success: function(response) { true },
  error: function(response) { true },
  complete: function(response) { true }
});

request.send();
```

### Handling the Response
You can attach serval events by using `on` or `addEventListener` to the request.

| Event name | Parameters        | When
|------------|-------------------|---------------------------------
| before     | xhr               | Before the request gets send, but after calling `xhr.open`. If you want to add additional headers, you can modify the `xhr` instance here.
| success    | response          | If the request was success.
| error      | response          | If the server returned an error.
| complete   | response          | At the end of the request.

#### Response
The `response` is an instance of `Response` which contains `xhr`, `data`, `status` and `success`.

The `data` will be JSON if it was able to parse it.

#### Example
```javascript
request.on("success", function(response) {
  console.log("Request was successful.");
  console.log(response.data);
});

request.on("complete", function(response) {
  if (response.success) {
    console.log("Request was successful.");
  }
  else {
    console.log("There was an error. Statuscode: #{response.status}");
  }
});
```

## Request Queue
You can use the `RequestQueue` to enqueue requests and give them priorities. You can configure the amount of simultaneous requests. The default value is 1.

There are currently 4 predefined priorities:
* LOW: 0
* NORMAL: 1
* MEDIUM: 2
* HIGH: 3

You can also use an integer as priority. The default is **NORMAL**.

```javascript
queue = new RequestQueue(2); // 2 simultaneous requests

request = new Request({ url: "/api/v1/users.json" });
job = new RequestJob(request, RequestQueue.MEDIUM);

queue.enqueue(job);
```

### Events
You can attach serval events by using `on` or `addEventListener` to the queue.

| Event name | Parameters        | When
|------------|-------------------|---------------------------------
| enqueue    | job               | When a job gets added to the queue.
| start      | job               | When a job starts.
| finish     | job               | When a job finishs.

### Example
```javascript
queue.on("enqueue", function(job) {
  console.log("A new job has been added!");
});
```

## Contributing
Ensure that you have installed [Node.js](http://www.nodejs.org) and [npm](http://www.npmjs.org/)

Test that Grunt's CLI is installed by running `grunt --version`. If the command isn't found, run `npm install -g grunt-cli`. For more information about installing Grunt, see the [getting started guide](http://gruntjs.com/getting-started).

1. Fork and clone the repository.
2. Run `npm install` to install the dependencies.
3. Run `grunt` to grunt this project.

### Tests
Make sure that all tests are passing before creating a Pull request. You can run the tests with `grunt test`.

The tests are located in `spec/` and written with [Jasmine](http://jasmine.github.io/).
