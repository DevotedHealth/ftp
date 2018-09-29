# goftp #

[![Build Status](https://travis-ci.org/dchang-dchang/ftp.svg?branch=master)](https://travis-ci.org/dchang-dchang/ftp)
[![Coverage Status](https://coveralls.io/repos/dchang-dchang/ftp/badge.svg?branch=master&service=github)](https://coveralls.io/github/dchang-dchang/ftp?branch=master)
[![Go ReportCard](http://goreportcard.com/badge/dchang-dchang/ftp)](http://goreportcard.com/report/dchang-dchang/ftp)

A FTP and FTPS client package for Go

## Install ##

```
go get -u github.com/dchang-dchang/ftp
```

## Usage ##

Create a FTP connection

```go
host, port := "ftp.hostname.com", 21
_ = s.Dial(fmt.Sprintf("%s:%d", host, port))
defer s.Quit()

```

Set timeouts, debug mode, and/or TLS config for implicit TLS

(Note: If a TLSConfig is set, both the command and data channel will be secured with the same config)

```go
host, port := "ftp.hostname.com", 990
s := ftp.ServerConn{
  Debug:     true,
  Timeout:   30*time.Second,
  TLSConfig: &tls.Config{ServerName: host},
}
_ = s.Dial(fmt.Sprintf("%s:%d", host, port))
defer s.Quit()
```

Login

```go
_ = s.Login(username, password)
```

List files
```go
entries, _ := s.List("/")
for _, entry := range entries {
  fmt.Println(entry)
}
```

Get a file
```go
res, _ := s.Retr("/filename.txt")
reader := bufio.NewReader(res)
for {
  line, err := reader.ReadString('\n')
  if err == io.EOF {
    break
  } else if err != nil {
    panic(err)
  }
  fmt.Println(line)
}
```

## Documentation ##

http://godoc.org/github.com/dchang-dchang/ftp
