[![PyPI](https://img.shields.io/pypi/v/basilisp-nrepl-async.svg?style=flat-square)](https://pypi.org/project/basilisp-nrepl-async/) [![CI](https://github.com/ikappaki/basilisp-nrepl-async/actions/workflows/tests-run.yml/badge.svg)](https://github.com/ikappaki/basilisp-nrepl-async/actions/workflows/tests-run.yml)

# Basilisp nREPL Server with Support for Cooperative Multitasking

[Basilisp](https://github.com/basilisp-lang/basilisp) is a Python-based Lisp implementation that offers broad compatibility with Clojure. Refer to [documentation](https://basilisp.readthedocs.io/en/latest/index.html) for more details.

## Overview

This package provides an nREPL server implementation for Basilisp, evolved from the `basilisp.contrib.nrepl-server` namespace in Basilisp, addressing issues that arise from serving nREPL request in parallel with the main event loop.

Serving an nREPL client connection on a parallel thread in Python may conflict with the Global Interpreter Lock (GIL) and single-threaded libraries, potentially causing errors or crashes.

To mitigate this, the library includes an asynchronous mode where client requests are queued, allowing the main application to process them at a designated time within its main event loop. This is in addition to the synchronous multi-threaded mode, where client requests are handled immediately upon arrival.

## Installation

To install `basilisp-nrepl-async`, run:

```shell
pip install https://github.com/ikappaki/basilisp-nrepl-async/releases/download/v0.1.0b3/basilisp_nrepl_async-0.1.0b3-py3-none-any.whl
```

## Usage

See [API.md](API.md).

### Asynchronous mode

To start the nREPL server on a random port bound to the local interface in asynchronous mode, call [server-start!](API.md#basilisp-nrepl-async.nrepl-server/server-start!) with the `async?` option set to `true`. Periodically invoke the returned `work-fn` within your program's main loop to handle client requests.

```clojure
(require '[basilisp-nrepl-async.nrepl-server :as nr])
(import time)

;; Start the nREPL server on a separate thread to handle client
;; requests asynchronously
(def server-async (nr/server-start! {:async? true}))
; nREPL server started on port 55144 on host 127.0.0.1 - nrepl://127.0.0.1:55144

;; Process client requests on this thread
(let [{:keys [host port shutdown-fn work-fn]} server-async]
  (loop [] ;; suppose this is the main event loop

    (work-fn) ;; Execute any pending nREPL client work

    ;; simulate some work
    (time/sleep 0.5)

    (recur)))
```

The server will create an `.nrepl-port` file in the current working directory with the port number, which nREPL-enabled Clojure editors can use to connect.

You can also pass additional options to the [server-start!](API.md#basilisp-nrepl-async.nrepl-server/server-start!) function, such as `:host`, `:port` and `:nrepl-port-file`, to explicitly configure the server's listening interface, port, and the file where the port number is written (typically `<your-basilisp-lib>/.nrepl-port` for integration with your editor).

### Synchronous mode

To start in synchronous mode, call [server-start!](API.md#basilisp-nrepl-async.nrepl-server/server-start!) with the optional `:host`, `:port` and `:nrepl-port-file` keys. The server will block and handle client requests as they arrive.

```clojure
(require '[basilisp-nrepl-async.nrepl-server :as nr])

(def server-async (nr/server-start! {:port 9999}))
; nREPL server started on port 9999 on host 127.0.0.1 - nrepl://127.0.0.1:9999
```

## Development and Testing

To run the test suite, use the following command:

```shell
basilsp test
```

## License

This project is licensed under the Eclipse Public License 2.0. See the [LICENSE](LICENSE) file for details.

## Acknowledgments

This library is a spin-off of [basilisp-blender](https://github.com/ikappaki/basilisp-blender)'s `basilisp-blender.nrepl-server` namespace.
