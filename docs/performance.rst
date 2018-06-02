Performance
===========

This library is written in Cython for a better performance than a pure-Python implementation could give you.


Decoder Performance
-------------------

The library is a bit slower than the shipped ``json`` module for *pure* JSON data.
If you know that your input does not use JSON5 extension, then this library is probably not what you need.

* Dataset: https://github.com/zemirco/sf-city-lots-json
* CPU: Core i7-3770 @ 3.40GHz
* :func:`pyjson5.decode`: **4.58** s ± 68.6 ms per loop *(lower is better)*
* :func:`json.loads`: **3.27** s ± 27.7 ms per loop
* The decoder works correcty: ``json.loads(content) == pyjson5.loads(content)``


Encoder Performance
-------------------

The encoder generates pure JSON data if there are no infinite or NaN values in the input, which are invalid in JSON.
The serialized data is XML-safe, i.e. there are no cheverons ``<>``, ampersands ``&``, apostrophes ``'`` or control characters in the output.
The output is always ASCII regardless if you call :func:`pyjson5.encode` or :func:`pyjson5.encode_bytes`.

* Dataset: https://github.com/zemirco/sf-city-lots-json
* CPU: Core i7-3770 @ 3.40GHz
* :func:`pyjson5.encode`: **8.54** s ± 29.3 ms per loop *(lower is better)*
* :func:`json.dumps`: **4.68** s ± 20.4 ms per loop
* :func:`json.dumps` + :func:`xml.sax.saxutils.escape`: **5.02** s ± 141 ms per loop
* The encoder works correcty: ``obj == json.loads(pyjson5.encode(obj, floatformat='%.16e'))``

Unless you need the advanced settings in :class:`pyjson5.Options`, most most likely don't benefit from using this library as an encoder.