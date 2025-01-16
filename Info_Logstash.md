https://www.elastic.co/guide/en/logstash/current/input-plugins.html
https://www.elastic.co/guide/en/logstash/current/filter-plugins.html
https://www.elastic.co/guide/en/logstash/current/output-plugins.html

# Ejemplos de Filtros en Logstash

A continuación, se presentan ejemplos de uso para varios filtros de Logstash. Cada ejemplo incluye:
1. **Configuración del Filtro.**
2. **Log de Entrada.**
3. **Resultado del Log después del Filtro.**

---

## 1. **Grok**
**Configuración del Filtro:**
```plaintext
filter {
  grok {
    match => { "message" => "%{IP:client_ip} - - \[%{HTTPDATE:timestamp}\] \"%{WORD:method} %{DATA:request} HTTP/%{NUMBER:http_version}\" %{NUMBER:status_code}" }
  }
}
```

**Log de Entrada:**
```plaintext
192.168.1.1 - - [12/Jan/2025:10:00:00 +0000] "GET /index.html HTTP/1.1" 200
```

**Resultado del Log:**
```json
{
  "client_ip": "192.168.1.1",
  "timestamp": "12/Jan/2025:10:00:00 +0000",
  "method": "GET",
  "request": "/index.html",
  "http_version": "1.1",
  "status_code": "200"
}
```

---

## 2. **Date**
**Configuración del Filtro:**
```plaintext
filter {
  date {
    match => ["timestamp", "dd/MMM/yyyy:HH:mm:ss Z"]
    target => "@timestamp"
  }
}
```

**Log de Entrada:**
```json
{
  "timestamp": "12/Jan/2025:10:00:00 +0000"
}
```

**Resultado del Log:**
```json
{
  "@timestamp": "2025-01-12T10:00:00.000Z"
}
```

---

## 3. **Mutate**
**Configuración del Filtro:**
```plaintext
filter {
  mutate {
    rename => { "host" => "server_name" }
    convert => { "status_code" => "integer" }
    add_field => { "log_type" => "webserver" }
    remove_field => ["unnecessary_field"]
  }
}
```

**Log de Entrada:**
```json
{
  "host": "web01",
  "status_code": "200",
  "unnecessary_field": "to_remove"
}
```

**Resultado del Log:**
```json
{
  "server_name": "web01",
  "status_code": 200,
  "log_type": "webserver"
}
```

---

## 4. **Split**
**Configuración del Filtro:**
```plaintext
filter {
  split {
    field => "items"
  }
}
```

**Log de Entrada:**
```json
{
  "items": ["item1", "item2", "item3"]
}
```

**Resultado del Log:**
```json
{
  "items": "item1"
}
``` 
```json
{
  "items": "item2"
}
```
```json
{
  "items": "item3"
}
```

---

## 5. **GeoIP**
**Configuración del Filtro:**
```plaintext
filter {
  geoip {
    source => "client_ip"
    target => "geoip"
  }
}
```

**Log de Entrada:**
```json
{
  "client_ip": "8.8.8.8"
}
```

**Resultado del Log:**
```json
{
  "geoip": {
    "ip": "8.8.8.8",
    "country_name": "United States",
    "region_name": "California",
    "city_name": "Mountain View",
    "latitude": 37.386,
    "longitude": -122.0838
  }
}
```

---

## 6. **Translate**
**Configuración del Filtro:**
```plaintext
filter {
  translate {
    field => "status_code"
    dictionary => { "200" => "OK", "404" => "Not Found" }
    fallback => "Unknown"
  }
}
```

**Log de Entrada:**
```json
{
  "status_code": "404"
}
```

**Resultado del Log:**
```json
{
  "status_code": "Not Found"
}
```

---

## 7. **CIDR**
**Configuración del Filtro:**
```plaintext
filter {
  cidr {
    address => ["%{client_ip}"]
    network => ["192.168.1.0/24", "10.0.0.0/8"]
    add_tag => ["internal"]
  }
}
```

**Log de Entrada:**
```json
{
  "client_ip": "192.168.1.5"
}
```

**Resultado del Log:**
```json
{
  "client_ip": "192.168.1.5",
  "tags": ["internal"]
}
```

---

Estos ejemplos muestran cómo configurar y aplicar diferentes filtros en Logstash. Si necesitas más detalles o filtros adicionales, no dudes en preguntar.

