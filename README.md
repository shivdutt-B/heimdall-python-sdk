# 🛡️ heimdall-python-sdk

[![PyPI version](https://img.shields.io/pypi/v/heimdall-python-sdk.svg)](https://pypi.org/project/heimdall-python-sdk/)
[![license](https://img.shields.io/pypi/l/heimdall-python-sdk)](./LICENSE)
[![GitHub issues](https://img.shields.io/github/issues/shivdutt-B/heimdall-python-sdk)](https://github.com/shivdutt-B/heimdall-python-sdk/issues)
[![GitHub stars](https://img.shields.io/github/stars/shivdutt-B/heimdall-python-sdk)](https://github.com/shivdutt-B/heimdall-python-sdk)

> Keep your backend servers **warm and monitored** with a single line of code.

`heimdall-python-sdk` provides a pre-built `/__ping__` endpoint to expose memory usage and uptime metadata for free-hosted platforms like **Render**, **Railway**, **Fly.io**, and more. Designed to integrate easily and safely across Django, FastAPI, and Flask frameworks.

---

## 🚀 Features

- 🔁 Keeps servers warm via automated pinging
- 🔒 Prevents endpoint tampering  
- 📊 Reports memory usage and system metrics
- 🌐 Framework auto-detection (FastAPI, Flask)
- ⚙️ Manual framework specification (Django)
- 🧩 Simple, plug-and-play integration
- 🐍 Pure Python with zero external dependencies

---

## 📦 Installation

```bash
pip install heimdall-python-sdk
```

---

## 🛠️ Usage

### Django

Add the health check endpoint to your Django URL configuration:

```python
from django.urls import path
from heimdall_python_sdk import register_ping

urlpatterns = [
    path("__ping__/", register_ping(framework="django")),
    # your other URL patterns...
]
```

### FastAPI

Register the health check with your FastAPI application:

```python
from fastapi import FastAPI
from heimdall_python_sdk import register_ping

app = FastAPI()
register_ping(app)  # Auto-detects FastAPI

# Your FastAPI routes and logic here...
```

### Flask

Register the health check with your Flask application:

```python
from flask import Flask
from heimdall_python_sdk import register_ping

app = Flask(__name__)
register_ping(app)  # Auto-detects Flask

# Your Flask routes and logic here...

if __name__ == "__main__":
    app.run()
```

---

## 📊 Response Format

The `/__ping__/` endpoint returns system metrics in JSON format:

```json
{
  "status": "healthy",
  "timestamp": "2024-12-13T10:30:00Z",
  "uptime": 3600.5,
  "memory": {
    "used": 45.2,
    "available": 512.0,
    "percent": 8.8
  },
  "framework": "fastapi"
}
```

---

## 🐳 Platform Integration

### Docker Health Check

```dockerfile
FROM python:3.9-slim

COPY . /app
WORKDIR /app
RUN pip install -r requirements.txt

# Use the health check endpoint
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:8000/__ping__/ || exit 1

CMD ["python", "app.py"]
```

### Kubernetes Liveness Probe

```yaml
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: app
    image: your-app:latest
    livenessProbe:
      httpGet:
        path: /__ping__/
        port: 8000
      initialDelaySeconds: 30
      periodSeconds: 10
```

### Render/Railway/Fly.io

Perfect for keeping your free-tier applications warm:

```python
# Your existing Flask/FastAPI/Django app
from heimdall_python_sdk import register_ping

# One line to keep your server monitored
register_ping(app)  # That's it!
```

---

## 🔒 Security

Heimdall is designed with security in mind:

- **Read-only**: The endpoint only exposes system metadata, no sensitive data
- **No tampering**: Users cannot modify the response or inject custom data
- **Rate limiting**: Built-in protection against abuse
- **Framework isolation**: Respects your existing middleware and security settings
- **Minimal footprint**: No external dependencies or security vulnerabilities

---

## 📚 API Reference

### `register_ping(app=None, framework=None)`

Registers a health check endpoint with your web application.

**Parameters:**
- `app` (optional): The application instance (required for FastAPI and Flask)
- `framework` (optional): Framework name for manual specification (required for Django)

**Supported Frameworks:**
- `"django"` - Django web framework  
- Auto-detected: FastAPI, Flask

**Returns:**
- For Django: Returns a URL pattern that can be included in `urlpatterns`
- For FastAPI/Flask: Registers the endpoint directly with the app instance

---

## 🧪 Requirements

- Python 3.7+
- Compatible with Django 3.0+, FastAPI 0.65+, Flask 1.0+
- No external dependencies required

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- Inspired by the need to keep free-tier servers warm
- Named after Heimdall, the Norse god who guards the Bifrost bridge  
- Built with ❤️ for the Python community
- Special thanks to all contributors and maintainers

---

**Keep your servers vigilant with Heimdall! 🛡️**