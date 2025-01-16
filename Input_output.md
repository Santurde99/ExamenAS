
# Guía de Uso de Input y Output en Logstash

Logstash utiliza los bloques **input** y **output** para especificar cómo ingresar datos al pipeline y dónde enviar los datos procesados después de aplicar filtros.

---

## **Bloque Input**
El bloque **`input`** define la fuente de datos para Logstash. Puede leer datos de múltiples orígenes como archivos, sockets, bases de datos, mensajes, y más.

### **Ejemplo de Configuración de Input**
```plaintext
input {
  file {
    path => "/var/log/myapp.log"
    start_position => "beginning"
  }
}
```

**Explicación:**
- **`file`**: Plugin que lee datos de un archivo.
- **`path`**: Ruta al archivo que se leerá.
- **`start_position`**: Define si leer desde el inicio (`beginning`) o desde el final (`end`).

### **Plugins Comunes de Input**
1. **File:** Para leer archivos.
   ```plaintext
   input {
     file {
       path => "/var/log/myapp.log"
     }
   }
   ```
2. **Beats:** Para recibir datos de Filebeat, Metricbeat, etc.
   ```plaintext
   input {
     beats {
       port => 5044
     }
   }
   ```
3. **HTTP:** Para recibir datos a través de peticiones HTTP.
   ```plaintext
   input {
     http {
       port => 8080
     }
   }
   ```
4. **Kafka:** Para consumir mensajes de un clúster Kafka.
   ```plaintext
   input {
     kafka {
       topics => ["logs"]
       bootstrap_servers => "localhost:9092"
     }
   }
   ```

---

## **Bloque Output**
El bloque **`output`** define el destino de los datos procesados. Logstash puede enviar datos a bases de datos, sistemas de mensajería, archivos, y servicios externos.

### **Ejemplo de Configuración de Output**
```plaintext
output {
  elasticsearch {
    hosts => ["http://localhost:9200"]
    index => "myapp_logs"
  }
}
```

**Explicación:**
- **`elasticsearch`**: Plugin que envía datos a un clúster de Elasticsearch.
- **`hosts`**: Dirección del clúster Elasticsearch.
- **`index`**: Nombre del índice donde se almacenarán los datos.

### **Plugins Comunes de Output**
1. **Elasticsearch:** Para enviar datos a un índice de Elasticsearch.
   ```plaintext
   output {
     elasticsearch {
       hosts => ["http://localhost:9200"]
       index => "logs_index"
     }
   }
   ```
2. **File:** Para escribir datos en un archivo.
   ```plaintext
   output {
     file {
       path => "/var/log/output.log"
     }
   }
   ```
3. **Kafka:** Para producir mensajes a un tópico Kafka.
   ```plaintext
   output {
     kafka {
       topic_id => "processed_logs"
       bootstrap_servers => "localhost:9092"
     }
   }
   ```
4. **Stdout (Depuración):** Para imprimir datos en la consola (ideal para pruebas).
   ```plaintext
   output {
     stdout {
       codec => rubydebug
     }
   }
   ```

---

## **Ejemplo Completo de Configuración**
### Descripción del Flujo:
1. Leer datos de un archivo.
2. Filtrar y estructurar los datos.
3. Enviar los datos procesados a Elasticsearch.

```plaintext
input {
  file {
    path => "/var/log/myapp.log"
    start_position => "beginning"
  }
}

filter {
  grok {
    match => { "message" => "%{IP:client_ip} - - \[%{HTTPDATE:timestamp}\] \"%{WORD:method} %{DATA:request} HTTP/%{NUMBER:http_version}\" %{NUMBER:status_code}" }
  }
}

output {
  elasticsearch {
    hosts => ["http://localhost:9200"]
    index => "myapp_logs"
  }
  stdout {
    codec => rubydebug
  }
}
```

---

## **Consejos para Input y Output**
- **Múltiples Entradas o Salidas:** Puedes combinar múltiples entradas y salidas en un solo archivo de configuración.
- **Pruebas con Stdout:** Usa `stdout` con `rubydebug` para depurar y verificar tus datos antes de enviarlos a un destino final.
- **Gestión de Errores:** Configura salidas adicionales para capturar logs en caso de errores.

---

Con esta guía, puedes configurar entradas y salidas en Logstash para manejar flujos de datos de forma eficiente. Si necesitas ejemplos adicionales o ayuda, ¡no dudes en preguntar!
