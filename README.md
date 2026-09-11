# Postman API Testing Collection

A Postman collection demonstrating **API testing and automated response validation** using public REST APIs. The collection covers functional API testing, status-code validation, response headers, response-time checks, response-size validation, data-type verification, and JSON Schema validation using **AJV**.

## 📌 Project Overview

This project demonstrates practical API testing techniques using **Postman** and JavaScript-based test scripts.

### Key Testing Areas

* HTTP method validation
* Status code validation
* Response header validation
* Response time validation
* Response size validation
* Response data-type validation
* Response content/data validation
* JSON Schema validation
* Automated API assertions
* Positive API testing

## 🛠️ Tools & Technologies

* **Postman** – API testing and automation
* **JavaScript** – Writing automated test scripts
* **AJV** – JSON Schema validation
* **REST APIs**
* **JSON**
* **Chai assertions** – Postman assertion syntax

## 📂 Collection Structure

The collection contains the following requests:

| Request            | Method | API             | Testing Focus                         |
| ------------------ | ------ | --------------- | ------------------------------------- |
| GetRequestProducts | GET    | DummyJSON       | Status, headers, response time & size |
| PostRequest        | POST   | JSONPlaceholder | Status code validation                |
| SchemaTest         | GET    | Fake Store API  | JSON Schema validation                |
| DataVerification   | GET    | DummyJSON       | Data type & content validation        |

---

## 1. GetRequestProducts

**Method:** `GET`

**Endpoint:**

`https://dummyjson.com/products`

### Test Validations

The request validates:

* HTTP status code is `200`
* `Content-Type` header exists
* Content-Type is `application/json`
* Response time is below `300 ms`
* Response size is below `50 KB`

### Example Assertions

```javascript
pm.test("Status of the response is 200", () => {
    pm.response.to.have.status(200);
});

pm.test("Presence of the Content-Type header", () => {
    pm.response.to.have.header("content-type");
});

pm.test("Content-Type is application/json", () => {
    pm.expect(
        pm.response.headers.get("content-type")
    ).to.eql("application/json");
});

pm.test("Response time is below 300 ms", () => {
    pm.expect(pm.response.responseTime).to.be.below(300);
});

pm.test("Response size is below 50 KB", () => {
    pm.expect(pm.response.size).to.be.below(50);
});
```

> **Note:** The response-size assertion uses Postman's response-size value as provided by the runtime; the collection's threshold is intended as a lightweight response-size check.

---

## 2. PostRequest

**Method:** `POST`

**Endpoint:**

`https://jsonplaceholder.typicode.com/posts`

### Request Body

```json
{
  "title": "Function Testing",
  "body": "Learning Postman and Database",
  "userId": 2
}
```

### Test Validation

The test verifies that the API successfully creates the resource and returns HTTP `201 Created`.

```javascript
pm.test("Data is created", () => {
    pm.response.to.have.status(201);
});
```

### Testing Concept

This demonstrates basic **POST API functional testing** and HTTP status-code validation.

---

## 3. SchemaTest

**Method:** `GET`

**Endpoint:**

`https://fakestoreapi.com/products`

### Test Objective

The response is validated against a predefined **JSON Schema** using the **AJV (Another JSON Schema Validator)** library.

### Schema Validations

Each product is expected to contain:

* `id` – Integer
* `title` – String
* `price` – Number
* `description` – String
* `category` – String
* `image` – URI
* `rating` – Object

The `rating` object must contain:

* `rate` – Number
* `count` – Integer

### Example

```javascript
var jsonData = pm.response.json();
var Ajv = require('ajv');
var ajv = new Ajv();

pm.test("Schema is verified", () => {
    pm.expect(
        ajv.validate(schema, jsonData)
    ).to.be.true;
});
```

### Testing Concept

This test ensures that the API response follows the expected **structure and data types**, rather than validating only individual values.

---

## 4. DataVerification

**Method:** `GET`

**Endpoint:**

`https://dummyjson.com/users`

### Data-Type Validation

The test verifies that:

* Response is an object
* `users` is an array
* User `id` is a number
* `firstName` is a string

### Data Validation

Specific response values are also validated:

```javascript
pm.expect(jsondata.users[0].id).to.eql(1);
pm.expect(jsondata.users[0].lastName).to.eql("Johnson");
pm.expect(jsondata.users[0].maidenName).to.eql("Smith");
pm.expect(jsondata.users[0].hair.color).to.eql("Brown");
```

### Testing Concept

This demonstrates the difference between:

**Data Type Validation**

Checking whether the response contains the expected data types.

**Data Validation**

Checking whether the returned values match the expected business/test data.

---

# 🧪 Testing Techniques Demonstrated

### Functional Testing

Validates whether API endpoints behave according to expected requirements.

### Status Code Validation

Examples:

* `200 OK` – Successful GET request
* `201 Created` – Successful POST request

### Header Validation

Checks whether required response headers are present and contain expected values.

### Response Time Validation

Ensures the API responds within an expected performance threshold.

### Response Size Validation

Checks that the API response does not exceed the defined size threshold.

### Data Validation

Validates specific fields and expected values in the API response.

### Data Type Validation

Verifies that API fields contain the expected data types.

### JSON Schema Validation

Validates the complete response structure against a predefined JSON Schema using AJV.

---

# ▶️ How to Run

## Prerequisites

Install:

* [Postman](https://www.postman.com/downloads/)
* [Newman](https://learning.postman.com/docs/collections/using-newman-cli/command-line-integration-with-newman/) *(optional, for command-line execution)*

## Import Collection

1. Open Postman.
2. Select **Import**.
3. Import the collection JSON file.
4. Open the imported collection.
5. Run individual requests or the complete collection.
6. Review the results in the **Test Results** section.

---

# 🚀 Run Using Newman

After installing Newman, the collection can be executed from the command line:

```bash
newman run CollectionTest.json
```

Example:

```bash
newman run CollectionTest.json -r cli
```

This allows the Postman tests to be executed as part of automated testing or CI/CD pipelines.

---

# 📊 Sample Test Coverage

| Validation             | Covered |
| ---------------------- | :-----: |
| GET Request            |    ✅    |
| POST Request           |    ✅    |
| HTTP Status Code       |    ✅    |
| Response Headers       |    ✅    |
| Response Time          |    ✅    |
| Response Size          |    ✅    |
| Data Type Validation   |    ✅    |
| Data Value Validation  |    ✅    |
| JSON Schema Validation |    ✅    |
| Automated Assertions   |    ✅    |
| AJV Schema Validation  |    ✅    |
| Newman Execution       |    ✅    |

---

# 🎯 Learning Objectives

This collection was created to demonstrate hands-on knowledge of:

* API functional testing
* Postman test scripting
* JavaScript assertions
* REST API testing
* JSON response validation
* JSON Schema validation
* AJV
* API performance checks
* Automated API test execution

## 👨‍💻 Author

**Arsalan Shaikh**

Senior QA Engineer | API Testing | Manual Testing | Postman | SQL | Functional Testing

---

## 🔗 Postman Collection

The original Postman collection can be accessed here:

[Open Collection in Postman](https://go.postman.co/collection/18441508-1ab22064-bcf6-46cc-8e05-391ab8dc946e?source=collection_link)
