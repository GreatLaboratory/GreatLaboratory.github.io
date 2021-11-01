---
title: "Golang - REST API"
excerpt: "echo 프레임워크를 활용한 간단한 crud, logging, basic auth"
toc: true
toc_sticky: true
categories:
  - Golang
last_modified_at: 2021-11-01
---

## Project init

```golang
// go module 사용
go mod init awesomeProject

// echo framework 사용
go get -u github.com/labstack/echo/v4
```

## main.go

```golang
package main

import (
	"awesomeProject/handlers"
	"awesomeProject/middlewares"
	"github.com/labstack/echo/v4"
	"net/http"
)

func main() {
	// create echo instance
	e := echo.New()

	// set logger config
	e.Use(middlewares.LoggerConfig())

	// track all routers
	e.Use(middlewares.RountingTracker)

	// simple hello world
	e.GET("/", func(c echo.Context) error {
		return c.String(http.StatusOK, "Hello world")
	})

	// set user router group
	userRouter := e.Group("/users")

	// authorize user router using basic auth
	userRouter.Use(middlewares.BasicAuthorization())

	// compose user routers
	userRouter.POST("/v1", handlers.CreateUserHandler1)
	userRouter.POST("/v2", handlers.CreateUserHandler2)
	userRouter.POST("/upload/profile-img", handlers.UploadProfileImgHandler)
	userRouter.GET("/:id", handlers.GetUserHandler)
	userRouter.GET("", handlers.SearchUserHandler)
	/**
	userRouter.PUT("/:id")
	userRouter.DELETE("/:id")
	 */

	// set port
	e.Logger.Fatal(e.Start(":1323"))
}
```

- 위와 같이 golang의 entry file은 프로젝트 루트 디렉토리에 위치하며 무조건 `package main`으로 구성해야한다.
- 이후 루트 디렉토리에서 `go run main.go` 을 실행하면 어플리케이션이 동작한다.
- 해당 파일에서 echo 어플리케이션이 실행되는 구조는 node.js의 express와 매우 닮아있다. 특히 middleware를 사용하여 context를 관리하는 것이 닮아있다.
- `userRouter := e.Group("/users")` 으로 엔드포인트를 그룹지어서 특정 그룹에 대한 미들웨어(e.g. authorization)을 걸 수도 있다.
- import에선 golang의 내장 라이브러리나 외부 라이브러리를 로드하는 것과 더불어 프로젝트의 외부 파일을 모듈화하여 로드한다. (e.g. awesomeProject/handlers, awesomeProject/middlewares)

## Reference

- [echo docs guide](https://echo.labstack.com/guide/)
- [github code](https://github.com/GreatLaboratory/go-web-example2)
