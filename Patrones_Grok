
# Guía de Patrones Grok en Logstash

Los patrones **Grok** son una poderosa herramienta de Logstash para analizar y estructurar texto no estructurado, como logs de aplicaciones o sistemas.

---

## ¿Qué es un Patrón Grok?
Un patrón **Grok** es una expresión que utiliza palabras clave predefinidas para identificar y extraer partes específicas de un texto. Estos patrones son reutilizables y pueden combinarse para analizar diferentes formatos de texto.

---

## **Sintaxis Básica de Grok**
La sintaxis básica de Grok es:
```plaintext
%{PATTERN_NAME:campo}
```
Donde:
- **`PATTERN_NAME`**: El nombre del patrón predefinido (como `IP`, `NUMBER`, `DATA`, etc.).
- **`campo`**: El nombre del campo donde se almacenará el valor extraído.

---

## **Patrones Grok Comunes**

### 1. **Extracción de Direcciones IP**
**Patrón:**
```plaintext
%{IP:client_ip}
```
**Log de Ejemplo:**
```plaintext
192.168.1.1
```
**Resultado:**
```json
{
  "client_ip": "192.168.1.1"
}
```

---

### 2. **Extracción de Fechas**
**Patrón:**
```plaintext
%{HTTPDATE:timestamp}
```
**Log de Ejemplo:**
```plaintext
12/Jan/2025:10:00:00 +0000
```
**Resultado:**
```json
{
  "timestamp": "12/Jan/2025:10:00:00 +0000"
}
```

---

### 3. **Extracción de Métodos HTTP**
**Patrón:**
```plaintext
%{WORD:method}
```
**Log de Ejemplo:**
```plaintext
GET
```
**Resultado:**
```json
{
  "method": "GET"
}
```

---

### 4. **Extracción de Números**
**Patrón:**
```plaintext
%{NUMBER:status_code}
```
**Log de Ejemplo:**
```plaintext
200
```
**Resultado:**
```json
{
  "status_code": 200
}
```

---

### 5. **Extracción de Mensajes Completos**
**Patrón:**
```plaintext
%{GREEDYDATA:message}
```
**Log de Ejemplo:**
```plaintext
User login successful at 12:00 PM.
```
**Resultado:**
```json
{
  "message": "User login successful at 12:00 PM."
}
```

---

## **Combinación de Patrones**
Los patrones Grok pueden combinarse para analizar logs más complejos.

### Ejemplo: Log de Apache
**Patrón Completo:**
```plaintext
%{IP:client_ip} - - \[%{HTTPDATE:timestamp}\] \"%{WORD:method} %{DATA:request} HTTP/%{NUMBER:http_version}\" %{NUMBER:status_code}
```
**Log de Ejemplo:**
```plaintext
192.168.1.1 - - [12/Jan/2025:10:00:00 +0000] "GET /index.html HTTP/1.1" 200
```
**Resultado:**
```json
{
  "client_ip": "192.168.1.1",
  "timestamp": "12/Jan/2025:10:00:00 +0000",
  "method": "GET",
  "request": "/index.html",
  "http_version": "1.1",
  "status_code": 200
}
```

---

## **Escapando Caracteres Especiales**
En algunos casos, necesitas escapar caracteres especiales (como `[` o `"`) en tu patrón. Usa el carácter de escape `\`.

### Ejemplo:
**Patrón:**
```plaintext
\[%{HTTPDATE:timestamp}\]
```
**Log de Ejemplo:**
```plaintext
[12/Jan/2025:10:00:00 +0000]
```
**Resultado:**
```json
{
  "timestamp": "12/Jan/2025:10:00:00 +0000"
}
```

---

Con esta guía, puedes empezar a usar y crear patrones Grok para estructurar datos de tus logs. Si necesitas ayuda con un caso específico, ¡no dudes en preguntar!
