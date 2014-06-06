------------------------------------
Erlang, Notes on Speed/Performance
------------------------------------

Notes from various talks I've watched on the web, related to writing speedy Erlang code.


  * Message passing using gen_server:call() is 2x slower than Pid ! Msg.

  * For speedy code use barebone processes to gen_servers

  * Use NIFs VERY CAREFULLY. NIFs undermine key properties of Erlang (realiability & soft-realtime guarantees)

  * Arbitrary precission numbers are not recommended for fast code. 32-bit or 62-bit unsigned integer counters with overflow - trivial in C, not easy in Erlang

  * Garbage Collection is key to speed

  * Arguments are registers. Many arguments do not make a function slower. Don't shuffle arguments:
  % SLOW
  animal(Cat, Dog, Horse) ->
    feed(Dog, Cat, Horse);
    ...

  * ETS tables are (mostly) ok, howerver:

  Do not use ets:tab2list() inside tight loops. Treat ETS as a Database, not as a set of global variables.

  Beware, ets:lookup(), etc creates a copy of the data on the heap of the caller, similarly to message passing.

  * Do not overuse records. Use tuples explicitly in performance critical sections.

  *