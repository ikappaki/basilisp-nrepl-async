# Changelog

## Unreleased

## 0.1.0b2

- Upgraded Basilisp to 0.2.4.
- Improved on the nREPL server exception messages by matching that of the REPL user friendly format, backported from [basilisp-blender](https://github.com/ikappaki/basilisp-blender).
- Fix incorrect line numbers for compiler exceptions in nREPL when evaluating forms in loaded files, backported from [basilisp-blender](https://github.com/ikappaki/basilisp-blender)
- nREPL server no longer sends ANSI color escape sequences in exception messages to clients, backported from [basilisp-blender](https://github.com/ikappaki/basilisp-blender).
- Conform to the `cider-nrepl` `info` ops spec by ensuring result's `:file` is URI, also added missing :column number, backported from [basilisp-blender](https://github.com/ikappaki/basilisp-blender).


## 0.1.0b1

- Initial version based on basilisp-blender nREPL server with improved error reporting.
