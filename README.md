# Security

নিচে সঠিক ফাইল নামসহ সবকিছু সুন্দর করে উল্লেখ করা হলো, যাতে আপনি সহজে প্রতিটি ফাইল সেভ করতে পারেন এবং কীভাবে কাজ করবে তা বুঝতে পারেন।

### **📂 ওয়েবসাইট নিরাপত্তা টেস্টিং প্ল্যাটফর্ম ফাইল স্ট্রাকচার**

```
security-testing-platform/
│── app.py                  # Main Backend Server (Flask API)
│── requirements.txt         # Dependencies & Required Packages
│── templates/               # HTML Templates (Frontend)
│   │── index.html           # Home Page
│   │── result.html          # Scan Result Page
│── static/                  # Static Files (CSS, JS, Images)
│   │── style.css            # Custom Stylesheet
│── tools/                   # Security Testing Scripts
│   │── sqlmap_scan.py       # SQL Injection Scanner (SQLMap Integration)
│   │── nmap_scan.py         # Network Port Scanner (Nmap Integration)
│   │── log_monitor.py       # Live Security Log Monitor
│   │── exploit_tester.py    # Manual Exploitation Tool
│── reports/                 # Scan Results & Reports
│── config.py                # Configuration & Settings
│── README.md                # Documentation
```

---

### **📌 ফাইল নাম ও তাদের কাজ:**

#### **1. `app.py`** - **Main Backend Server (Flask API)**

```python
from flask import Flask, render_template, request
import subprocess

app = Flask(__name__)

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/scan', methods=['POST'])
def scan():
    url = request.form['url']
    result = run_scan(url)
    return render_template('result.html', result=result)

def run_scan(url):
    result = subprocess.run(['python', 'tools/sqlmap_scan.py', '--url', url], capture_output=True, text=True)
    return result.stdout

if __name__ == '__main__':
    app.run(debug=True)
```

**ফাইলের নাম**: `app.py`

---

#### **2. `requirements.txt`** - **Required Libraries**

```
Flask
subprocess
requests
nmap
sqlmap
```

**ফাইলের নাম**: `requirements.txt`

---

#### **3. `index.html`** - **Home Page**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Security Testing Platform</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>
    <div class="container">
        <h1>Welcome to Security Testing Platform</h1>
        <form action="{{ url_for('scan') }}" method="POST">
            <label for="url">Enter URL to scan:</label>
            <input type="text" id="url" name="url" required>
            <button type="submit">Scan</button>
        </form>
    </div>
</body>
</html>
```

**ফাইলের নাম**: `index.html`

---

#### **4. `result.html`** - **Scan Result Page**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Scan Results</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>
    <div class="container">
        <h1>Scan Results</h1>
        <pre>{{ result }}</pre>
        <a href="{{ url_for('index') }}">Go Back</a>
    </div>
</body>
</html>
```

**ফাইলের নাম**: `result.html`

---

#### **5. `style.css`** - **Custom Stylesheet**

```css
body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f4;
    color: #333;
}

.container {
    max-width: 800px;
    margin: 50px auto;
    padding: 20px;
    background-color: #fff;
    border-radius: 5px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

h1 {
    color: #2c3e50;
}

form {
    margin-top: 20px;
}

input[type="text"] {
    padding: 10px;
    width: 80%;
    margin-right: 10px;
    border-radius: 5px;
}

button {
    padding: 10px 20px;
    background-color: #3498db;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

button:hover {
    background-color: #2980b9;
}
```

**ফাইলের নাম**: `style.css`

---

#### **6. `sqlmap_scan.py`** - **SQL Injection Scanner (SQLMap Integration)**

```python
import sys
import subprocess

def run_sqlmap(url):
    result = subprocess.run(['sqlmap', '--url', url, '--batch', '--risk=3', '--level=5'], capture_output=True, text=True)
    return result.stdout

if __name__ == '__main__':
    if len(sys.argv) != 3:
        print("Usage: python sqlmap_scan.py --url <target_url>")
        sys.exit(1)
    url = sys.argv[2]
    print(run_sqlmap(url))
```

**ফাইলের নাম**: `sqlmap_scan.py`

---

#### **7. `nmap_scan.py`** - **Network Port Scanner (Nmap Integration)**

```python
import nmap

def scan_ports(target):
    nm = nmap.PortScanner()
    nm.scan(target, '22-1024')
    return nm.all_hosts()

if __name__ == '__main__':
    target = input("Enter target IP or URL to scan: ")
    hosts = scan_ports(target)
    for host in hosts:
        print(f"Host: {host}, State: {nm[host].state()}")
```

**ফাইলের নাম**: `nmap_scan.py`

---

#### **8. `log_monitor.py`** - **Live Security Log Monitor**

```python
import time

log_file = '/path/to/your/logfile.log'

def monitor_logs():
    with open(log_file, 'r') as file:
        while True:
            line = file.readline()
            if not line:
                time.sleep(0.1)
                continue
            if 'error' in line.lower():
                print(f"Security issue detected: {line}")

if __name__ == '__main__':
    monitor_logs()
```

**ফাইলের নাম**: `log_monitor.py`

---

#### **9. `exploit_tester.py`** - **Manual Exploitation Tool**

```python
import requests

def exploit_test(url):
    payload = {"username": "' OR 1=1 --", "password": "password"}
    response = requests.post(url, data=payload)
    if "Welcome" in response.text:
        print("Potential vulnerability found!")
    else:
        print("No vulnerability detected.")

if __name__ == '__main__':
    url = input("Enter the URL to test: ")
    exploit_test(url)
```

**ফাইলের নাম**: `exploit_tester.py`

---

#### **10. `config.py`** - **Configuration & Settings**

```python
# Configurations for the app
SQLMAP_PATH = "/usr/local/bin/sqlmap"
NMAP_PATH = "/usr/bin/nmap"
LOG_FILE_PATH = "/var/log/system.log"
```

**ফাইলের নাম**: `config.py`

---

এভাবে আপনি প্রতিটি ফাইলকে সঠিক নাম সহ সেভ করতে পারবেন। এটি একটি সম্পূর্ণ ওয়েবসাইট নিরাপত্তা টেস্টিং প্ল্যাটফর্ম যেখানে আপনি ওয়েবসাইটের নিরাপত্তা পরীক্ষা করতে পারবেন এবং ফলাফল দেখতে পারবেন।
