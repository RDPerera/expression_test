# Expression Editor Testing in WSO2 MI  

## 1. JSON Data Extraction Testing in WSO2 MI

This document outlines the tests performed using the Expression Editor in WSO2 MI for extracting data from a JSON payload. The tests validate object extraction functionality and ensure that variables are correctly retrieved and processed.  

The following JSON payload was used for testing:  

```json
{
  "orderId": "ORD-20250218-12345",
  "customer": {
    "customerId": "CUST-78901",
    "name": "John Doe",
    "email": "john.doe@example.com",
    "phone": "+1-234-567-8901",
    "address": {
      "street": "123 Main St",
      "city": "New York",
      "state": "NY",
      "zip": "10001",
      "country": "USA"
    },
    "preferences": {
      "language": "en-US",
      "currency": "USD",
      "newsletterSubscribed": true
    }
  },
  "items": [
    {
      "productId": "PROD-101",
      "name": "Wireless Headphones",
      "category": "Electronics",
      "price": 99.99,
      "quantity": 2,
      "discount": {
        "type": "percentage",
        "value": 10
      },
      "attributes": {
        "color": "Black",
        "batteryLife": "20 hours",
        "connectivity": "Bluetooth 5.0"
      }
    },
    {
      "productId": "PROD-205",
      "name": "Smartphone",
      "category": "Electronics",
      "price": 799.99,
      "quantity": 1,
      "discount": {
        "type": "fixed",
        "value": 50
      },
      "attributes": {
        "brand": "TechBrand",
        "storage": "128GB",
        "ram": "8GB",
        "color": "Blue"
      }
    }
  ],
  "payment": {
    "method": "Credit Card",
    "transactionId": "TXN-56789",
    "amount": 899.98,
    "currency": "USD",
    "status": "Completed",
    "billingAddress": {
      "street": "123 Main St",
      "city": "New York",
      "state": "NY",
      "zip": "10001",
      "country": "USA"
    }
  },
  "shipping": {
    "method": "Express",
    "trackingId": "TRK-34567",
    "estimatedDelivery": "2025-02-22",
    "carrier": "FastShip",
    "status": "Shipped"
  },
  "metadata": {
    "createdAt": "2025-02-18T14:30:00Z",
    "updatedAt": "2025-02-18T15:00:00Z",
    "notes": [
      "Customer requested gift wrap",
      "Delivery address confirmed"
    ],
    "tags": ["electronics", "vip-customer", "discount-applied"]
  }
}
```

## Test Cases and Results  

### 1. Extracting `orderId` from Payload  

#### Expression Used:  
```xml
<variable name="orderId" type="STRING" expression="${payload.orderId}" />
```

#### Expected Result:  
- The variable `orderId` should store `"ORD-20250218-12345"`.  

#### Actual Result:  
✅ **Success** (Extracted correctly)  

---

### 2. Extracting `customerId` from Payload  

#### Expression Used:  
```xml
<variable name="customerId" type="STRING" expression="${payload.customer.customerId}" />
```

#### Expected Result:  
- The variable `customerId` should store `"CUST-78901"`.  

#### Actual Result:  
✅ **Success** (Extracted correctly)  

---

### 3. Extracting `address` Object from Payload  

#### Expression Used:  
```xml
<variable name="address" type="STRING" expression="${payload.customer.address}" />
```

#### Expected Result:  
- The variable `address` should store:  
  ```json
  {
    "street": "123 Main St",
    "city": "New York",
    "state": "NY",
    "zip": "10001",
    "country": "USA"
  }
  ```

#### Actual Result:  
✅ **Success** (Extracted correctly)  

---

### 4. Generating Response Using `payloadFactory`  

The extracted variables were used in a JSON response:  

```json
{
  "OrderID": "ORD-20250218-12345",
  "CustomerID": "CUST-78901",
  "Address": {
    "street": "123 Main St",
    "city": "New York",
    "state": "NY",
    "zip": "10001",
    "country": "USA"
  }
}
```

#### Expected Result:  
- The response should return the correct values extracted from the payload.  

#### Actual Result:  
✅ **Success** (Response was generated correctly)  

---

## Summary of Results  

| Test Case | Expected Output | Actual Output | Status |
|-----------|----------------|---------------|--------|
| Extract `orderId` | `ORD-20250218-12345` | ✅ Extracted | **PASS** |
| Extract `customerId` | `CUST-78901` | ✅ Extracted | **PASS** |
| Extract `address` object | JSON Object | ✅ Extracted | **PASS** |
| Generate Response | JSON with extracted values | ✅ Success | **PASS** |

