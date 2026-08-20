---
schema_version: "1.0.0"
document_id: "eeba6772f06eccc0baf9322f1a92658589da20ffea52056f9232f6fa74f96bfe"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/releasing-czlib-and-zstd-go-bindings/"
published_at: "2016-07-11T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T22:27:31.756931+00:00"
content_hash: "sha256:fdab30523e327bc03b3090fba0822613d27e465535cf68f7c1a5ab02762295fb"
---

# Releasing czlib and zstd Go bindings

Jason Moiron


To commemorate the third annual[GopherCon US](https://www.gophercon.com/) in Denver this week, we’re releasing cgo bindings to two compression libraries that we’ve been using in production at Datadog for a while now:[czlib](https://github.com/datadog/czlib) and[zstd](https://github.com/datadog/zstd) .


[czlib](https://github.com/datadog/czlib) started as a fork of the[vitess project’s cgzip](https://github.com/youtube/vitess/tree/master/go/cgzip) package. Our primary data pipeline uses zlib compressed messages, but the standard library’s pure Go implementation can be significantly slower than the C zlib library. In order to address this gap, we modified a few flags in <code>cgzip</code> to make it encode and decode with[zlib wrapping](https://tools.ietf.org/html/rfc1950) rather than with gzip headers.


We’ve detailed some of the other more novel design decisions in <code>[czlib](https://github.com/datadog/czlib) </code>, including its batch interfaces, in our[general blog on performance in Go](https://www.datadoghq.com/blog/go-performance-tales/) a couple of years ago. Performance varies quite a bit among the various interfaces, so it pays to benchmark using a message that is typical for your system by running the <code>[czlib](https://github.com/datadog/czlib) </code> benchmark suite with PAYLOAD=path_to_message go test -run=NONE -bench .


Here are modern benchmark results running go1.7beta2 for compression and decompression using the non-streaming interface in czlib, the streaming interface, and the standard library’s[compress/zlib](https://golang.org/pkg/compress/zlib/) that show the variance in performance:


~~~~


# using a 2kb plaintext message


BenchmarkCompress-4 30000 47415 ns/op 44.42 MB/s BenchmarkCompressStream-4 20000 61732 ns/op 34.11 MB/s BenchmarkCompressStdZlib-4 5000 227182 ns/op 9.27 MB/s BenchmarkDecompress-4 200000 8238 ns/op 255.62 MB/s BenchmarkDecompressStream-4 100000 18352 ns/op 114.75 MB/s BenchmarkDecompressStdZlib-4 50000 31565 ns/op 66.72 MB/s


# using a 1.7MB plaintext message


BenchmarkCompress-4 20 69808144 ns/op 24.70 MB/s BenchmarkCompressStream-4 20 73170819 ns/op 23.56 MB/s BenchmarkCompressStdZlib-4 20 70498763 ns/op 24.46 MB/s BenchmarkDecompress-4 200 6709252 ns/op 256.98 MB/s BenchmarkDecompressStream-4 200 6891833 ns/op 250.18 MB/s BenchmarkDecompressStdZlib-4 100 14256445 ns/op 120.94 MB/s ~~~~


[zstd](https://github.com/Cyan4973/zstd) , pronounced Zstandard, is a relatively new fast compression library from[Yann Collet](http://fastcompression.blogspot.fr/) , the author of lz4. It has recently finalized its format, and a 1.0 release is pending. It compresses slightly faster than zlib at level 6 at a slightly better ratio, and decompresses much faster, making it a great general purpose zlib replacement.


The[zstd library](https://github.com/Cyan4973/zstd) supports some interfaces that are common in more advanced compression libraries like stream compression, compression levels and pre-computed dictionaries. These are all exposed by our[zstd Go binding](https://github.com/Datadog/zstd) , with the dictionary builder available in the upstream repos. The binding intentionally mimics the zlib interface, and aside from a few functions that do not return error in zstd, it is functionally a drop-in replacement. It also exposes a fixed-length batch compression interface present in the underlying library, very similar to the lz4 interface.


-
-
-
