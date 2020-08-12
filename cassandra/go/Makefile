build-go:
	@echo "Building go seed"
	go build -a -installsuffix cgo \
		-ldflags "-s -w" \
		-o ../image/files/cassandra-seed.so -buildmode=c-shared src/main.go