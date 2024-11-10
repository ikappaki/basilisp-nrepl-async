[![PyPI](https://img.shields.io/pypi/v/basilisp-nrepl-async.svg?style=flat-square)](https://pypi.org/project/basilisp-nrepl-async/) [![CI](https://github.com/ikappaki/basilisp-nrepl-async/actions/workflows/tests-run.yml/badge.svg)](https://github.com/ikappaki/basilisp-nrepl-async/actions/workflows/tests-run.yml)

# A Basilisp async nREPL server for cooperative multitasking

## Overview
`basilisp-nrepl-async` is an nREPL server implementation for [Basilisp](https://basilisp.readthedocs.io/en/latest/) that allows client requests to be evaluated asynchronously by the main program at a chosen time and place. This enables cooperative multitasking on a single thread between the program and the nREPL server on the Python VM, while avoiding conflicts with the Global Interpreter Lock (GIL) and single threaded libraries.

## Installation

To install `basilisp-nrepl-async`, run:

```shell
pip install https://github.com/ikappaki/basilisp-nrepl-async/releases/download/v0.1.0b2/basilisp_nrepl_async-0.1.0b2-py3-none-any.whl
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
  (loop [] ;; suppose this is the main event loop

    (work-fn) ;; Execute any pending nREPL client work

    ;; simulate some work
    (time/sleep 0.5)

    (recur)))
```

The server will create an `.nrepl-port` file in the current working directory with the port number, which nREPL-enabled Clojure editors can use to connect.

You can also pass additional options to the `server-start!` function, such as `:host`, `:port` and `:nrepl-port-file`, to explicitly configure the server's listening interface, port, and the file where the port number is written (typically `<your-basilisp-lib>/.nrepl-port` for integration with your editor). See the function documentation for more details in [src/basilisp_nrepl_async/nrepl_server.lpy](src/basilisp_nrepl_async/nrepl_server.lpy)

```clojure
"Create an nREPL server with `server-make` (of which see) according to
  ``opts`` if given, and blocks for serving clients.

  It prints out the `nrepl-server-signature` message at startup for
  IDEs to pickup the host number to connect to.

  ``opts`` is a map of options with the following optional keys

  :async? If truthy, runs the server non-blocking in a separate thread
  where requests are queued instead of executed immediately. Returns a
  map with

           :error :error Contains details of any error during server
           creation.

           :host The host interface the server is bound to.

           :port The local port the server is listening on.

           :shutdown-fn The function to shutdown the server.

           :work-fn The function to process any pending client
           requests.

  :nrepl-port-file An optional filepath to write the port number
  to. Typically set to .nrepl-port for the editors to detect.

  :server* An optional promise of a map delivered on success with

           :host The host interface the server is bound to.

           :port The local port the server is listening on.

           :shutdown-fn The function to shutdown the server.

  also see `server-make` for additionally supported ``opts`` keys."
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
