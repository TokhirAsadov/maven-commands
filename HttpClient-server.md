# API Documentation

```shell
go to main module directory
mkdir server
cd server/
# paste test.jar
java -jar test.jar
```

---

## Groups API

### 1. Create a New Group
- **Endpoint**: `POST /api/groups`
- **Description**: Yangi guruhni yaratish uchun ishlatiladi.
- **Request**:
  - **Method**: `POST`
  - **Headers**: `Content-Type: application/json`
  - **Body**:
    ```json
    {
      "name": "G62",
      "level": 5
    }
    ```
  - **Required Fields**: `name` (string), `level` (integer)
- **Response**:
  - **Status**: `201 Created`
  - **Body**:
    ```json
    {
      "id": 1,
      "name": "G62",
      "level": 5
    }
    ```
  - **Error**: `400 Bad Request` (agar noto'g'ri ma'lumot kelsa)

### 2. Get All Groups
- **Endpoint**: `GET /api/groups`
- **Description**: Barcha guruhlarni ro'yxat sifatida qaytaradi.
- **Request**:
  - **Method**: `GET`
  - **Headers**: Yo'q
  - **Parameters**: Yo'q
- **Response**:
  - **Status**: `200 OK`
  - **Body**:
    ```json
    [
      {
        "id": 1,
        "name": "G58",
        "level": 5
      },
      {
        "id": 2,
        "name": "G52",
        "level": 10
      }
    ]
    ```
  - **Error**: Yo'q

### 3. Get Group by ID
- **Endpoint**: `GET /api/groups/{id}`
- **Description**: Berilgan ID bo'yicha guruhni qaytaradi.
- **Request**:
  - **Method**: `GET`
  - **Path Variable**: `id` (Long)
  - **Example**: `GET /api/groups/1`
- **Response**:
  - **Status**: `200 OK`
  - **Body**:
    ```json
    {
      "id": 1,
      "name": "G58",
      "level": 5
    }
    ```
  - **Error**: `404 Not Found` (agar ID topilmasa)

### 4. Update Group
- **Endpoint**: `PUT /api/groups/{id}`
- **Description**: Berilgan ID bo'yicha guruhni yangilaydi.
- **Request**:
  - **Method**: `PUT`
  - **Path Variable**: `id` (Long)
  - **Headers**: `Content-Type: application/json`
  - **Body**:
    ```json
    {
      "name": "G58-Updated",
      "level": 6
    }
    ```
  - **Example**: `PUT /api/groups/1`
- **Response**:
  - **Status**: `200 OK`
  - **Body**:
    ```json
    {
      "id": 1,
      "name": "G58-Updated",
      "level": 6
    }
    ```
  - **Error**: `404 Not Found` (agar ID topilmasa)

### 5. Delete Group
- **Endpoint**: `DELETE /api/groups/{id}`
- **Description**: Berilgan ID bo'yicha guruhni o'chiradi.
- **Request**:
  - **Method**: `DELETE`
  - **Path Variable**: `id` (Long)
  - **Example**: `DELETE /api/groups/1`
- **Response**:
  - **Status**: `204 No Content` (muvaffaqiyatli o'chirilganda)
  - **Error**: `404 Not Found` (agar ID topilmasa)

## Students API

### 1. Create a New Student
- **Endpoint**: `POST /api/students`
- **Description**: Yangi talabani yaratish uchun ishlatiladi.
- **Request**:
  - **Method**: `POST`
  - **Headers**: `Content-Type: application/json`
  - **Body**:
    ```json
    {
      "name": "Ali Valiev",
      "age": 20,
      "gpa": 3.26,
      "groupId": 1
    }
    ```
  - **Required Fields**: `name` (string), `age` (integer), `gpa` (double), `groupId` (Long)
- **Response**:
  - **Status**: `201 Created`
  - **Body**:
    ```json
    {
      "id": 1,
      "name": "Ali Valiev",
      "age": 20,
      "gpa": 3.26,
      "group": {
        "id": 1,
        "name": "G58",
        "level": 5
      }
    }
    ```
  - **Error**: 
    - `400 Bad Request` (agar noto'g'ri ma'lumot kelsa)
    - `500 Internal Server Error` (agar `groupId` topilmasa)

### 2. Get All Students
- **Endpoint**: `GET /api/students`
- **Description**: Barcha talabalarni ro'yxat sifatida qaytaradi.
- **Request**:
  - **Method**: `GET`
  - **Headers**: Yo'q
  - **Parameters**: Yo'q
- **Response**:
  - **Status**: `200 OK`
  - **Body**:
    ```json
    [
      {
        "id": 1,
        "name": "Ali Valiev",
        "age": 20,
        "gpa": 3.26,
        "group": {
          "id": 1,
          "name": "G58",
          "level": 5
        }
      }
    ]
    ```
  - **Error**: Yo'q

### 3. Get Student by ID
- **Endpoint**: `GET /api/students/{id}`
- **Description**: Berilgan ID bo'yicha talabani qaytaradi.
- **Request**:
  - **Method**: `GET`
  - **Path Variable**: `id` (Long)
  - **Example**: `GET /api/students/1`
- **Response**:
  - **Status**: `200 OK`
  - **Body**:
    ```json
    {
      "id": 1,
      "name": "Ali Valiev",
      "age": 20,
      "gpa": 3.26,
      "group": {
        "id": 1,
        "name": "G58",
        "level": 5
      }
    }
    ```
  - **Error**: `404 Not Found` (agar ID topilmasa)

### 4. Update Student
- **Endpoint**: `PUT /api/students/{id}`
- **Description**: Berilgan ID bo'yicha talabani yangilaydi.
- **Request**:
  - **Method**: `PUT`
  - **Path Variable**: `id` (Long)
  - **Headers**: `Content-Type: application/json`
  - **Body**:
    ```json
    {
      "name": "Ali Valiev Updated",
      "age": 21,
      "gpa": 3.5,
      "groupId": 1
    }
    ```
  - **Example**: `PUT /api/students/1`
- **Response**:
  - **Status**: `200 OK`
  - **Body**:
    ```json
    {
      "id": 1,
      "name": "Ali Valiev Updated",
      "age": 21,
      "gpa": 3.5,
      "group": {
        "id": 1,
        "name": "G58",
        "level": 5
      }
    }
    ```
  - **Error**: 
    - `404 Not Found` (agar ID topilmasa)
    - `500 Internal Server Error` (agar `groupId` topilmasa)

### 5. Delete Student
- **Endpoint**: `DELETE /api/students/{id}`
- **Description**: Berilgan ID bo'yicha talabani o'chiradi.
- **Request**:
  - **Method**: `DELETE`
  - **Path Variable**: `id` (Long)
  - **Example**: `DELETE /api/students/1`
- **Response**:
  - **Status**: `204 No Content` (muvaffaqiyatli o'chirilganda)
  - **Error**: `404 Not Found` (agar ID topilmasa)
