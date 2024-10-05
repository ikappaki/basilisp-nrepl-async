[![PyPI](https://img.shields.io/pypi/v/basilisp-nrepl-async.svg?style=flat-square)](https://pypi.org/project/basilisp-nrepl-async/) [![CI](https://github.com/ikappaki/basilisp-nrepl-async/actions/workflows/tests-run.yml/badge.svg)](https://github.com/ikappaki/basilisp-nrepl-async/actions/workflows/tests-run.yml)

# A Basilisp async nREPL server for cooperative multitasking

## Overview
`basilisp-nrepl-async` is an nREPL server implementation for Basilisp that allows client requests to be evaluated asynchronously by the main program at a chosen time and place. This enables cooperative multitasking on a single thread between the program and the nREPL server.

## Installation

To install `basilisp-nrepl-async`, run:

```shell
pip install basilisp-nrepl-async
```

## Usage

Start the nREPL server in async mode and periodically call its `work-fn` within the program's main loop

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


## Development and Testing

To run the test suite, use the following command:

```shell
basilsp test
```

## License

This project is licensed under the Eclipse Public License 2.0. See the [LICENSE](LICENSE) file for details.

## Acknowledgments

This library is a spin-off of the [basilisp-blender](https://github.com/ikappaki/basilisp-blender) nrepl-server namespace.
