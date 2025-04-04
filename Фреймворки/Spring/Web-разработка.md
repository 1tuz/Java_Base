#Spring #spring_web
🔹 **Spring Boot упрощает создание REST API**

✅ **Контроллеры**

- `@RestController` – делает класс контроллером REST, объединяет `@Controller` и `@ResponseBody`.
- `@RequestMapping` – задает общий путь для обработчиков запросов в контроллере.
- `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping` – аннотации для маршрутизации HTTP-запросов.
- `@PathVariable` – извлекает переменные пути (`/users/{id}`).
- `@RequestParam` – получает параметры запроса (`/users?name=John`).
- `@RequestBody` – принимает тело запроса (JSON, XML и т. д.).

✅ **Клиенты для HTTP-запросов**

- `RestTemplate` – синхронный клиент, удобен для простых запросов, но устарел.
- `WebClient` – реактивный клиент, поддерживает асинхронные вызовы и стриминг данных.
- `FeignClient` – декларативный HTTP-клиент, интегрируется с Spring Cloud, упрощает взаимодействие между сервисами.
