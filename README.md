# Base💯

Encode things into Emoji.

Base💯 can represent any byte with a unique emoji symbol, therefore it can
represent binary data with zero printable overhead (see caveats for more info).

## Usage

```
$ echo "the quick brown fox jumped over the lazy dog" | base100
👫👟👜🐗👨👬👠👚👢🐗👙👩👦👮👥🐗👝👦👯🐗👡👬👤👧👜👛🐗👦👭👜👩🐗👫👟👜🐗👣👘👱👰🐗👛👦👞🐁
```

Base💯 will read from stdin unless a file is specified, will write UTF-8 to
stdout, and has a similar API to GNU's base64. Data is encoded by default,
unless `--decode` is specified; the `--encode` flag does nothing and exists
solely to accommodate lazy people who don't want to read the docs (like me).

```
USAGE:
    base100 [FLAGS] [input]

FLAGS:
    -d, --decode     Tells base💯 to decode this data
    -e, --encode     Tells base💯 to encode this data
    -F, --fast       Go twice as fast, but crash on imperfect input (decode only)
    -h, --help       Prints help information
    -V, --version    Prints version information

ARGS:
    <input>    The input file to use
```

## Caveats

Base💯 is *very* space inefficient. It bloats the size of your data by around 3x,
and should only be used if you have to display encoded binary data in as few
__printable__ characters as possible.

## Performance

```
$ cat /dev/urandom | base100 | pv > /dev/null
 [ 502MiB/s]

$ cat /dev/urandom | base64 | pv > /dev/null
 [ 232MiB/s]

[adam@AdamsPC release]$ cat /dev/urandom | base100 | base100 -dF | pv > /dev/null
 [ 223MiB/s]

[adam@AdamsPC release]$ cat /dev/urandom | base64 | base64 -d | pv > /dev/null
 [ 176MiB/s]
```

In both scenarios, base💯 compares favorably to GNU base64. It should be noted
that base100 in fast-mode sacrifices all sanity checks and makes zero guarantees
about gracefully handling malformed input.

## Future plans

- Allow data to be encoded with the full 1024-element emoji set
- Add further optimizations and ensure we're abusing SIMD as much as possible
- Make fast mode less fragile while maintaining fastness