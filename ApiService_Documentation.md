
# Documentation: Comprehensive API Service and Internet Connectivity Manager

---

## 1. Introduction

This project is a complete solution for managing HTTP requests using the `Dio` package in Dart. It offers an easy-to-use and flexible interface for interacting with RESTful APIs, featuring:

- **Internet connectivity management.**
- **Centralized response handling via the `ApiResponse` class.**
- **Unified error handling.**
- **Support for essential operations: GET, POST, PUT, DELETE.**

---

## 2. Overall Structure

### 2.1 `ApiService`
The main class for managing HTTP requests. It provides methods such as `get`, `post`, `put`, and `delete`.

### 2.2 `InternetConnectivityService`
A dedicated class for live internet connectivity management, broadcasting connection status in real time.

### 2.3 `ApiResponse`
A class encapsulating all response details, enabling easy access to data and information.

---

## 3. Key Components

### 3.1 `ApiService`

#### Description:
`ApiService` is responsible for sending HTTP requests, handling responses, and managing errors. It checks internet connectivity before sending any request.

#### Variables:

| Variable               | Type                           | Description                                      |
|------------------------|--------------------------------|------------------------------------------------|
| `_dio`                 | `Dio`                         | Dio instance for managing requests.            |
| `connectivityService`  | `InternetConnectivityService`  | Service for managing internet connectivity.    |

#### Available Methods:

| Method     | Return Type         | Description                                                             |
|------------|---------------------|-------------------------------------------------------------------------|
| `get`      | `Future<ApiResponse>` | Sends a GET request and handles the response.                          |
| `post`     | `Future<ApiResponse>` | Sends a POST request, supporting data and file uploads.                |
| `put`      | `Future<ApiResponse>` | Sends a PUT request, supporting data (no file uploads).                |
| `delete`   | `Future<ApiResponse>` | Sends a DELETE request, optionally with data.                          |

#### Usage:

```dart
final apiService = ApiService(
  baseUrl: 'https://example.com/api',
  connectivityService: connectivityService,
);
```

#### Examples:

**GET Request:**

```dart
final response = await apiService.get('/users', params: {'page': 1});
if (response.isSuccess) {
  print("Data: ${response.data}");
} else {
  print("Error: ${response.message}");
}
```

**POST Request:**

```dart
final response = await apiService.post('/users', data: {'name': 'John Doe'});
if (response.isSuccess) {
  print("User created: ${response.data}");
} else {
  print("Error: ${response.message}");
}
```

---

### 3.2 `InternetConnectivityService`

#### Description:
A class for managing internet connectivity status with live broadcasting. It supports WiFi, Mobile, and Ethernet networks.

#### Variables:

| Variable                     | Type                   | Description                                 |
|------------------------------|------------------------|---------------------------------------------|
| `_connectivity`              | `Connectivity`         | Instance of `connectivity_plus`.            |
| `_connectivityStreamController` | `StreamController<bool>` | Live broadcasting of connectivity status.  |
| `_isConnected`               | `bool`                | Current connection status.                  |

#### Available Methods:

| Method                | Return Type      | Description                                                       |
|-----------------------|------------------|-------------------------------------------------------------------|
| `checkConnection`     | `Future<bool>`   | Manually checks for internet connectivity.                       |
| `onConnectivityChanged` | `Stream<bool>` | Live broadcasting of connectivity status (`true` for connected). |
| `dispose`             | `void`          | Cleans up the `StreamController` when finished.                  |

#### Usage:

```dart
final connectivityService = InternetConnectivityService();

connectivityService.onConnectivityChanged.listen((isConnected) {
  if (isConnected) {
    print("Connected to the Internet");
  } else {
    print("No Internet Connection");
  }
});
```

#### Examples:

**Manual Connectivity Check:**

```dart
final isConnected = await connectivityService.checkConnection();
if (!isConnected) {
  print("No Internet Connection");
} else {
  print("Connected to the Internet");
}
```

---

### 3.3 `ApiResponse`

#### Description:
A class dedicated to encapsulating server responses, making it easy to access data and related information.

#### Variables:

| Variable      | Type                  | Description                                     |
|---------------|-----------------------|------------------------------------------------|
| `success`     | `bool`                | The status of the request (`true` for success).|
| `statusCode`  | `int?`                | HTTP status code (e.g., 200, 404).             |
| `url`         | `String?`             | The URL of the request.                        |
| `data`        | `dynamic`             | The data returned by the server.               |
| `params`      | `Map<String, dynamic>?` | Parameters sent with the request (GET).       |
| `body`        | `dynamic`             | Data sent with the request (POST, PUT).        |
| `message`     | `String?`             | Error or success message.                      |

#### Methods:

| Method       | Return Type | Description                                      |
|--------------|-------------|--------------------------------------------------|
| `isSuccess`  | `bool`      | Checks if the response is successful (`true`).   |
| `toString`   | `String`    | Converts the response to a detailed string.      |

#### Usage:

```dart
final response = ApiResponse(
  success: true,
  statusCode: 200,
  url: "https://example.com/api/users",
  data: {"id": 1, "name": "John Doe"},
);

print(response.data); // {"id": 1, "name": "John Doe"}
```

---

## 4. Key Features

1. **Comprehensive Response Management:**
   - All response details are encapsulated in the `ApiResponse` class.

2. **Centralized Error Handling:**
   - Errors are handled within `ApiService` and returned as part of `ApiResponse`.

3. **Internet Connectivity Validation:**
   - Checks connectivity before sending requests.
   - Live broadcasting of connection status.

4. **Support for Essential Operations:**
   - GET, POST, PUT, DELETE with flexible parameter and data handling.

5. **Extensible Design:**
   - Easily add new methods or features.

---

## 5. Advanced Examples

**1. Live Internet Connectivity Monitoring:**

```dart
connectivityService.onConnectivityChanged.listen((isConnected) {
  if (isConnected) {
    customSnackBar("Connected to the Internet", SnackBarType.correct);
  } else {
    customSnackBar("No Internet Connection", SnackBarType.error);
  }
});
```

**2. Error Management with `ApiResponse`:**

```dart
final response = await apiService.get('/endpoint');
if (!response.isSuccess) {
  customSnackBar("Error: ${response.message}", SnackBarType.error);
} else {
  print("Data: ${response.data}");
}
```

---

## 6. Future Enhancements

- **Authentication Support:**
  - Add methods for managing tokens with requests.

- **Error Logging:**
  - Save errors to a file or send them to a monitoring server.

- **Advanced File Uploading:**
  - Support for large file uploads (chunked upload).

---

## 7. Conclusion

This code is designed to be a flexible and scalable system for managing HTTP requests, internet connectivity, and server responses. It simplifies API interactions while ensuring robust error handling and monitoring.

Feel free to use or enhance this system for your specific use cases! 🚀
