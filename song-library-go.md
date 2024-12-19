go mod init song-library 
go get github.com/joho/godotenv 
go get github.com/go-chi/chi/v5 
go get github.com/go-chi/cors 
go get github.com/lib/pq 
go get github.com/google/uuid 
go get github.com/pressly/goose/v3
go get github.com/sqlc-dev/sqlc@v1.27.0
go install github.com/swaggo/swag/cmd/swag@latest
go get -u github.com/swaggo/http-swagger

postgres://pariram:@localhost:5432/song-library?sslmode=disable
goose -dir ./sql/schema postgres postgres://pariram:@localhost:5432/song_library up
swag init

### SWAGGER
```
package main

import (
	"httpSwagger "github.com/swaggo/http-swagger/v2
	"_ "github.com/par1ram/song-library/docs""
)

func main() {
 router.Get("/swagger/*", httpSwagger.Handler(httpSwagger.URL("http://localhost:8000/swagger/doc.json")))
}
```