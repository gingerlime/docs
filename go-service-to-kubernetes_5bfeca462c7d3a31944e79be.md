
* [Overview](#overview)
* [About Kubernetes](#about-kubernetes)
* [About Google Cloud Platform](#about-google-cloud-platform)
* [The secret that will be used](#the-secret-that-will-be-used)
* [The Go code](#the-go-code)
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

The Go program that implements the web server is as follows:



## The Semaphore project

The `.semaphore/semaphore.yml` file that will be used is as follows:


## See also

* [sem command line tool Reference](https://docs.semaphoreci.com/article/53-sem-reference)
* [Secrets YAML reference](https://docs.semaphoreci.com/article/51-secrets-yaml-reference)
