
* [Overview](#overview)
* [About Kubernetes](#about-kubernetes)
* [About Google Cloud Platform](#about-google-cloud-platform)
* [The secret that will be used](#the-secret-that-will-be-used)
* [The Go code](#the-go-code)
* [The Kubernetes setup](#the-kubernetes-setup)
* [The Semaphore project](#the-semaphore-project)
* [See also](#see-also)

## Overview

This document presents a recipe on how to publish a Go service to Kubernetes.

For the purposes of this recipe, the Google Cloud Platform will be used for
deploying the Kubernetes service but you can use any similar platform you want.

## About Kubernetes

Kubernetes is

## About Google Cloud Platform

The Google Cloud Platform can do many things. In this page, you are going to
use Google Cloud Platform to deploy a Go service in Kubernetes.

## The secret that will be used

You are going to need a `secret` for storing


## The Go code

The Go program that implements the web server is called web.go and contains the
following code:

	package main
    
	import (
	    "fmt"
	    "net/http"
	    "os"
	    "time"
	)
    
	func myHandler(w http.ResponseWriter, r *http.Request) {
	    fmt.Fprintf(w, "Serving: %s\n", r.URL.Path)
	    fmt.Printf("Served: %s\n", r.Host)
	}
    
	func timeHandler(w http.ResponseWriter, r *http.Request) {
	    t := time.Now().Format(time.RFC1123)
	    Body := "The current time is:"
	    fmt.Fprintf(w, "<h1 align=\"center\">%s</h1>", Body)
	    fmt.Fprintf(w, "<h2 align=\"center\">%s</h2>\n", t)
	    fmt.Fprintf(w, "Serving: %s\n", r.URL.Path)
	    fmt.Printf("Served time for: %s\n", r.Host)
	}
    
	func main() {
	    PORT := ":8001"
	    arguments := os.Args
	    if len(arguments) != 1 {
	        PORT = ":" + arguments[1]
	    }
	    fmt.Println("Using port number: ", PORT)
    
	    http.HandleFunc("/time", timeHandler)
	    http.HandleFunc("/", myHandler)
    
	    err := http.ListenAndServe(PORT, nil)
	    if err != nil {
	        fmt.Println(err)
	        return
	    }
	}

The presented web server supports the `/time` URL. Everything else will be
handled by the handler function of the `/` URL because the `/` URL in Go
matches everything.

Also, the web server uses port number **8001** by default unless you provide a
different port number as a command line argument.

## The Kubernetes setup



## The Semaphore project

The `.semaphore/semaphore.yml` file that will be used is as follows:


## See also

* [sem command line tool Reference](https://docs.semaphoreci.com/article/53-sem-reference)
* [Secrets YAML reference](https://docs.semaphoreci.com/article/51-secrets-yaml-reference)
