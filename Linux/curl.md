```
### 🔸 1. **Basic GET Request**

`curl https://example.com:port`

Fetches the page and prints the response to the terminal.

---

### 🔸 2. **Save Output to a File**

`curl -o filename.html https://example.com`

Saves the output as `filename.html`.

---

### 🔸 3. **Follow Redirects**

`curl -L https://short.url`

Follows HTTP 3xx redirects to the final URL.

---

### 🔸 4. **Show Headers Only**

`curl -I https://example.com`

Sends a HEAD request to view HTTP headers only.

---

### 🔸 5. **Send POST Request**

`curl -X POST -d "username=user&password=pass" https://example.com/login`

Sends form data as POST.

---

### 🔸 6. **Send JSON Data (API Request)**

`curl -X POST -H "Content-Type: application/json" -d '{"key":"value"}' https://api.example.com/data`

---

### 🔸 7. **Add Custom Header**

`curl -H "Authorization: Bearer YOUR_TOKEN" https://api.example.com/data`

---

### 🔸 8. **Download with Progress Bar**

`curl -# -O https://example.com/file.zip`

Adds a progress bar (`-#`) and saves using the original filename (`-O`).

---

### 🔸 9. **Upload a File**

`curl -T file.txt ftp://example.com/uploads/ --user username:password`

Uploads via FTP.

---

### 🔸 10. **Verbose Output for Debugging**

`curl -v https://example.com`

Shows request/response headers and other info.

---
```
