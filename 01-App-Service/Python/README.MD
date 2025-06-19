
## OBJETIVO

* Crear una app Flask (`Hola Mundo` en `<h1>` centrado)
* Sin usar `--startup-file`
* Azure debe detectar automáticamente cómo ejecutarla

---

### ⚠️ Requisitos previos:

* Tener instalado **Flask**

```bash
pip install flask
```

---

## 1. Estructura del proyecto compatible con App Service Linux

Tu estructura debe verse así:

```
FlaskApp/
├── app.py           ✅ punto de entrada (contiene `app`)
├── requirements.txt ✅ incluye `flask` y `gunicorn`
```

> Azure detecta automáticamente `gunicorn` y usa `app:app` si `app.py` tiene `app = Flask(...)`.

---

### 📜 `app.py`

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello():
    return '<h1 style="text-align:center;">Hola Mundo</h1>'
```

> ⚠️ **NO** necesitas `if __name__ == '__main__'`, ya que Azure usará `gunicorn app:app` automáticamente.

---

### 📜 `requirements.txt`

```txt
flask
gunicorn
```

> Al detectar `gunicorn`, Azure Linux App Service lanza automáticamente con:
> `gunicorn --bind=0.0.0.0 --timeout 600 app:app`

---

## 2. Crear el ZIP del proyecto

```bash
cd FlaskApp
cd ..
zip -r FlaskApp.zip FlaskApp
```

> En PowerShell:

```powershell
Compress-Archive -Path .\FlaskApp\* -DestinationPath .\FlaskApp.zip
```

---

## 3. Crear y desplegar en Azure (Linux)

### 3.1. Iniciar sesión

```bash
az login
```

---

### 3.2. Desplegar la aplicación (sin startup)

```bash
az webapp deploy \
  --resource-group MiGrupo \
  --name MiFlaskLinuxApp \
  --src-path FlaskApp.zip \
  --type zip
```

---

## ¿Qué pasa si no usas gunicorn?

Azure necesita `gunicorn` para apps Python en Linux. Sin él, fallará el arranque.
**No uses `flask run`**, ya que no es apto para producción.

