go get github.com/99designs/gqlgen@v0.17.60
go run github.com/99designs/gqlgen generate

go:generate protoc --go_out=./pb --go-grpc_out=./pb account.proto

