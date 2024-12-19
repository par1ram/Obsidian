![[OMS.png]]

gateway > go run .

go mod init github.com/par1ram/oms-[service]
go work init ./common ./gateway ./kitchen ./orders ./payment ./stock
go get github.com/joho/godotenv
air init

brew install protobuf
gRPC
```sh
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
```

Makefile 
```sh
protoc --go_out=. --go_opt=paths=source_relative \
    --go-grpc_out=. --go-grpc_opt=paths=source_relative \
    helloworld/helloworld.proto
```

...
go get github.com/hashicorp/consul/api
