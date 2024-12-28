![Amarillo y Naranja Simple Limpio Digital Adivina la Imagen con Zoom Juego Presentación Divertida (1)](https://github.com/user-attachments/assets/b44a65f0-8742-4cfe-8fb2-426db97f0162)


# Anotaciones de Spring Boot

En este archivo encontrarás una explicación detallada de las anotaciones más comunes utilizadas en Spring Boot, acompañadas de ejemplos prácticos para entender cómo funcionan.

---

## **1. @SpringBootApplication**

### Descripción:
- Marca la clase principal de tu aplicación Spring Boot.
- Combina tres anotaciones:
  - `@Configuration`: Indica que la clase tiene configuraciones de Spring.
  - `@EnableAutoConfiguration`: Habilita la configuración automática.
  - `@ComponentScan`: Escanea los componentes (beans) dentro del paquete.

### Ejemplo:
```java
@SpringBootApplication
public class MiAplicacion {
    public static void main(String[] args) {
        SpringApplication.run(MiAplicacion.class, args);
    }
}
```
---

## **2. @Entity**

### Descripción:
- Define una clase como una entidad JPA (Java Persistence API).
- Cada instancia de esta clase se mapea a una fila de la base de datos.

### Ejemplo:
```java
@Entity
public class Persona {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String nombre;
    private int edad;

    // Getters y setters
}
```
> **Nota:** En este caso, `Persona` se mapeará a una tabla llamada `persona` en la base de datos.

---

## **3. @Id y @GeneratedValue**

### Descripción:
- `@Id`: Marca un campo como clave primaria.
- `@GeneratedValue`: Indica que el valor de la clave primaria será generado automáticamente.
  - `GenerationType.IDENTITY`: Usa la estrategia de generación de claves basada en la base de datos.

### Ejemplo:
```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
```
> **Nota:** La base de datos será responsable de generar el ID único.

---

## **4. @Repository**

### Descripción:
- Marca una clase como un repositorio, que es un mecanismo para acceder a los datos.
- Es una especialización de `@Component`.

### Ejemplo:
```java
@Repository
public interface PersonaRepository extends JpaRepository<Persona, Long> {
}
```
> **Nota:** No necesitas implementar métodos básicos como `save` o `findAll`, ya que JpaRepository los proporciona automáticamente.

---

## **5. @RestController**

### Descripción:
- Combina `@Controller` y `@ResponseBody`.
- Marca una clase como un controlador REST que devuelve respuestas directamente como JSON o XML.

### Ejemplo:
```java
@RestController
@RequestMapping("/api/personas")
public class PersonaController {
    @GetMapping
    public List<Persona> listarPersonas() {
        return personaRepository.findAll();
    }
}
```

---

## **6. @RequestMapping**

### Descripción:
- Define una ruta base para todos los endpoints de una clase o un método.

### Ejemplo:
```java
@RequestMapping("/api/personas")
```
- Todos los endpoints de esta clase estarán bajo `/api/personas`.

---

## **7. @GetMapping, @PostMapping, @PutMapping, @DeleteMapping**

### Descripción:
- Definen rutas específicas para métodos HTTP:
  - `@GetMapping`: Para solicitudes GET.
  - `@PostMapping`: Para solicitudes POST.
  - `@PutMapping`: Para solicitudes PUT.
  - `@DeleteMapping`: Para solicitudes DELETE.

### Ejemplo:
```java
@GetMapping("/{id}")
public Persona obtenerPersona(@PathVariable Long id) {
    return personaRepository.findById(id).orElseThrow(() -> new RuntimeException("Persona no encontrada"));
}

@PostMapping
public Persona crearPersona(@RequestBody Persona persona) {
    return personaRepository.save(persona);
}
```

---

## **8. @RequestBody y @PathVariable**

### Descripción:
- `@RequestBody`:
  - Extrae datos del cuerpo de la solicitud.
  - Ideal para operaciones POST y PUT.

- `@PathVariable`:
  - Extrae valores directamente de la URL.
  - Ideal para operaciones que requieren un identificador único.

### Ejemplo:
```java
@PostMapping
public Persona crearPersona(@RequestBody Persona persona) {
    return personaRepository.save(persona);
}

@GetMapping("/{id}")
public Persona obtenerPersona(@PathVariable Long id) {
    return personaRepository.findById(id).orElseThrow(() -> new RuntimeException("Persona no encontrada"));
}
```

---

## **9. @Autowired**

### Descripción:
- Permite inyectar dependencias automáticamente.
- Se usa para conectar el repositorio con el controlador, entre otros usos.

### Ejemplo:
```java
@Autowired
private PersonaRepository personaRepository;
```
> **Nota:** Esto hace que `personaRepository` esté disponible en el controlador sin necesidad de inicializarlo manualmente.

---

## **10. @GeneratedValue**

### Descripción:
- Configura cómo se generará el valor para un campo anotado con `@Id`.
- Estrategias comunes:
  - `IDENTITY`: La base de datos genera el valor.
  - `SEQUENCE`: Usa una secuencia para generar valores únicos.

### Ejemplo:
```java
@Id
@GeneratedValue(strategy = GenerationType.SEQUENCE)
private Long id;
```
---

## **11. @ResponseEntity**

### Descripción:
- Representa toda la respuesta HTTP.
- Te permite configurar códigos de estado, encabezados y cuerpo de la respuesta.

### Ejemplo:
```java
@GetMapping("/{id}")
public ResponseEntity<Persona> obtenerPersona(@PathVariable Long id) {
    return personaRepository.findById(id)
           .map(ResponseEntity::ok)
           .orElse(ResponseEntity.notFound().build());
}
```
> **Nota:** Si la persona existe, devuelve un 200 (OK). Si no, un 404 (Not Found).

---

Con estas anotaciones, puedes crear aplicaciones Spring Boot de manera eficiente y organizada. ¡Espero que este resumen te sea útil! 🎉
