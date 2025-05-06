# 🧪 Lab: SQL injection UNION attack, finding a column containing text

## 🎯 Objective
This lab demonstrates how to identify which column in a SQL `UNION SELECT` query is compatible with string data. The goal is to make a provided random string (in this case, **`ok0OOq`**) appear in the application’s response.

---

## 🧭 Steps

### 1️⃣ Access the Lab

Open the lab in your browser and take note of the random string to be retrieved.

📸 `1-access-lab.png`

---

### 2️⃣ Intercept the Product Category Request

Using Burp Suite, click any category to capture the request. Send the intercepted request to Repeater.

📸 `2-click-category.png`

---

### 3️⃣ Determine the Number of Columns

Send the following payload to determine how many columns the original query returns:

```
'+UNION+SELECT+NULL,NULL,NULL--
```

If no error is returned and the page loads, it confirms the query has **3 columns**.

📸 `3-check-column-count.png`

---

### 4️⃣ Test Each Column for Text Compatibility

Inject the provided string `ok0OOq` into each column one by one:

- `'+UNION+SELECT+'ok0OOq',NULL,NULL--`
- `'+UNION+SELECT+NULL,'ok0OOq',NULL--`
- `'+UNION+SELECT+NULL,NULL,'ok0OOq'--`

The one that displays the string in the HTML output identifies the string-compatible column.

✅ In this lab, the **second column** accepts strings.

📸 `4-string-column-found.png`

---

### 5️⃣ Lab Solved

Once the string is visible in the page output, and the lab acknowledges it, the message **“Congratulations, you solved the lab!”** will appear.

📸 `5-lab-solved.png`

---

## ✅ Conclusion

This lab builds on the previous exercise by combining column count detection with string injection. It's a vital step before retrieving meaningful data like usernames or emails in UNION-based SQLi scenarios.
