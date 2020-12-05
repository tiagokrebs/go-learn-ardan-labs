### Code coverage

70-80% é uma medida saudável para overall, 100% não existe.

```
go test -cover

go test -coverprofile c.out  # put profile on c.out

go tool cover -html=c.out  # pretty output
```