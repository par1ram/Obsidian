go mod init github.com/par1ram/aggregator-go
go build && ./aggregator-go
go get github.com/joho/godotenv
go get github.com/go-chi/chi/v5
go get github.com/go-chi/cors
go get github.com/lib/pq
go get github.com/google/uuid
go mod tidy
go mod vendor

go install github.com/pressly/goose/v3/cmd/goose@latest
go install github.com/sqlc-dev/sqlc/cmd/sqlc@latest

goose postgres postgres://pariram:@localhost:5432/aggregator_go up
goose postgres postgres://pariram:@localhost:5432/aggregator_go down
sqlc generate

psql postgres
URL connection "postgres://pqgotest:password@localhost/dtname?sslmode=verify-full"
### End points
- /v1/healthz
- /v1/error
- /v1/users get post
- /v1/feeds get post
- /v1/feed_follows get post
- v1/feed_follows/{feedFollowId} delete
- v1/posts get

### Status Codes
- 201 created code
- 403 permission denied

- `json.Marshal` преобразует типы данных go в json формат, возвращает срез байтов
- `handle_readiness` функция, которая передает в сервис пустую структуру, чтобы проверить работоспособность, готовность сервиса
- теги в go
```
type errResponse struct {
	// ключ Error для json преобразуется в нижний регистр
	Error string `json:"error"` 
}
```
- Передавать контекст http необходимо для того, чтобы его корректно закрыть. То есть каждая горутинку обрабатывает один запрос, нам необходимо её в итоге убить.

### Ошибки с которыми столкнулся
- Написал авторизацию по уникальному ключу. При попытке получить пользователя, приходила ошибка, что нет записей, хотя ключ был верный. В итоге ошибка в том, что я возвращал в функции авторизации не ключ, а слово ApiKey.
- Забыл поставить для связи UUID NOT NULL, потом долго не мог понять, почему не могу использовать id юзера для таблицы feed. Были разные типы UUID.