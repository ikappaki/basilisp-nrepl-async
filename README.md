[![PyPI](https://img.shields.io/pypi/v/basilisp-nrepl-async.svg?style=flat-square)](https://pypi.org/project/basilisp-nrepl-async/) [![CI](https://github.com/ikappaki/basilisp-nrepl-async/actions/workflows/tests-run.yml/badge.svg)](https://github.com/ikappaki/basilisp-nrepl-async/actions/workflows/tests-run.yml)

# A Basilisp async nREPL server for cooperative multitasking

## Overview
`basilisp-nrepl-async` is an nREPL server implementation for [Basilisp](https://basilisp.readthedocs.io/en/latest/) that allows client requests to be evaluated asynchronously by the main program at a chosen time and place. This enables cooperative multitasking on a single thread between the program and the nREPL server on the Python VM.

## Installation

To install `basilisp-nrepl-async`, run:

```shell
pip install https://github.com/ikappaki/basilisp-nrepl-async/releases/download/v0.1.0b1/basilisp_nrepl_async-0.1.0b1-py3-none-any.whl
```

## Usage

To start the nREPL server on a random port bound to the local interface, call the the `basilisp-nrepl-async.nrepl-server/server-start!` function with the `async?` option set to `true`, and periodically invoke the returned `work-fn` within your program's main loop. 

```clojure
(require '[basilisp-nrepl-async.nrepl-server :as nr])
(import time)

;; Start the nREPL server on a separate thread to handle client
;; requests asynchronously
(def server-async (nr/start-server! {:async? true}))
; nREPL server started on port 55144 on host 127.0.0.1 - nrepl://127.0.0.1:55144

;; Process client requests on this thread 
(let [{:keys [host port shutdown-fn work-fn]} server-async]
  (try 

    (loop [] ;; suppose this is the main loop

      (work-fn) ;; Execute any pending nREPL client work

      ;; simulate some work
      (time/sleep 0.5)

      (recur))))
```

The server will create an `.nrepl-port` file in the current working directory with the port number, which nREPL-enabled Clojure editors can use to connect.

You can also pass additional options to the `server-start!` function, such as `:address` and `:port`, to explicitly set the server's listening interface and port, respectively. See the function documentation for more details.

## Development and Testing

To run the test suite, use the following command:

```shell
basilsp test
```

## License

This project is licensed under the Eclipse Public License 2.0. See the [LICENSE](LICENSE) file for details.

## Acknowledgments

This library is a spin-off of the [basilisp-blender](https://github.com/ikappaki/basilisp-blender) nrepl-server namespace.
