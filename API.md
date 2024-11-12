# Table of contents
-  [`basilisp-nrepl-async.nrepl-server`](#basilisp-nrepl-async.nrepl-server) 
    -  [`logger`](#basilisp-nrepl-async.nrepl-server/logger) - The logger for this namespace.
    -  [`ops`](#basilisp-nrepl-async.nrepl-server/ops) - A map of operations supported by the nREPL server (as keywords) to function handlers for those operations.
    -  [`server-start!`](#basilisp-nrepl-async.nrepl-server/server-start!) - Creates an <code>socketserver/ThreadingTCPServer</code> nREPL server with optional <code>OPTS</code>.
-  [`basilisp-nrepl-async.utils`](#basilisp-nrepl-async.utils) 
    -  [`error->str`](#basilisp-nrepl-async.utils/error->str) - Converts the <code>ERROR</code> list to a human-readable string and returns it.
    -  [`error-add`](#basilisp-nrepl-async.utils/error-add) - Adds additional <code>DETAILS</code> to the existing <code>ERROR</code> list and returns it.
    -  [`error-make`](#basilisp-nrepl-async.utils/error-make) - Returns a list from the provided error <code>DETAILS</code> to represent an error.
    -  [`with-eprotect`](#basilisp-nrepl-async.utils/with-eprotect) - Creates a try/catch wrapper around <code>BODY</code> that returns any exception as an <code>error</code> prefixed with the <code>id</code> from <code>ID-OR-OPTS</code>.

-----
# <a name="basilisp-nrepl-async.nrepl-server">basilisp-nrepl-async.nrepl-server</a>






## <a name="basilisp-nrepl-async.nrepl-server/logger">`logger`</a><a name="basilisp-nrepl-async.nrepl-server/logger"></a>




The logger for this namespace.
<p><sub><a href="/blob/main/src/basilisp_nrepl_async/nrepl_server.lpy#L19-L21">Source</a></sub></p>

## <a name="basilisp-nrepl-async.nrepl-server/ops">`ops`</a><a name="basilisp-nrepl-async.nrepl-server/ops"></a>




A map of operations supported by the nREPL server (as keywords) to function
  handlers for those operations.
<p><sub><a href="/blob/main/src/basilisp_nrepl_async/nrepl_server.lpy#L332-L345">Source</a></sub></p>

## <a name="basilisp-nrepl-async.nrepl-server/server-start!">`server-start!`</a><a name="basilisp-nrepl-async.nrepl-server/server-start!"></a>
``` clojure

(server-start!)
(server-start! opts)
```
Function.

Creates an `socketserver/ThreadingTCPServer` nREPL server with
  optional `OPTS`. By default, it listens on `127.0.0.1` at a random
  available port and blocks for serving clients.

  It prints out the [`nrepl-server-signature`](#basilisp-nrepl-async.nrepl-server/nrepl-server-signature) message at startup for
  IDEs to pickup the host number to connect to.

  `OPTS` is a map of options with the following optional keys

  `:async?` If truthy, runs the server non-blocking in a separate
  thread where requests are queued instead of executed
  immediately. Returns a map with

     :error Contains details of any error during server creation.

     :host The host interface the server is bound to.

     :port The local port the server is listening on.

     :shutdown-fn The function to shutdown the server.

     :work-fn The function to process any pending client requests.

  `:host` The host address to bind to, defaults to 127.0.0.1.

  `:nrepl-port-file` An optional filepath to write the port number
  to. Typically set to .nrepl-port (the default) for the editors to
  detect.

  `:port` The port number to listen to, defaults to 0 which means to

  `:server*` An optional promise delivering a map on server upbringing
  with the following keys

     :host The host interface the server is bound to.

     :port The local port the server is listening on.

     :shutdown-fn The function to shutdown the server.
<p><sub><a href="/blob/main/src/basilisp_nrepl_async/nrepl_server.lpy#L568-L659">Source</a></sub></p>

-----
# <a name="basilisp-nrepl-async.utils">basilisp-nrepl-async.utils</a>






## <a name="basilisp-nrepl-async.utils/error->str">`error->str`</a><a name="basilisp-nrepl-async.utils/error->str"></a>
``` clojure

(error->str error)
```
Macro.

Converts the `ERROR` list to a human-readable string and returns
  it. Includes stack traces for any embedded exceptions.
<p><sub><a href="/blob/main/src/basilisp_nrepl_async/utils.lpy#L19-L29">Source</a></sub></p>

## <a name="basilisp-nrepl-async.utils/error-add">`error-add`</a><a name="basilisp-nrepl-async.utils/error-add"></a>
``` clojure

(error-add error & details)
```
Macro.

Adds additional `DETAILS` to the existing `ERROR` list and returns
  it.
<p><sub><a href="/blob/main/src/basilisp_nrepl_async/utils.lpy#L12-L17">Source</a></sub></p>

## <a name="basilisp-nrepl-async.utils/error-make">`error-make`</a><a name="basilisp-nrepl-async.utils/error-make"></a>
``` clojure

(error-make & details)
```
Macro.

Returns a list from the provided error `DETAILS` to represent an
  error.
<p><sub><a href="/blob/main/src/basilisp_nrepl_async/utils.lpy#L6-L10">Source</a></sub></p>

## <a name="basilisp-nrepl-async.utils/with-eprotect">`with-eprotect`</a><a name="basilisp-nrepl-async.utils/with-eprotect"></a>
``` clojure

(with-eprotect id-or-opts & body)
```
Macro.

Creates a try/catch wrapper around `BODY` that returns any exception
  as an `error` prefixed with the `id` from `ID-OR-OPTS`.

  `ID-OR-OPTS` is either a printable object used as the `id`, or a map
  with the following keys

  `:id` The `id` to prefix the error with.

  `:on-err-str` [opt] A function to call if an exception is caught,
  which accepts the formated error message as only argument.
<p><sub><a href="/blob/main/src/basilisp_nrepl_async/utils.lpy#L31-L61">Source</a></sub></p>
