---
title: 'Cloud Integration Docs'
author: 'Thomas LACAZE'
date: 25/11/2021
---

*This document is generated automatically at each commit thanks to Drone CI*
See here for more information about CI : https://drone.io/

All code source is available at **[Github](https://github.com/Cloud-Integration-2021)**. Each lab has a readme and a installation steps

![Stack](images/stack.png)

# Lab1 - Java API Rest
*Using Spring boot, Hibernate and JPA with PostgreSQL, Swagger*

CRUD operations on movies. With CircuitBreaker, Retry and Timeout. 

There are two version of this API:

## V1

Using CircuitBreaker, Retry, TimeLimiter with annotations. All endpoints disposes of a circuit breaker, retry and only *getAll* and *getById* has TimeLimiter.

So in fact, Spring root all traffic to service B. If service B is down, the circuit breaker will open. And request will be sent to himself.

## V2

Using CircuitBreaker as we saw in *spring-boot-sample*. So all crud operations are managed by Spring boot with no circuitbreaker.

Only *getAll* and *getById* are managed by CircuitBreaker : all informations from Movie using JPA repository, but actors list will be managed by service B. 

If service B is down, the circuit breaker will open. And request will be sent to himself with default list of actors. 

## Integration tests

Each version has a different integration test. No unit test decause we used JPA default repository.

Due to TimeLimiter integration tests needs to be asynchronous.

Spring profile to use H2 in tests 


## CI 

**[See here](https://raw.githubusercontent.com/Cloud-Integration-2021/lab1/main/.drone.yml)**
------Integration, building image, pushing image to registry are done with CI.  

# Lab2 - Golang API Rest
*Using Gin framework, Gorm with PostgreSQL*

It's a replication of the previous API but in Golang. With CRUD operations on movies.

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

Integration, building image, pushing image to registry are done with CI.  

## CI 

**[See here](https://raw.githubusercontent.com/Cloud-Integration-2021/lab2/main/.drone.yml)**

## Lab3 - React Web App
*Using tailwind*

Web UI using V1 routes from API without actors list. So there are CircuitBreaker and Retry on each route.

SCREEN HERE


## CI 

**[See here](https://raw.githubusercontent.com/Cloud-Integration-2021/lab3/main/.drone.yml)**

There are 3 pipelines :

- *Build* : trigger on every action expect promote. This pipeline will build app with yarn.

- *Deploy docker container & readme* : trigger on every action in branch main except promote on main. This pipeline will deploy docker container and update repo readme to dockerhub.

- *Deploy to S3* : trigger on every promote. This pipeline will build application and use static build to push it to S3.

**[Promote is done in Drone UI](https://readme.drone.io/promote/)**

# Lab4 - Deploy to AWS
## Deploy to S3


Deploy to S3 with CI.

SCREEN HERE

## Deploy to Beanstalk

SCREEN HERE

## CORS

Les règles CORS (Cross Origin Resource Sharing) sont un ensemble de standard qui régissent l'accès à des ressources situées sur des domaines différents et permettent de sélectionner les en-têtes accessibles.

En ayant un frontend et backend ségaré et n'utilisant pas le localhost, nous avons remarqué que certains en-têtes étaient bloqués par les CORS. Ainsi nous avons dû créer une Cache Policy et une Origin Policy afin de permettre la transmission des en-têtes.

# Installation Steps

## Prerequisites
* Docker
* Docker-compose

## Clone repo

```bash
$ git clone https://github.com/Cloud-Integration-2021/docs
$ cd docs
```

## Fill all environment variables .env

```bash
CLOUDINTEGRATION_DB_HOST=**
CLOUDINTEGRATION_DB_PORT=**
CLOUDINTEGRATION_DB_USER=**
CLOUDINTEGRATION_DB_PASSWORD=**
CLOUDINTEGRATION_DB_NAME_LAB2=**
CLOUDINTEGRATION_DATASOURCE_DB_URL_LAB1=jdbc:postgresql://**:**/**
```
*Lab1 and Lab2 need to have a separated database*


## Build docker-compose.yml

```bash
$ docker-compose up
```

# License

**[MIT](https://github.com/Cloud-Integration-2021/docs/blob/master/LICENSE)**
