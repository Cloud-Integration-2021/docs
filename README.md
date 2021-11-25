# Cloud Integration - Thomas LACAZE - M2 2022
*This document is generated automatically at each commit thanks to Drone CI*

All code source is available at [Github](https://github.com/Cloud-Integration-2021). Each lab has a readme and a installation steps

## Lab1 - Java API Rest
Using Spring boot, Hibernate and JPA with PostgreSQL, Swagger


There are two version of this API:
- V1: 
- V2: 

## Lab2 - Golang API Rest
Using Gin framework, Gorm with PostgreSQL

It's a replication of the previous API but in Golang.

```go
	r.GET("/movies", DB.FindMovies)
	r.GET("/movies/:id", DB.FindMovieById)
	r.POST("/movies", DB.CreateMovie)
	r.PUT("/movies/:id", DB.UpdateMovie)
	r.DELETE("/movies/:id", DB.DeleteMovie)
```

But with a new endpoint for actors and no persistent storage of actors : Genearte a random list of actors for a movie.

```go
	r.GET("/actors/:id", v2.FindActorsByMovieId)
```

## Lab3 - React Web App


## Lab4 - Deploy to AWS


### Deploy to S3


### Deploy to Beanstalk

## 🛠️ Installation Steps

### Prerequisites
* Docker
* Docker-compose

### Download docker-compose.yml

```bash
$ curl -L https://raw.githubusercontent.com/Cloud-Integration-2021/lab-4/master/docker-compose.yml > docker-compose.yml
```

### Fill all environment variables .env

```
CLOUDINTEGRATION_DB_HOST=**
CLOUDINTEGRATION_DB_PORT=**
CLOUDINTEGRATION_DB_USER=**
CLOUDINTEGRATION_DB_PASSWORD=**
CLOUDINTEGRATION_DB_NAME_LAB2=**
CLOUDINTEGRATION_DATASOURCE_DB_URL_LAB1=jdbc:postgresql://**:**/**
```
*Lab1 & Lab2 need to have a separated database*


### Build docker-compose.yml

```bash
$ docker-compose up
```

🌟 You are all set!
## License
<a href="https://github.com/Cloud-Integration-2021/docs/blob/master/LICENSE">MIT</a>