---


Here's the documentation for your string manipulation tests, formatted to match the previous section. You can append this to your existing README.  

---

## 2. String Manipulation Testing in WSO2 MI  

This section outlines the tests performed using the Expression Editor in WSO2 MI for string manipulations. The tests validate operations such as concatenation, replacement, case conversion, trimming, word count, and substring search.  


The following JSON payload was used for testing:  

```json
{
  "user": {
    "firstName": "John",
    "lastName": "Doe",
    "email": "john.doe@example.com",
    "username": "john_d_92",
    "bio": "Passionate developer. Love coding, reading, and exploring new technologies.",
    "website": "https://johndoe.dev"
  },
  "messages": [
    {
      "id": 1,
      "content": "Hello, world! This is my first post.",
      "timestamp": "2025-02-18T10:15:30Z"
    },
    {
      "id": 2,
      "content": "JSON is a lightweight data format. It's easy to read and write.",
      "timestamp": "2025-02-18T12:30:45Z"
    },
    {
      "id": 3,
      "content": "Coding tip: Always write clean and maintainable code!",
      "timestamp": "2025-02-18T15:00:00Z"
    }
  ],
  "settings": {
    "theme": "dark-mode",
    "language": "en-US",
    "notifications": {
      "email": true,
      "sms": false,
      "push": true
    }
  },
  "tags": ["developer", "coding", "technology", "json"],
  "randomText": "   Trim this string and remove extra spaces.    ",
  "paragraph": "The quick brown fox jumps over the lazy dog. This sentence contains every letter in the English alphabet."
}
```

## Test Cases and Results  

### 1. Concatenating `firstName` and `lastName`  

#### Expression Used:  
```xml
<variable name="fullname" type="STRING"
    expression="${payload.user.firstName + payload.user.lastName}" />
```

#### Expected Result:  
- The variable `fullname` should store `"JohnDoe"`.  

#### Actual Result:  
✅ **Success** (Concatenated correctly)  

---

### 2. Replacing `_` with `-` in `username`  

#### Expression Used:  
```xml
<variable name="username" type="STRING"
    expression="${replace(payload.user.username,&quot;_&quot;,&quot;-&quot;)}" />
```

#### Expected Result:  
- The variable `username` should store `"john-d-92"`.  

#### Actual Result:  
✅ **Success** (Replaced correctly)  

---

### 3. Converting `username` to Lowercase  

#### Expression Used:  
```xml
<variable name="lowerUsername" type="STRING" expression="${toLower(vars.username)}" />
```

#### Expected Result:  
- The variable `lowerUsername` should store `"john-d-92"`.  

#### Actual Result:  
✅ **Success** (Converted correctly)  

---

### 4. Trimming Whitespace from `randomText`  

#### Expression Used:  
```xml
<variable name="trimmedRandomText" type="STRING"
    expression="${trim(payload.randomText)}" />
```

#### Expected Result:  
- The variable `trimmedRandomText` should store `"Trim this string and remove extra spaces."`.  

#### Actual Result:  
✅ **Success** (Trimmed correctly)  

---

### 5. Counting the Number of Characters in `paragraph`  

#### Expression Used:  
```xml
<variable name="wordcount" type="STRING" expression="${length(payload.paragraph)}" />
```

#### Expected Result:  
- The variable `wordcount` should store `"97"` (character count, including spaces and punctuation).  

#### Actual Result:  
✅ **Success** (Counted correctly)  

---

### 6. Checking if "JSON" Exists in `messages[1].content`  

#### Expression Used:  
```xml
<variable name="JSONPresence" type="STRING"
    expression="${contains(payload.messages[1].content,&quot;JSON&quot;)}" />
```

#### Expected Result:  
- The variable `JSONPresence` should store `"true"`.  

#### Actual Result:  
✅ **Success** (Detected correctly)  

---

## Summary of Results  

| Test Case | Expected Output | Actual Output | Status |
|-----------|----------------|---------------|--------|
| Concatenate `firstName` and `lastName` | `"JohnDoe"` | ✅ Concatenated | **PASS** |
| Replace `_` with `-` in `username` | `"john-d-92"` | ✅ Replaced | **PASS** |
| Convert `username` to lowercase | `"john-d-92"` | ✅ Converted | **PASS** |
| Trim whitespace from `randomText` | `"Trim this string and remove extra spaces."` | ✅ Trimmed | **PASS** |
| Count characters in `paragraph` | `"97"` | ✅ Counted | **PASS** |
| Check for "JSON" in `messages[1].content` | `"true"` | ✅ Detected | **PASS** |


