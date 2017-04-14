[![Build Status](https://travis-ci.org/vietdien2005/laravel-circleci.svg?branch=master)](https://travis-ci.org/vietdien2005/laravel-circleci)

# Laravel CircleCI

## About

This project for create Image to testing laravel source with circleCI 2.0

## Create Project

``` laravel new source/laravel ``` 

Or 

``` composer create-project --prefer-dist laravel/laravel source/laravel ```

## Config

### Nginx 
Create config in folder build ` nginx/conf.d `


## Run 

``` docker-compose up -d ```

Go to ` http://172.48.0.2 ` and now you can write some code inside folder ` source `

## Build 

``` docker-compose build ```

## Pull without Build 

``` docker pull vietdien2005/laravel_circleci ```