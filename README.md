# trinity

A sweet Clojure API for [Atomix].

## Setup

Add the Leiningen dependency:

[![Clojars Project](http://clojars.org/io.atomix/trinity/latest-version.svg)](http://clojars.org/io.atomix/trinity)

## Core Usage

```clojure
(require '[trinity.core :as trinity])
```

Create an Atomix replica specifying local port to listen on and a set of remote servers that the replica should connect to:

```clojure
(trinity/replica 
  5555 
  [{:host node2 :port 5555}
   {:host node3 :port 5555}])
```

Create an Atomix client for a set of servers:

```clojure
(trinity/client
  [{:host node1 :port 5555}
   {:host node2 :port 5555}
   {:host node3 :port 5555}])
```

Open an Atomix client or replica:

```
(trinity/open! atomix)
```

Close an Atomix client or replica:

```
(trinity/close! atomix)
```

Note: Trinity functions operate sychronously by default, but many functions have async counterparts such as `open-async!` which return [CompletableFuture].

## Using Atomix Resources

#### Distributed Value

Get a distributed value for some resource name:

```clojure
(def register 
  (trinity/get-value client "register"))
```

Operate on the value:

```clojure
(require '[trinity.distributed-value :as dvalue])

(dvalue/get register)
(dvalue/set! register "value")
(dvalue/cas! register "expected" "updated")
```

#### Distributed Map

Get a distributed map for some resource name:

```clojure
(def cache 
  (trinity/get-map client "cache"))
```

Operate on the map:

```clojure
(require '[trinity.distributed-map :as dmap])

(dmap/get cache "key")
(dvalue/put! cache "key" "value")
(dvalue/remove! cache "key")
```

## Docs

API docs are available [here](http://atomix.io/trinity/docs/).

## License

Copyright © 2015 Jonathan Halterman

Distributed under the Eclipse Public License either version 1.0

[Atomix]: http://atomix.io/atomix
[CompletableFuture]: https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html