## 3. Integer Operations Testing in WSO2 MI  

This section documents the testing of mathematical operations using the Expression Editor in WSO2 MI. The tests validate operations such as addition, subtraction, division, multiplication, type conversion, and number validation.  

## Test Cases and Results  

### 1. Addition of Two Integers (`A + B`)  

#### Expression Used:  
```xml
<variable name="Addition" type="INTEGER" expression="${vars.A+vars.B}" />
```

#### Expected Result:  
- The variable `Addition` should store `300`.  

#### Actual Result:  
✅ **Success** (Correct addition)  

---

### 2. Subtraction (`B - A`)  

#### Expression Used:  
```xml
<variable name="Substraction" type="INTEGER" expression="${vars.B-vars.A}" />
```

#### Expected Result:  
- The variable `Substraction` should store `100`.  

#### Actual Result:  
✅ **Success** (Correct subtraction)  

---

### 3. Division (`B / A`)  

#### Expression Used:  
```xml
<variable name="Dividance" type="INTEGER" expression="${vars.B/vars.A}" />
```

#### Expected Result:  
- The variable `Dividance` should store `2`.  

#### Actual Result:  
✅ **Success** (Correct division)  

---

### 4. Addition with String Converted to Integer (`A + integer(C)`)  

#### Expression Used:  
```xml
<variable name="AdditionWithString" type="INTEGER"
    expression="${vars.A+integer(vars.C)}" />
```

#### Expected Result:  
- The variable `AdditionWithString` should store `110`.  

#### Actual Result:  
✅ **Success** (Correct conversion and addition)  

---

### 5. Checking if a Variable is a Number (`isNumber(A)`)  

#### Expression Used:  
```xml
<variable name="isNumber" type="STRING" expression="${isNumber(vars.A)}" />
```

#### Expected Result:  
- The variable `isNumber` should store `"true"`.  

#### Actual Result:  
✅ **Success** (Correct type validation)  

---

### 6. Multiplication (`A * B`)  

#### Expression Used:  
```xml
<variable name="Multification" type="STRING" expression="${vars.A*vars.B}" />
```

#### Expected Result:  
- The variable `Multification` should store `"20000"`.  

#### Actual Result:  
✅ **Success** (Correct multiplication)  

---

## Summary of Results  

| Test Case | Expected Output | Actual Output | Status |
|-----------|----------------|---------------|--------|
| Addition (`A + B`) | `300` | ✅ 300 | **PASS** |
| Subtraction (`B - A`) | `100` | ✅ 100 | **PASS** |
| Division (`B / A`) | `2` | ✅ 2 | **PASS** |
| Addition with String (`A + integer(C)`) | `110` | ✅ 110 | **PASS** |
| Check if Number (`isNumber(A)`) | `"true"` | ✅ "true" | **PASS** |
| Multiplication (`A * B`) | `20000` | ✅ 20000 | **PASS** |


Here's the documentation for your date test, formatted in the same structure as the previous ones. You can append this to your existing README.  

---

# 3. Date Format Testing in WSO2 MI  

This section documents the testing of date formatting in the Expression Editor in WSO2 MI. The test validates the ability to get the current date and time and format it in a custom format.  

## Test Case and Result  

### 1. Get Current Date and Time and Format It  

#### Expression Used:  
```xml
<variable name="DateNow" type="STRING"
    expression="${formatDateTime(now(), &quot;yyyy-MM-dd HH:mm:ss&quot;)}" />
```

#### Expected Result:  
- The variable `DateNow` should store the current date and time in the format `yyyy-MM-dd HH:mm:ss`.  

#### Actual Result:  
✅ **Success** (Date correctly formatted and returned)  

---

## Summary of Results  

| Test Case | Expected Output | Actual Output | Status |
|-----------|----------------|---------------|--------|
| Get Current Date and Format | `yyyy-MM-dd HH:mm:ss` | ✅ Current Date and Time | **PASS** |

---

# Conclusion

The tests performed on the Expression Editor in WSO2 MI have been successful, demonstrating the ability to extract data from JSON payloads, manipulate strings, perform mathematical operations, and format dates. The results indicate that the Expression Editor is a powerful tool for working with data and expressions in WSO2 MI 4.4.0 RC 2 with MI VS-Code Extension Pre-Release.
