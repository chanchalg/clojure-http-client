# Clojure HTTP Client

by Dan Larkin and Phil Hagelberg

Note: this library is deprecated. Clojure HTTP Client was created
simply to wrap the JDK's built-in HTTP classes, which are not as good
as the functionality that other clients such as Apache's provide. Now
that dependencies are not a huge headache to use from Clojure, there
is no reason limit yourself to the JDK's classes, so other libraries
are more capable.

Please take a look at [clj-http](http://github.com/dakrone/clj-http)
and [http.async.client](http://github.com/neotyk/http.async.client)
instead.

## Example

There are two namespaces, clojure-http.client, which provides a simple
"request" function, and clojure-http.resourcefully, which is targeted
more towards interactions with REST-based APIs.

    (ns clojure-http.example
      (:use [clojure-http.client]
            [clojure.contrib.json.write])
      (:require [clojure-http.resourcefully :as res]))

    (let [response (request "http://google.com")]
      (:code     response)  ;; 200
      (:msg      response)  ;; "OK"
      (:body-seq response)) ;; ("<html><head><meta[...]" ...

    (resourcefully/put "http://localhost:5984/my-db/doc1" 
                      {} (json-str {:hello "world"}))

    (res/with-cookies {}
      (res/post "http://localhost:3000/login" 
                {} {"user" user "password" password})
      (res/get "http://localhost:3000/my-secret-page))

The request function requires a URL and optionally accepts a method
(GET by default), a map of headers, a map of cookies, and a request
body. The resourcefully functions take a URL, an optional headers map,
and an optional body.

Request bodies may be strings, maps, or InputStreams. Strings get sent
verbatim. Maps get sent as application/x-www-form-urlencoded, and
InputStreams get streamed to the server 1024 bytes at a time, though
this can be changed by rebinding \*buffer-size\*.

## Usage

The functions in resourcefully are named after the HTTP verbs. Note
that resourcefully must be required with the :as option since it
defines a "get" function, which would interfere with clojure.core if
it were fully referred. Exceptions will be raised for status codes
that indicate problems, so you don't have to check return codes
manually. If you use resourcefully inside a "with-cookies" block,
cookies will automatically be saved in a *cookies* ref and sent out
with each request.

## TODO

* Connection pooling/keep-alive?
* Anything else? Send a message via Github or the Clojure mailing list.

Copyright (c) 2009-2010 Dan Larkin and Phil Hagelberg

Licensed under the same terms as Clojure. See COPYING
