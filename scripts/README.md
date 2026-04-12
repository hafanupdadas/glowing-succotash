# V2Go Checker Binary

Положи сюда скомпилированный бинарник `v2go-checker` (Linux) или `v2go-checker.exe` (Windows).

## Сборка

```bash
cd ../../v2go-main
GOOS=linux GOARCH=amd64 go build -ldflags="-s -w" -o bin/v2go-checker main.go output.go sort.go
cp bin/v2go-checker ../v2go_checker_bin/scripts/
```

## Проверка

```bash
chmod +x v2go-checker
./v2go-checker --help
```

## Размер бинарника

Ожидаемый размер: ~50-80 MB (включает Xray-core библиотеку)
