# SSO Service Protocol Buffers

Этот проект содержит Protocol Buffer определения для SSO (Single Sign-On) сервиса аутентификации и авторизации.

## Описание

Проект включает в себя:
- **Protocol Buffer схемы** для определения API сервиса аутентификации
- **Сгенерированный Go код** из .proto файлов
- **gRPC сервис** для регистрации, входа и проверки административных прав

## Структура проекта

```
protos/
├── proto/
│   └── sso/
│       └── sso.proto          # Protocol Buffer определения
├── gen/
│   └── go/
│       └── sso/
│           ├── sso.pb.go      # Сгенерированные Go структуры
│           └── sso_grpc.pb.go # Сгенерированный gRPC код
├── go.mod                     # Go модуль
└── README.md                  # Документация
```

## API Сервиса

### Auth Service

Сервис предоставляет следующие методы:

#### 1. Register
Регистрация нового пользователя.

**Request:**
```protobuf
message RegisterRequest {
    string email = 1;
    string password = 2;
}
```

**Response:**
```protobuf
message RegisterResponse {
    int64 user_id = 1;
}
```

#### 2. Login
Вход пользователя в систему.

**Request:**
```protobuf
message LoginRequest {
    string email = 1;
    string password = 2;
    int32 app_id = 3;
}
```

**Response:**
```protobuf
message LoginResponse {
    string token = 1;
}
```

#### 3. IsAdmin
Проверка административных прав пользователя.

**Request:**
```protobuf
message IsAdminRequest {
    string user_id = 1;
}
```

**Response:**
```protobuf
message IsAdminResponse {
    bool is_admin = 1;
}
```

## Установка и настройка

### Требования

- Go 1.24.5 или выше
- Protocol Buffers compiler (protoc)
- protoc-gen-go и protoc-gen-go-grpc плагины

### Установка зависимостей

```bash
# Установка protoc-gen-go
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest

# Установка protoc-gen-go-grpc
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
```

### Генерация кода

Для генерации Go кода из .proto файлов выполните:

```bash
# Генерация Go структур
protoc --go_out=. --go_opt=paths=source_relative \
    proto/sso/sso.proto

# Генерация gRPC кода
protoc --go-grpc_out=. --go-grpc_opt=paths=source_relative \
    proto/sso/sso.proto
```

## Использование

### Импорт в Go проекте

```go
import (
    "google.golang.org/grpc"
    ssov1 "protos/gen/go/sso"
)

// Создание клиента
conn, err := grpc.Dial("localhost:50051", grpc.WithInsecure())
if err != nil {
    log.Fatal(err)
}
defer conn.Close()

client := ssov1.NewAuthClient(conn)

// Пример регистрации
resp, err := client.Register(context.Background(), &ssov1.RegisterRequest{
    Email:    "user@example.com",
    Password: "password123",
})
```

## Версии

- **Go**: 1.24.5
- **protoc-gen-go**: v1.36.8
- **protoc**: v3.12.4

## Лицензия

[Укажите лицензию проекта]

## Контакты

[Укажите контактную информацию]
