---
schema_version: "1.0.0"
document_id: "7cbfe720a2b4b8e8ae99b93eebbd1fcb77b2d9320a12ff4bf571da11b254459c"
company_key: "doximity-inc-class-a-common-stock"
company: "Doximity Inc."
source_id: "doximity-inc-class-a-common-stock-rss-4199c1c9997e"
canonical_url: "https://technology.doximity.com/articles/ruby-delights-built-into-the-language"
published_at: "2022-12-20T16:00:00+00:00"
first_seen_at: "2026-07-25T01:15:21.586776+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:16ae80d9cfdb4b5d4aff0255d702d6125095be662f4372c8692ff88f357319a9"
---

# Ruby Delights Built Into The Language

*BTW, we're ⚡ hiring Infra, SRE, Web, Mobile, and Data engineers at[Doximity (see roles)](https://work.doximity.com/positions) -- find out more about our[technical stack](https://technology.doximity.com/technology-stack) .*


---


# Ruby Delights Built Into The Language


The hidden gems that make Ruby a delight are not always... gems. There are so many Ruby features *built in* to the language itself. In[the very first episode of this series](https://technology.doximity.com/articles/the-hidden-gems-of-ruby-s-irb) , we dove deep into the internals of one such gem bundled with Ruby's standard library: IRB. Keeping on theme, I'd like to take some time to explore some other areas of Ruby's source.


So get ready to open up that IRB console you read all about, and we'll explore some incredible Ruby examples provided to you by Ruby itself.


# Jump To Your Favorites


1. benchmark.rb – Measure performance of Ruby code
2. biorhythm.rb – A biorhythm calculator
3. cal.rb – A simple calendar display
4. cbreak.rb – Supress echo of terminal input using ioctl
5. cgi-session-pstore.rb – A file-based persistence mechanism for tracking the CGI Session as a Hash
6. clnt.rb, svr.rb and tsvr.rb – Start a TCP socket server and connect a client
7. coverage.rb – Simple test code coverage
8. delegate.rb – Delegating methods with ease
9. dir.rb – Directory access
10. DRb – Distributed object system for Ruby (think RPC for OO)
11. dualstack-fetch.rb and dualstack-httpd.rb – A simultaneous multi-threaded IPv4/IPv6 TCP server and client
12. eval.rb – A simple evaluator
13. export.rb – method access example
14. exyacc.rb – Extract BNF from the yacc file
15. fact.rb – A factorial calculator
16. fib.rb – Fibonacci number calculations
17. from.rb – Scan mail spool
18. fullpath.rb – Convert ls -lR to fullpath format
19. iseq_loader.rb – A sample of compiler/loader for binary compiled file
20. less.rb – A front-end for the less command


---


## Benchmark


[source: benchmark.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/benchmark.rb)


Benchmarking can be an incredibly valuable tool, but is quite the rabbit hole of theory vs. pragmatism. The "what", and "how", of benchmarking is fraught with sharp opinions and inconsistent recommendations. For now, we'll throw all that out the window and focus on Ruby's lovely example using the[Benchmark module](https://rubyapi.org/3.1/o/benchmark) to calculate the fastest way to assign a single character string to a local variable inside various iterators.


```text
$> ruby sample/benchmark.rb
[50000 times iterations of `a = "1"']
user     system      total        real
for:      0.023797   0.000935   0.024732 (  0.025606)
times:    0.017888   0.000440   0.018328 (  0.018801)
upto:     0.016897   0.000565   0.017462 (  0.017925)
0.014552   0.000614   0.015166 (  0.015863)
0.016184   0.000804   0.016988 (  0.017833)
0.015323   0.000534   0.015857 (  0.016280)


```


I think this is a nice little introduction to the power of the tool. With a little tweaking we could try to isolate side-affects of the environment and VM (like garbage collection) and show some simple statistics on averages and total timing. I'll leave that to you :-)


### IRB's friendly benchmarking extension: measure


As of Ruby 3.0, you can now get instant feedback on how long things take to process right in IRB!


```text
irb  (  main  ):  001  :  0  >    measure
TIME    is    added  .
=  >    nil


irb  (  main  ):  002  :  0  >    sleep    1
processing    time:   1.004886  s
=>    1


irb  (  main  ):  003  :  0  >    puts    "Measure me."
Measure    me  .
processing    time:   0.000043  s
=>    nil


irb  (  main  ):  004  :  0  >    measure    :off
=>    nil


```


Check out my[deep-dive on IRB](https://technology.doximity.com/articles/the-hidden-gems-of-ruby-s-irb) for more juicy features!


### Learn More About Benchmarking Ruby Code


Benchmarking is a popular (and broad) topic. This sample is a tiny piece in the very large benchmarking pie. If you want to dig in more, here are some great entry points:


- Much of` Benchmark` has moved to the[benchmark gem](https://github.com/ruby/benchmark)
- [Matthew Gaudet](https://twitter.com/MattStudies) 's 2016 RubyKaigi talk is a great introduction on the science of benchmarking:[Ruby3x3: How are we going to measure 3x?](https://www.youtube.com/watch?v=kJDOpucaUR4)
- There are many[long-running Ruby benchmarks tracked by RubyBench](https://rubybench.org/benchmarks) .
- [Benchmarking various Ruby implementations](https://eregon.me/blog/2022/01/06/benchmarking-cruby-mjit-yjit-jruby-truffleruby.html)
- [Learning by Benchmarking A Proposed Ruby Change](https://www.mayerdan.com/ruby/2018/03/25/ruby-benchmarking) by[Dan Mayer](https://twitter.com/danmayer)
- [fast-ruby](https://github.com/fastruby/fast-ruby) : micro-benchmarks of Ruby idioms.
- The[time_up gem](https://blog.testdouble.com/posts/2021-07-19-benchmarking-your-ruby-with-time_up/) is a great tool for benchmarking aggregates and comparing


---


## A Biorhythm Calculator


[source: biorhythm.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/biorhythm.rb)


I wasn't able to find the original post from the Newsgroup referenced in the comment, but this was by far one of my favorite examples. It prompts you for your birth date, and then crunches some planetary numbers to calculate, AND GRAPH, your emotional (E), physical (P), and mental (M) well being! This week I'm sharp-as-a-tack, and slow and sad as a sack. Here's to next week!


```text
$>   ruby sample/biorhythm.rb
Your birthday  (  YYYYMMDD )  : 19840221


>>>   Biorhythm  <<<
The birthday 1984.02.21 is a Tue
Age  in   days:  [  13770]


P  =  physical,  E  =  emotional,  M  =  mental
-------------------------  +-------------------------
Bad Condition    |    Good Condition
-------------------------  +-------------------------
2021.11.03 : .E.......................|........................M
2021.11.04 : P.E......................|.......................M.
2021.11.05 : .P...E...................|.....................M...
2021.11.06 : ...P.....E...............|..................M......
2021.11.07 : .......P......E..........|..............M..........
2021.11.08 : ............P......E.....|..........M..............
2021.11.09 : ..................P......E......M..................
2021.11.10 : .........................P.M...E...................
2021.11.11 : .......................M.|......P...E..............
2021.11.12 : ..................M......|............P..E.........
-------------------------  +-------------------------


```


---


## Display A Simple Calendar


[source: cal.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/cal.rb)


Another fun example here with some practical use.[Cal.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/cal.rb) is a clone of the[linux cal(1) command](https://man7.org/linux/man-pages/man1/cal.1.html) re-implemented in Ruby. It will print out a calendar in your terminal in several flavors (examples below). It makes some very interesting use of[GetoptLong](https://github.com/ruby/ruby/blob/5b8d22ebe60267283a5ca4a1c2ddba507b3d8ba9/lib/getoptlong.rb#L87) as well as some functional programming techniques extracted from an old 1988 textbook:[Introduction to Functional Programming](https://www.amazon.com/Introduction-Functional-Programming-International-Computing/dp/0134841891/) .


Display the current month:


```text
$>   ruby sample/cal.rb  -m
November 2021
M Tu  W Th  F  S  S
1  2  3  4  5  6  7
8  9 10 11 12 13 14
15 16 17 18 19 20 21
22 23 24 25 26 27 28
29 30


```


Display the current month vertically:


```text
$>   ruby sample/cal.rb  -t
November 2021
S     7 14 21 28
M  1  8 15 22 29
Tu  2  9 16 23 30
W  3 10 17 24
Th  4 11 18 25
F  5 12 19 26
S  6 13 20 27


```


Display the whole year:


```text
$>   ruby sample/cal.rb  -y


January               February               March
S  M Tu  W Th  F  S   S  M Tu  W Th  F  S   S  M Tu  W Th  F  S
1  2      1  2  3  4  5  6      1  2  3  4  5  6
3  4  5  6  7  8  9   7  8  9 10 11 12 13   7  8  9 10 11 12 13
10 11 12 13 14 15 16  14 15 16 17 18 19 20  14 15 16 17 18 19 20
17 18 19 20 21 22 23  21 22 23 24 25 26 27  21 22 23 24 25 26 27
24 25 26 27 28 29 30  28                    28 29 30 31
31
April                  May                   June
S  M Tu  W Th  F  S   S  M Tu  W Th  F  S   S  M Tu  W Th  F  S
1  2  3                     1         1  2  3  4  5
4  5  6  7  8  9 10   2  3  4  5  6  7  8   6  7  8  9 10 11 12
11 12 13 14 15 16 17   9 10 11 12 13 14 15  13 14 15 16 17 18 19
18 19 20 21 22 23 24  16 17 18 19 20 21 22  20 21 22 23 24 25 26
25 26 27 28 29 30     23 24 25 26 27 28 29  27 28 29 30
30 31
July                 August              September
S  M Tu  W Th  F  S   S  M Tu  W Th  F  S   S  M Tu  W Th  F  S
1  2  3   1  2  3  4  5  6  7            1  2  3  4
4  5  6  7  8  9 10   8  9 10 11 12 13 14   5  6  7  8  9 10 11
11 12 13 14 15 16 17  15 16 17 18 19 20 21  12 13 14 15 16 17 18
18 19 20 21 22 23 24  22 23 24 25 26 27 28  19 20 21 22 23 24 25
25 26 27 28 29 30 31  29 30 31              26 27 28 29 30


October               November              December
S  M Tu  W Th  F  S   S  M Tu  W Th  F  S   S  M Tu  W Th  F  S
1  2      1  2  3  4  5  6            1  2  3  4
3  4  5  6  7  8  9   7  8  9 10 11 12 13   5  6  7  8  9 10 11
10 11 12 13 14 15 16  14 15 16 17 18 19 20  12 13 14 15 16 17 18
17 18 19 20 21 22 23  21 22 23 24 25 26 27  19 20 21 22 23 24 25
24 25 26 27 28 29 30  28 29 30              26 27 28 29 30 31
31


```


Display the calendar year: 1984


```text
$>   ruby sample/cal.rb  -y   1984


January               February               March
S  M Tu  W Th  F  S   S  M Tu  W Th  F  S   S  M Tu  W Th  F  S
1  2  3  4  5  6  7            1  2  3  4               1  2  3
8  9 10 11 12 13 14   5  6  7  8  9 10 11   4  5  6  7  8  9 10
15 16 17 18 19 20 21  12 13 14 15 16 17 18  11 12 13 14 15 16 17
22 23 24 25 26 27 28  19 20 21 22 23 24 25  18 19 20 21 22 23 24
29 30 31              26 27 28 29           25 26 27 28 29 30 31


April                  May                   June
S  M Tu  W Th  F  S   S  M Tu  W Th  F  S   S  M Tu  W Th  F  S
1  2  3  4  5  6  7         1  2  3  4  5                  1  2
8  9 10 11 12 13 14   6  7  8  9 10 11 12   3  4  5  6  7  8  9
15 16 17 18 19 20 21  13 14 15 16 17 18 19  10 11 12 13 14 15 16
22 23 24 25 26 27 28  20 21 22 23 24 25 26  17 18 19 20 21 22 23
29 30                 27 28 29 30 31        24 25 26 27 28 29 30


July                 August              September
S  M Tu  W Th  F  S   S  M Tu  W Th  F  S   S  M Tu  W Th  F  S
1  2  3  4  5  6  7            1  2  3  4                     1
8  9 10 11 12 13 14   5  6  7  8  9 10 11   2  3  4  5  6  7  8
15 16 17 18 19 20 21  12 13 14 15 16 17 18   9 10 11 12 13 14 15
22 23 24 25 26 27 28  19 20 21 22 23 24 25  16 17 18 19 20 21 22
29 30 31              26 27 28 29 30 31     23 24 25 26 27 28 29
30
October               November              December
S  M Tu  W Th  F  S   S  M Tu  W Th  F  S   S  M Tu  W Th  F  S
1  2  3  4  5  6               1  2  3                     1
7  8  9 10 11 12 13   4  5  6  7  8  9 10   2  3  4  5  6  7  8
14 15 16 17 18 19 20  11 12 13 14 15 16 17   9 10 11 12 13 14 15
21 22 23 24 25 26 27  18 19 20 21 22 23 24  16 17 18 19 20 21 22
28 29 30 31           25 26 27 28 29 30     23 24 25 26 27 28 29
30 31


```


### Learn More About Dates With Ruby


- Be sure to check out the Ruby[Date documentation](https://rubyapi.org/3.1/o/date)
- [Greg Navis](https://twitter.com/gregnavis) recently shared[a lovely example of how to use Date objects for determining various date facts](https://twitter.com/gregnavis/status/1600882033806364672)


---


## Suppress Echo of Terminal Input Using ioctl


[source: cbreak.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/cbreak.rb)


[cbreak.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/cbreak.rb) showcases several bit-wise operators and uses a special[IO#ioctl](https://github.com/ruby/ruby/blob/d92f09a5eea009fa28cd046e9d0eb698e3d94c5c/spec/ruby/core/io/ioctl_spec.rb#L13) method to buffer an empty string in binary to hide ( *not* echo) the input from` STDIN` .


While this may be a clever way to way to hide the input stream, as of Ruby 2.0 you can simply use[IO#noecho](https://rubyapi.org/3.1/o/io#method-i-noecho) to achieve the same result.


---


## Extending PStore For The CGI Session


[source: cgi-session-pstore.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/cgi-session-pstore.rb)


While the days of CGI are behind many of us, the[sample/cgi-session-pstore.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/cgi-session-pstore.rb) is a simple example of using the more impressive[CGI::Session::PStore](https://github.com/ruby/ruby/blob/ruby_3_1/lib/cgi/session/pstore.rb#L23) which extends[PStore](https://rubyapi.org/3.1/o/pstore) to create a file-based persistence mechanism for keeping track of the CGI Session as a Hash.


If you ever find the need for an efficient way to persist data represented as a hash to the file system, check out` PStore` !


### Learn More About PStore In Ruby


- [PStore](https://github.com/ruby/pstore) is now it's own gem!
- [Paweł Pacana](https://twitter.com/pawelpacana) wrote up how they used it to[create a thread-safe persisted cache-store for a faraday plugin](https://blog.arkency.com/practical-use-of-ruby-pstore/) ! Neat!


---


## A Simple TCP Server and Client


Source files used in these examples:


- [source: svr.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/svr.rb)
- [source: clnt.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/clnt.rb)
- [source: tsvr.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/tsvr.rb)


The[svr.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/svr.rb) shows how to start a simple (single-threaded)[TCP Socket](https://en.wikipedia.org/wiki/Transmission_Control_Protocol) server that echoes out the input:


```text
$>   ruby sample/svr.rb
Trying localhost ...  done
addr: AF_INET6:65390:::1:::1
server is on 65390:::1:::1
#<TCPServer:0x00007ff192818bf8> is accepted
#<TCPSocket:0x00007ff192850eb8> is gone


```


You can then connect to this TCP socket using Ruby with the[clnt.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/clnt.rb) :


```text
$>   ruby sample/clnt.rb
Trying localhost ...  done
addr: AF_INET6:65392:::1:::1
peer: AF_INET6:65390:::1:::1
Check
Check


```


A multi-threaded TCP socket server example ([tsvr.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/tsvr.rb) ) is also included!


```text
$>   ruby sample/tsvr.rb
server is on 52276::::::
#<TCPSocket:0x00007f925101a488> is accepted
#<TCPSocket:0x00007f925101a488> is gone


```


With a little adjusting, you could easily stream your logs to a simple locally running TCP socket rather than taking up that valuable disk space 😁


## Test Code Coverage


[source: coverage.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/coverage.rb)


The[sample/coverage.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/coverage.rb) example shows how to use Ruby's native` Coverage` module to analyze and measure what parts of a Ruby program are run. The file by itself doesn't measure anything, so I made a simple` cov.rb` file in the root of the[fast-ruby source](https://github.com/fastruby/fast-ruby) :


```text
# cov.rb
require_relative    '../ruby/sample/coverage.rb'
load    File  .  expand_path  (  './code/string/gsub-vs-tr.rb'  )


```


This runs code coverage on[this gsub-vs-tr.rb benchmark file](https://github.com/fastruby/fast-ruby/blob/ruby_3_1/code/string/gsub-vs-tr.rb) and generated a` ./code/string/gsub-vs-tr.rb.cov` file with the results:


```text
1:    1:require 'benchmark/ips'
-:    2:
1:    3:SLUG = 'writing-fast-ruby'
-:    4:
1:    5:def slow
4454286:    6:  SLUG.gsub('-', ' ')
-:    7:end
-:    8:
1:    9:def fast
13655760:   10:  SLUG.tr('-', ' ')
-:   11:end
-:   12:
1:   13:Benchmark.ips do |x|
4454287:   14:  x.report('String#gsub') { slow }
13655761:   15:  x.report('String#tr')   { fast }
1:   16:  x.compare!
-:   17:end


```


### Learn More About Code Coverage In Ruby


- For a deeper dive on code coverage in Ruby, check out[Kevin Murphy](https://twitter.com/kevin_j_m) 's excellent article,["Ruby's Got You Covered"](https://kevinjmurphy.com/posts/rubys-got-you-covered/) .
- [Jemma Issroff](https://www.twitter.com/JemmaIssroff) has[an excellent article on the importance of both line and branch code coverage in Ruby](https://jemma.dev/blog/ruby-code-coverage) –[Ernesto Tagwerker](http://twitter.com/etagwerker) has[a fantastic breakdown of code coverage reports for the Rails framework](https://www.fastruby.io/blog/rails/what-is-code-coverage-ruby-on-rails.html) .


---


## Delegating Methods With Ease


[source: delegate.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/delegate.rb)


Delegating methods is a great way to keep classes lean. Ruby comes bundled with the[Delegator gem](https://github.com/ruby/delegate) which is highlighted in the[delegate.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/delegate.rb) . Two takeaways here are:


1. You can use[DelegateClass](https://rubyapi.org/3.1/o/object#method-i-DelegateClass) to proxy all methods to the provided class


```text
require    "delegate"
class    SuperArray    <    DelegateClass  (  Array  )
def    initialize  ()
super  ([])
end
end


SuperArray  .  new  .  push    25    # => [25]


```


1. Proxy all methods to an instance:


```text
require    "delegate"
woo    =    Object  .  new
def    woo  .  hoo
puts    "woohoo!"
end
SimpleDelegator  .  new  (  woo  ).  hoo    # => "woohoo!"


```


### Learn More About Delegation In Ruby


- Delegates are a great way to transition legacy objects into new objects that better represent how your application has grown 😁 They are also a common way to[refactor away from inherited classes](https://refactoring.guru/replace-inheritance-with-delegation) .
- Avdi has a wonderful article "[Sustainable Development in Ruby, Part 3: Delegation](https://avdi.codes/sustainable-development-in-ruby-part-3-delegation/) " where he leaves some wisdom on when to consider using delegation and some caveats.


---


## Directory Access


[source: dir.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/dir.rb)


This[dir.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/dir.rb) provides some insight into the powerful[Dir class](https://github.com/ruby/ruby/blob/ruby_3_1/dir.rb) . I modified the example to print out all Ruby files (` *.rb` ) in the root of the Ruby project directory.


```text
# directory access
# list all .rb files
dirp    =    Dir  .  open  (  "."  )
for    f    in    dirp
case    f
when    /\.rb\z/
print    f  ,    "  \n  "
else
# do not print
end
end
dirp  .  close


```


When I ran this, I found some interesting files! Like, what's the story behind[golf_prelude.rb](https://github.com/ruby/ruby/blob/ruby_3_1/golf_prelude.rb) ?


```text
$>   ruby sample/dirb.rb
array.rb
nilclass.rb
numeric.rb
prelude.rb
ast.rb
golf_prelude.rb
ractor.rb
KNOWNBUGS.rb
warning.rb
gem_prelude.rb
io.rb
timev.rb
trace_point.rb
dir.rb
kernel.rb
gc.rb
pack.rb


```


### Learning more about working with directories in Ruby


- [Dir](https://rubyapi.org/3.1/o/dir) is a very powerful feature of Ruby. I recommend checking out more of what it's capable of in the documentation, which has plenty of treats to discover.


---


## DRb: Distributed Object System For Ruby


Source files used in these examples:


- [source: drb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/drb)
- [source: dchats.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/drb/dchats.rb)
- [source: dchatc.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/drb/dchatc.rb)
- [source: dlogd.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/drb/dlogd.rb)
- [source: dlogc.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/drb/dlogc.rb)
- [source: rinda-ring.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/rinda-ring.rb)


If you're as unfamiliar with what a distributed object system is as I was, then you'll find some comfort in this[spectacular documentation on the subject](https://www.druby.org/sidruby/2-1-understanding-distributed-object-systems.html) .


TLDR; with **dRuby** , you can create distributed systems as if you were doing normal Ruby programming.[Twitter originally used dRuby and Rinda](http://www.slideshare.net/Blaine/scaling-twitter) before building its own queuing system, called Starling, in Ruby.


Ruby includes *many* examples of using dRuby in the source, but I'll showcase the most clear use-case I found:


1. [Distributed chat server](https://github.com/ruby/ruby/blob/ruby_3_1/sample/drb/dchats.rb)
2. [Distributed chat client](https://github.com/ruby/ruby/blob/ruby_3_1/sample/drb/dchatc.rb)


With those in place, we can then start a dRuby chat server:


```text
$>   ruby sample/dchats.rb
druby://::1:53383


```


Then connect to the dRuby chat server as myself:


```text
$>   ruby sample/dchatc.rb druby://::1:53383 Valentino
Hi
>  Valentino< Hi


```


And connect another client named **Robo** :


```text
$>   ruby sample/dchatc.rb druby://::1:53383 Robo
Hello
>  Robo< Hello


```


And a small snapshot of our mind-bending conversation from Valentino's client:


```text
Hi
>Valentino< Hi
<Robo> Hello
What is up?
>Valentino< What is up?
<Robo> Nuttin Bot


```


Not the prettiest of chat apps, but the power of dRuby is distribution. The chat clients don't have to be on the same OS, or even the same machine!


### Learn More About Distributed Ruby


The Ruby source has[many other dRuby examples to explore](https://github.com/ruby/ruby/blob/ruby_3_1/sample/drb) . One that I found of interest was[the distributed log server](https://github.com/ruby/ruby/blob/ruby_3_1/sample/drb/dlogd.rb) and[example usage](https://github.com/ruby/ruby/blob/ruby_3_1/sample/drb/dlogc.rb) .


For more dRuby in The Real World™, check out[Masatoshi Seki's enlightening Ruby Kaigi talk](https://rubykaigi.org/2021-takeout/presentations/m_seki.html) where they use dRuby in embedded software for small medical devices.


Be sure to investigate the source of[this embedded drb gem](https://github.com/ruby/drb) .


---


## Dual-stack TCP Server and Client


Source files used in these examples:


- [source: dualstack-httpd.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/dualstack-httpd.rb)
- [source: dualstack-fetch.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/dualstack-fetch.rb)


First, we start up our multi-protocol daemon using the[dualstack-httpd.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/dualstack-httpd.rb) :


```text
$>   ruby sample/dualstack-httpd.rb
socket  #<TCPServer:0x00007facd68303d0> started, address 0.0.0.0 port 8888
socket  #<TCPServer:0x00007facd6833b98> started, address :: port 8888
socket 0.0.0.0 port 8888 listener started, pid 39003 thread  #<Thread:0x00007facd6833300 sample/dualstack-httpd.rb:24 run>
socket :: port 8888 listener started, pid 39003 thread  #<Thread:0x00007facd6832d38 sample/dualstack-httpd.rb:24 run>
socket :: port 8888 accepted, thread  #<Thread:0x00007facd6831dc0 sample/dualstack-httpd.rb:30 run>
socket :: port 8888 got string
socket :: port 8888 processed, thread  #<Thread:0x00007facd6831dc0 sample/dualstack-httpd.rb:30 run> terminating
socket :: port 8888 accepted, thread  #<Thread:0x00007facd6853ab0 sample/dualstack-httpd.rb:30 run>
socket :: port 8888 got string
socket :: port 8888 processed, thread  #<Thread:0x00007facd6853ab0 sample/dualstack-httpd.rb:30 run> terminating


```


Now we can use the[dualstack-fetch.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/dualstack-fetch.rb) example to connect to the server either over IPv4:


```text
$>   ruby sample/dualstack-fetch.rb http://localhost:8888/
conntecting to localhost port 8888
conntected to ::1 port 8888
HTTP/1.0 200 OK
Content-type: text/plain


this is  test  : my name is :: port 8888, you sent:
---start
GET / HTTP/1.0
Host: localhost
---end


```


Or over IPv6:


```text
$>   ruby sample/dualstack-fetch.rb http://:::8888/
conntecting to :: port 8888
conntected to ::1 port 8888
HTTP/1.0 200 OK
Content-type: text/plain


this is  test  : my name is :: port 8888, you sent:
---start
GET / HTTP/1.0
Host: ::
---end


```


### Learn more about IPv6 In Ruby


[Claus Lensbøl](https://twitter.com/lensboel) has[a great series on working with IPv6 in Ruby](https://blog.cmol.me/ipv6-ruby-part-1-introduction-to-ipv6-f45c7cf18151) . He explores both the value of using it, as well as how to provide backwards compatibility for IPv4.


---


## Evaluating Ruby


[source: eval.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/eval.rb)


While there are quite a number of ways to evaluate Ruby code, the most notoriously easy of these is[Kernel.eval](https://rubyapi.org/3.1/o/kernel#method-i-eval) . In just forty lines of code, this Ruby script creates a super simplified REPL reminiscent of IRB, and it even gracefully handles errors!


```text
$>   ruby sample/eval.rb
ruby> puts  "Hello Ruby ❤️"
Hello Ruby ❤️
nil
ruby> class BestLanguageEver
ruby| def is?
ruby| puts  "Ruby, of course!"
ruby| end
ruby| end
:is?
ruby> BestLanguageEver.new.is?
Ruby, of course!
nil
ruby> hug
ERR: undefined  local   variable or method  `  hug ' for main:Object
ruby>


```


It does this by endlessly looping and waiting on` STDIN` for new lines. Once one is entered, it pipes the content of that line through[Kernel 's built-in eval method](https://github.com/ruby/ruby/blob/ruby_3_1/vm_eval.c#L1802) . This incredibly powerful method allows you to evaluate and execute arbitrary strings as if it were Ruby itself. It can even do this[in the context of a specific binding](https://github.com/ruby/ruby/blob/ruby_3_1/vm_eval.c#L1731) (if you pass it one), and you can also provide it with a file name and/or line number to attach to the stack trace in case there are any issues with its execution.


### Learn more about evaluating Ruby code


First and foremost, check out Kevin Newton's incredible series:[Advent of YARV](https://kddnewton.com/2022/11/30/advent-of-yarv-part-0.html) . His multi-part exploration into the CRuby internals is absolutely incredible at describing in great detail all that goes into the CRuby stack.


There are so many great resources out there that offer some great detail into how Ruby executes your code. The gold standard for this is, of course, Pat Shaughnessy's[Ruby Under A Microscope](https://patshaughnessy.net/ruby-under-a-microscope) . This delightful book goes above and beyond to explain in the finest of detail how the YARV stack and the Ruby stack work together to evaluate and execute your Ruby code. In this specific example provided in the Ruby source, it focuses on evaluating Ruby using` Kernel` ; however, there are[several other ways to execute arbitrary ruby code that are outlined in this great article by Jay Fields](https://www.infoq.com/articles/eval-options-in-ruby/) . I also enjoyed this excellent write-up by Ilya Bylich on[Evaluating Ruby in Ruby](https://iliabylich.github.io/2020/01/25/evaluating-ruby-in-ruby.html) that goes step-by-step through this process starting from the instruction sequence and following the frames through the stack using various examples.


---


## Method Access


[source: export.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/export.rb)


This tiny example showcases the power of method access with the` public` and` private` keywords. Now, if you're like me, you had *no idea these can take arguments* ! To make the example more clear as to what is happening, I made a small tweak to this example with some updated naming:


```text
# sample/method_access.rb
class    PrivatePuts
private    :puts
end


begin
PrivatePuts  .  new  .  puts
rescue    NoMethodError    =>    e
printf    "Tisk, tisk: %s  \n  "  ,    e  .  message
end


class    PublicPuts    <    PrivatePuts
def    puts  (  *  args  )
print    "Announcement: "
super
end
end


PublicPuts  .  new  .  puts    "Hello, goodbye."


```


When running this, you can see how we can "export", or provide access to, a private method in a parent-class to a public method in a sub-class.


```text
$>   ruby sample/method_access.rb
Tisk, tisk: private method  `  puts ' called for #<PrivatePuts:0x00007ff0a4833fa0>
Announcement: Hello, goodbye.


```


### Learn More About Method Access In Ruby


"So why is this example called` export.rb` ?" I hear you ask. Well, as it turns out,` public` and` private` (amongst others) are[implemented in the VM with the concept of method visibility](https://github.com/ruby/ruby/blob/ruby_3_1/vm_method.c#L2268) . When these methods are called, they tell the interpreter to "export" the provided method(s) to the VM using[rb_export_method](https://github.com/ruby/ruby/blob/ruby_3_1/vm_method.c#L1596) and tell it what visibility it should have (` public` ,` private` ,` protected` ).


[RubyGuides has a great article on method visibility in Ruby](https://www.rubyguides.com/2018/10/method-visibility/) with further examples within a few different contexts that may interest you.


---


## Explore The Ruby Grammar


[source: exyacc.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/exyacc.rb)


Ruby's entire grammar used to parse Ruby's syntax originates in` parse.y` . We can use this clever example (taken from the Programming Perl book; yes, the one with a camel on it) to print out the grammar stored in it:


```text
$>   ruby sample/exyacc.rb parse.y |  head    -n   28


```


I abbreviated quite a lot of the output for everyone's sanity, but if your curious mind gets to you then you can examine it and discover the excruciating detail of the Ruby language.


```text
program         : top_compstmt
;


top_compstmt    : top_stmts opt_terms
;


top_stmts       : none
| top_stmt
| top_stmts terms top_stmt
| error top_stmt
;


top_stmt        : stmt
| keyword_BEGIN begin_block
;


begin_block     :  '{'   top_compstmt  '}'
;


bodystmt        : compstmt
opt_rescue
k_else
compstmt
opt_ensure
| compstmt
opt_rescue
opt_ensure
;


```


There are two golden Easter Eggs that I found nestled into this lovely example. First, if you look closely at the first line, you'll notice that the shebang declared makes use of two special options available in the` ruby` executable:


```text
#! /usr/local/bin/ruby -Kn


```


1.


` -K \[ kcode\]` This option specifies the multi-byte character set code (e or E for EUC (extended Unix code); s or S for SJIS (Shift-JIS); u or U for UTF8; and a, A, n, or N for ASCII).


2.


` -n` Places code within an input loop (as in` while gets; ... end` ).


The second golden feature nestled in here is the use of` ARGF` .[From the documentation for ARGF](https://rubyapi.org/3.1/o/argf) :


> ARGF is a stream designed for use in scripts that process files given as command-line arguments or passed in via STDIN.


As opposed to the more popular[ARGV](https://rubyapi.org/3.1/o/argv) (for extracting command line *arguments* ), ARGF provides some delightful insight into how Ruby provides a way for easily working with files as arguments! This functionality is[built into Ruby's IO](https://github.com/ruby/ruby/blob/v3_1_2/io.c#L13088) and you can read about how the C code builds an enumerator for iterating over each line in the file(s) (as is done in this example).


### Learn More About Ruby's Grammar


The Ruby Hacking Guide has an incredible section all about the Ruby parser and how Ruby uses its grammar to parse and compile Ruby programs. Anything and everything you'll ever need to know will be in there.


Kevin Newton also has[a fantastic breakdown of how Ruby makes use of Ripper](https://kddnewton.com/2022/02/14/formatting-ruby-part-1.html) (a streaming Ruby parser) to parse Ruby syntax. You can also find the entire history of how Ruby parsing has evolved on his[Parsing Ruby](https://kddnewton.com/parsing-ruby/) site.


---


## Factorial Calculator


[source: fact.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/fact.rb)


This is the first of many examples exploring mathematical concepts and formulas. Adopting the right mathematical model can drastically improve the performance of your systems, as we'll see improving this example 🚀.


In this case, we're talking about[Factorials](https://en.wikipedia.org/wiki/Factorial) . Facto-what? Don't worry, even if you're not familiar, the concept is simple. Given a number` n` , the factorial is what you end up with by multiplying the number while decrementing it's value down to` 1` . This can be expressed as the following formula:


```text
n! = n * (n - 1)!


```


Running the example we get what we expect out of` 5!` :` 5 * 4 * 3 * 2 * 1 = 120`


```text
$>   ruby samples/fact.rb 5
120


```


Now, to support common mathematical concepts and formula like this, Ruby comes with the incredible[Math module](https://rubyapi.org/3.1/o/math) , with a little tweaking of the original example, we can utilize the[Math.gamma method](https://rubyapi.org/3.1/o/math#method-c-gamma) which implements a[Gamma Function](https://en.wikipedia.org/wiki/Gamma_function) (basically the factorial of n - 1). I'll be using the` benchmark-ips` gem to make it easier to understand the comparisons between reports of the benchmark we'll run to see how much we can improve things.


```text
# sample/fact.rb – Updated to use Math.gamma with benchmark
require    'bundler/inline'


gemfile    do
source    'https://rubygems.org'
gem    "benchmark-ips"  ,    require:   false
end
require    "benchmark/ips"


def    fact  (  n  )
return    1    if    n    ==    0
f    =    1
n  .  downto  (  1  )    do    |  i  |
f    *=    i
end
f
end


def    fact_gamma  (  n  )
return    1    if    n    <=    1
n    *    Math  .  gamma  (  n  )
end


Benchmark  .  ips    do    |  x  |
x  .  report  (  "original factorial example"  )    {    fact  (  ARGV  [  0  ].  to_i  )    }
x  .  report  (  "factorial with gamma"  )    {    fact_gamma  (  ARGV  [  0  ].  to_i  )    }
x  .  compare!
end


```


Now if we re-run the script to find the factorial of 1000 (` 1000!` ):


```text
$>   ruby sample/fact.rb 1000
Warming up  --------------------------------------
original factorial example
90.000  i/100ms
factorial with gamma   203.392k i/100ms
Calculating  -------------------------------------
original factorial example
896.596   (  ± 5.7% )   i/s -      4.500k  in     5.036761s
factorial with gamma      2.060M  (  ± 3.9% )   i/s -     10.373M  in     5.044264s


Comparison:
factorial with gamma:  2059772.9 i/s
original factorial example:      896.6 i/s - 2297.33x   (  ± 0.00 )   slower


```


No, you're not reading that wrong. Using` Math.gamma` to calculate factorials is **2,297 times faster** ! With a whopping 2.06 million iterations per second!! Let's make our machine try a little harder and calculate` 100,000!` .


```text
$>   ruby sample/fact.rb 100000
Warming up  --------------------------------------
original factorial example
1.000  i/100ms
factorial with gamma   267.503k i/100ms
Calculating  -------------------------------------
original factorial example
0.112   (  ± 0.0% )   i/s -      1.000   in     8.945673s
factorial with gamma      2.563M  (  ± 3.2% )   i/s -     12.840M  in     5.015354s


Comparison:
factorial with gamma:  2563085.9 i/s
original factorial example:        0.1 i/s - 22928528.45x   (  ± 0.00 )   slower


```


Incredible. At a blazing 2.56 million iterations-per-second, this improvement really pulls away from the original with a **22,928,528x improvement** .


### Learn More About Factorials


"But wait a minute here," I hear you say, "HOW!??". It gets this lovely boost by[storing static values for the first twenty two factorials in a table](https://github.com/ruby/ruby/blob/v3_1_2/math.c#L835) and fetching them as it goes. The rest of the` Math` module is also a great resource and worth exploring what else is in there.


Now that you know how to calculate factorials, there are some really powerful things you can use them for! Factorials are at the foundation of[combinatorics in discrete mathematics](https://en.wikipedia.org/wiki/Discrete_mathematics#Combinatorics) , which explores how discrete structures can be combined or arranged; and they also poke their heads in many areas of statistics. If you ever need to calculate how you can sort your favorite playlist, or figuring out the probability of picking out a blue M&M from the pack.


---


## Fibonacci


[source: fib.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/fib.rb)


At first glance, there's not much to this simple example, but aw shucks am I sucker for a good math problem. It calculates a Fibonacci number at a specific position in the sequence. At the top of the file is a small note by Matz that says, "calculate Fibonacci(2) for benchmark". Naturally, I decided to do some light investigation on calculating Fibonacci and setting up some small benchmarks.


[After some quick research on Wikipedia](https://en.wikipedia.org/wiki/Fibonacci_number) , I was surprised to find that Fibonacci numbers were not discovered by the Italian mathematician, but were first described as early as 200 BC in a work by an Indian poet and mathematician:[Acharya Pingala](https://en.wikipedia.org/wiki/Pingala) on enumerating possible patterns of Sanskrit poetry formed from syllables of two lengths. Truly fascinating; but let's get back to our example.


To get things started, let's take a quick look at the original example:


```text
def    fib  (  n  )
if    n  <  2
n
else
fib  (  n  -  2  )  +  fib  (  n  -  1  )
end
end


```


This function takes an` Integer` **n** and recursively reduces the calculated Fibonacci number at that level. Now, if this is really used in a benchmark, we should try and make this as fast as possible. Let's see what kind of power we can bake into this Bad Mama Jama.


Thanks to[this excellent article from RubyGuides](https://www.rubyguides.com/2015/08/ruby-recursion-and-memoization/) , there are a couple issues with the initial implementation.


Issue 1: Use of recursion on a boundless calculation


As pointed out in the article, Ruby has some limitations on how many times you can recursively call a method (around 7500 depending on your system; and funny enough, I found[an old rejected feature request to allow getting and setting the recursion limit](https://bugs.ruby-lang.org/issues/1218) ). To get around this limitation, it is typical to use an iterative solution.


Issue 2: Lack of caching when reducing calculated


The article also points out that, due to the nature of the calculation, some values are re-calculated as the level of **n** grows. To further improve our example, we can take advantage of caching the levels while we iterate , which gives us throughput at the expense of memory bloat (for higher levels); but hey, at least we're not StackOverflow'in amirite? I've included both an inline cache and external cache example so we can see what kind of improvements we get in both a closed-function setting and one in an encapsulated setting (like creating a` Fibonacci` class :chuckle:)


```text
def    fib_iterative_internal_cache  (  n  )
cache    =    [  0  ,    1  ]
0  .  upto  (  i  )    do    |  i  |
next    if    cache  [  n  ]
cache  [  i  ]    =    i    <    2    ?    i    :    cache  [  i    -    1  ]    +    cache  [  i    -    2  ]
end
cache  [  n  ]
end


@external_cache    =    [  0  ,    1  ]
def    fib_iterative_external_cache  (  n  )
0  .  upto  (  n  )    do    |  i  |
next    if    @external_cache  [  i  ]


@external_cache  [  i  ]    =    i    <    2    ?    i    :    @external_cache  [  i    -    1  ]    +    @external_cache  [  i    -    2  ]
end
@external_cache  [  n  ]
end


```


A quick benchmark and we can see our improvements! Also, we can now feel confident that we won't overflow the interpreter.


```text
# Appended to our above examples
require    "benchmark/ips"


Benchmark  .  ips    do    |  x  |
x  .  report  (  "original fib"  )    {    fib  (  20  )    }
x  .  report  (  "iterative fib (internal cache)"  )    {    fib_iterative_internal_cache  (  20  )    }
x  .  report  (  "iterative fib (external cache)"  )    {    fib_iterative_external_cache  (  20  )    }
x  .  compare!
end


```


Now running our updated example to see the results:


```text
$>   ruby sample/fib.rb
6765
Warming up  --------------------------------------
original fib    55.000  i/100ms
iterative fib  (  internal cache )
15.498k i/100ms
iterative fib  (  external cache )
30.071k i/100ms
Calculating  -------------------------------------
original fib    554.945   (  ± 5.2% )   i/s -      2.805k  in     5.069228s
iterative fib  (  internal cache )
166.978k  (  ± 7.8% )   i/s -    836.892k  in     5.048092s
iterative fib  (  external cache )
291.676k  (  ± 7.2% )   i/s -      1.473M  in     5.082205s


Comparison:
iterative fib  (  external cache )  :   291676.1 i/s
iterative fib  (  internal cache )  :   166977.7 i/s - 1.75x   (  ± 0.00 )   slower
original fib:      554.9 i/s - 525.59x   (  ± 0.00 )   slower


```


A **554x improvement** ! That's what I'm talking about! Although this is a great speed boost, this is math! Shouldn't there be some kind of formula we can take advantage of to just, I don't know, **calculate the solution all at once** . Well, you guessed it, hiding in plain sight, from our trusty Wikipedia article, we can extract two formula:


1. Finding a positive Fibonacci number at position **n** in the sequence


```text
Fn = [( (1 + √5)^n - (1 - √5)^n ) / (2^n × √5)]


```


1. Finding a positive or negative Fibonacci number at position **n** in the sequence


```text
Fn = [( (1 + √5)^n ) / (2^n × √5)]


```


Using these lovely new formulas, we can introduce two new examples that utilize the raw power of the CPU to calculate the values in, what some might say, a "zero-sum game" 😂.


```text
# Appended to our ongoing example


# Positive only equation
# Fn = [( (1 + √5)^n ) / (2^n × √5)]
def    fib_math_positives_only  (  n  )
sqrt_of_five    =    Math  .  sqrt  (  5  )
(
((  1    +    sqrt_of_five  )  **  n  )    /    (  2  **  n    *    sqrt_of_five  )
).  round
end


# Positive and negative equation
# Fn = ( (1 + √5)^n - (1 - √5)^n ) / (2^n × √5)
def    fib_math  (  n  )
sqrt_of_five    =    Math  .  sqrt  (  5  )
(
((  1    +    sqrt_of_five  )  **  n    -    (  1    -    sqrt_of_five  )    **    n  )    /    (  2  **  n    *    sqrt_of_five  )
).  round
end


```


Running our benchmark again on all of our examples:


```text
$>   ruby sample/fib.rb
Warming up  --------------------------------------
original fib    49.000  i/100ms
iterative fib  (  internal cache )
17.331k i/100ms
iterative fib  (  external cache )
31.671k i/100ms
fib math    90.836k i/100ms
fib math  (  positivesonly )
120.339k i/100ms
Calculating  -------------------------------------
original fib    550.045   (  ± 7.3% )   i/s -      2.744k  in     5.023195s
iterative fib  (  internal cache )
166.070k  (  ± 6.7% )   i/s -    831.888k  in     5.034572s
iterative fib  (  external cache )
302.488k  (  ± 8.8% )   i/s -      1.520M  in     5.075258s
fib math    902.943k  (  ± 6.7% )   i/s -      4.542M  in     5.057940s
fib math  (  positivesonly )
1.159M  (  ± 7.4% )   i/s -      5.776M  in     5.017804s


Comparison:
fib math  (  positivesonly )  :  1158712.8 i/s
fib math:   902943.5 i/s - 1.28x   (  ± 0.00 )   slower
iterative fib  (  external cache )  :   302487.6 i/s - 3.83x   (  ± 0.00 )   slower
iterative fib  (  internal cache )  :   166070.4 i/s - 6.98x   (  ± 0.00 )   slower
original fib:      550.0 i/s - 2106.58x   (  ± 0.00 )   slower


```


**WOW!** Using raw computation get's us to a whopping **2106x** improvement!! It *also* busts us out of our high-order memory concern albeit at a much smaller expense of the CPU; and we can save that resource benchmark for another time 😂.


So fun!


---


## Unix Mail From


[source: from.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/from.rb)


This example showcases an implementation of the Unix mail program's "from" sub-command, which takes a list of messages and prints their message headers.


```text
$>   ruby sample/from.rb codenamev
You have no mail.


```


Welp, short example, but I'll try again when I get some mail :-)


---


## Gather Full Paths


[source: fullpath.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/fullpath.rb)


This is a great example showcasing how to take advantage of UNIX Pipes as input to your ruby in scripts! This example takes the output from an` ls -lR` call and doctors up the output to show the full paths of the files!


For clarity of what is happening, I'm sampling the last few lines of the output of` ls -lR` inside of the` sample` directory of the ruby source:


```text
$>    ls    -lR   /Users/codenamev/src/opensource/ruby/sample |  tail
-rw-r--r--    1 codenamev  staff    53 Sep 22 16:57 authors.markdown
-rw-r--r--    1 codenamev  staff   171 Sep 22 16:57 entry.rb
-rw-r--r--    1 codenamev  staff  1851 Sep 22 16:57 remarks.markdown


/Users/codenamev/src/opensource/ruby/sample/trick2018/05-tompng:
total 160
-rw-r--r--    1 codenamev  staff     67 Sep 22 16:57 authors.markdown
-rw-r--r--    1 codenamev  staff   1897 Sep 22 16:57 entry.rb
-rw-r--r--    1 codenamev  staff  66800 Sep 22 16:57 preview_of_output.png
-rw-r--r--    1 codenamev  staff    828 Sep 22 16:57 remarks.markdown


```


The output is trimmed, but we can at least see how this lovely script will give us a more workable list of full paths for all the files in this directory and their sub-directories:


```text
$>    ls    -lR   /Users/codenamev/src/opensource/ruby/sample | ruby sample/fullpath.rb|  tail
-rw-r--r--    1 codenamev  staff  5852 Sep 22 16:57 /Users/codenamev/src/opensource/ruby/sample/trick2018/03-tompng/output.txt
-rw-r--r--    1 codenamev  staff   531 Sep 22 16:57 /Users/codenamev/src/opensource/ruby/sample/trick2018/03-tompng/remarks.markdown
-rw-r--r--    1 codenamev  staff  5661 Sep 22 16:57 /Users/codenamev/src/opensource/ruby/sample/trick2018/03-tompng/trick.png
-rw-r--r--    1 codenamev  staff    53 Sep 22 16:57 /Users/codenamev/src/opensource/ruby/sample/trick2018/04-colin/authors.markdown
-rw-r--r--    1 codenamev  staff   171 Sep 22 16:57 /Users/codenamev/src/opensource/ruby/sample/trick2018/04-colin/entry.rb
-rw-r--r--    1 codenamev  staff  1851 Sep 22 16:57 /Users/codenamev/src/opensource/ruby/sample/trick2018/04-colin/remarks.markdown
-rw-r--r--    1 codenamev  staff     67 Sep 22 16:57 /Users/codenamev/src/opensource/ruby/sample/trick2018/05-tompng/authors.markdown
-rw-r--r--    1 codenamev  staff   1897 Sep 22 16:57 /Users/codenamev/src/opensource/ruby/sample/trick2018/05-tompng/entry.rb
-rw-r--r--    1 codenamev  staff  66800 Sep 22 16:57 /Users/codenamev/src/opensource/ruby/sample/trick2018/05-tompng/preview_of_output.png
-rw-r--r--    1 codenamev  staff    828 Sep 22 16:57 /Users/codenamev/src/opensource/ruby/sample/trick2018/05-tompng/remarks.markdown


```


### Learn More About Piping To Ruby


- [Starr Horne](https://ruby.social/@starr) has an incredible write-up on[Level Up Your Command-Line-Fu With Ruby](https://www.honeybadger.io/blog/ruby-unix-command-line/) . So much to learn. So much to love ❤️
- [Paul Fioravanti](https://www.paulfioravanti.com/) wrote this fairly entertaining piece:[Pipe a Codebase into Ruby](https://www.paulfioravanti.com/blog/pipe-codebase-into-ruby/) .


---


## Saving And Loading Ruby Instruction Sequences


[source: iseq_loader.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/iseq_loader.rb)


The super special[RubyVM::InstructionSequence](https://rubyapi.org/3.1/o/rubyvm/instructionsequence) class is extended in this example to showcase how to compile ruby code into its instruction sequences into a separate file that can then be loaded at a later time.


The[Ruby documentation](https://rubyapi.org/3.1/o/rubyvm/instructionsequence) does an excellent job explaining what this special class is and why you might want to use it:


> The InstructionSequence class represents a compiled sequence of instructions for the Virtual Machine used in MRI. Not all implementations of Ruby may implement this class, and for the implementations that implement it, the methods defined and behavior of the methods can change in any version.
>
>
> With it, you can get a handle to the instructions that make up a method or a proc, compile strings of Ruby code down to VM instructions, and disassemble instruction sequences to strings for easy inspection. It is mostly useful if you want to learn how YARV works, but it also lets you control various settings for the Ruby iseq compiler.


To test this out, I created a` simple_math.rb` file with` 1 + 2` inside. Then, I compiled the instructions for this file into another using this sample script:


```text
$>   ruby sample/iseq_loader.rb simple_math.rb
Writing to file: simple_math.rb, simple_math.rb.yarb


```


Now that I have the compiled instructions, I opened up an IRB session with this example pre-loaded


```text
$>   irb  -r   sample/iseq_loader.rb


```


Then I tested out loading the compiled file:


```text
irb  (  main  ):  001  :  0  >    simple_math_iseq    =    RubyVM  ::  InstructionSequence  ::  FSStorage  .  new  .  load_iseq    "simple_math.rb"
=>    <  RubyVM  ::  InstructionSequence  :<  main  >  @simple_math  .  rb  :  1  >
irb  (  main  ):  002  :  0  >    simple_math_iseq  .  eval
=>    3
irb  (  main  ):  003  :  0  >    simple_math_iseq  .  to_a
=>
[  "YARVInstructionSequence/SimpleDataFormat"  ,
3  ,
1  ,
1  ,
{  :arg_size  =>  0  ,    :local_size  =>  0  ,    :stack_max  =>  2  ,    :node_id  =>  4  ,    :code_location  =>  [  1  ,    0  ,    1  ,    5  ],    :node_ids  =>  [  0  ,    1  ,    3  ,    -  1  ]},
"<main>"  ,
"simple_math.rb"  ,
"/Users/codenamev/src/opensource/ruby/sample/simple_math.rb"  ,
1  ,
:top  ,
[],
{},
[],
[  1  ,    :RUBY_EVENT_LINE  ,    [  :putobject_INT2FIX_1_  ],    [  :putobject  ,    2  ],    [  :opt_plus  ,    {  :mid  =>  :  +  ,    :flag  =>  16  ,    :orig_argc  =>  1  }],    [  :leave  ]]]
irb  (  main  ):  004  :  0  >    ls    simple_math_iseq
RubyVM  ::  InstructionSequence  #methods:
absolute_path     base_label     disasm     disassemble     each_child     eval     first_lineno     inspect     label     path     script_lines     to_a     to_binary     trace_points
=>    nil
irb  (  main  ):  005  :  0  >    simple_math_iseq  .  disasm
=>    "== disasm: #<ISeq:<main>@simple_math.rb:1 (1,0)-(1,5)> (catch: FALSE)  \n  0000 putobject_INT2FIX_1_                                             (   1)[Li]  \n  0001 putobject                              2  \n  0003 opt_plus                               <calldata!mid:+, argc:1, ARGS_SIMPLE>[CcCr]  \n  0005 leave  \n  "


```


Neat! With this, you can peek into the internals of CRuby's VM. So, how is this useful? Well, let's say you run into some Ruby code that is doing something you don't expect. With` RubyVM::InstructionSequence` , you can peek into what is happening!


I was recently helping a co-worker debug an issue dynamically building keys for a hash. I had thought that the keys were getting set as strings, but as it turns out, this particular notation of Ruby makes them symbols:


```text
irb  (  main  ):  001  :  0  >    {    "thing1"  :    1  ,    "thing2"  :    2    }
=>    {  :thing1  =>  1  ,    :thing2  =>  2  }
irb  (  main  ):  002  :  0  >


```


Using our trusty new tool, I disassembled the compiled CRuby code to see what was happening:


```text
irb  (  main  ):  003  :  0  >    RubyVM  ::  InstructionSequence  .  compile  (  '{ "thing1": 1, "thing2": 2 }'  ).  disasm
=>    "== disasm: #<ISeq:<compiled>@<compiled>:1 (1,0)-(1,28)> (catch: FALSE)  \n  0000 duphash                                {:thing1=>1, :thing2=>2}  (   1)[Li]  \n  0002 leave  \n  "


```


Ah ha! While I thought it was using strings, the CRuby VM is building the hash with symbols! I was able to confirm this by performing this same test with the old "hashrocket" Hash notation:


```text
irb  (  main  ):  004  :  0  >    RubyVM  ::  InstructionSequence  .  compile  (  '{ "thing1" => 1, "thing2" => 2 }'  ).  disasm
=>    "== disasm: #<ISeq:<compiled>@<compiled>:1 (1,0)-(1,32)> (catch: FALSE)  \n  0000 duphash                                {  \"  thing1  \"  =>1,   \"  thing2  \"  =>2}(   1)[Li]  \n  0002 leave  \n  "
irb  (  main  ):  005  :  0  >    {    "thing1"    =>    1  ,    "thing2"    =>    2    }
=>    {  "thing1"  =>  1  ,    "thing2"  =>  2  }


```


Good times.


### Learn More About Ruby's VM


- Make sure to check out[Kevin Newton's incredible Advent of YARV](https://kddnewton.com/2022/11/30/advent-of-yarv-part-0.html) for an in-depth look at CRuby's virtual machine and how you can make use of these instruction sequences!
- For some fun, join Stefanni Brasil and Thiago Araujo over at[HexDevs](https://www.hexdevs.com/) combine forces with Aaron Patterson in this great breakdown:[A Tender Introduction to Ruby Internals with TenderJIT](https://www.hexdevs.com/posts/ruby-just-in-time-compiler-tenderjit/)


---


## A Front-end For The Less Command


[source: less.rb](https://github.com/ruby/ruby/blob/ruby_3_1/sample/less.rb)


This nifty little script allows you to combine the powers of[less](https://man7.org/linux/man-pages/man1/less.1.html) and[zcat](https://man7.org/linux/man-pages/man1/zcat.1p.html) to properly dump the contents of a file to one of these system pagers without having to care about whether it's compressed or not. I had to alter the paths to match my system, but with that I was able to play around with some options:


```text
$>   ruby sample/less.rb  -NR    -p   mental  --use-color   sample/biorhythm.rb


```


This throws me into the less utility at the first occurrence of the pattern I provided ("mental"):


```text
102   print "                     P=physical, E=emotional, M=mental\n"
103   print "             -------------------------+-------------------------\n"
104   print "                     Bad Condition    |    Good Condition\n"
105   print "             -------------------------+-------------------------\n"
106
107   (dd - bd).step(dd - bd + display_period) do |z|
108     phys, emot, geist = get_position(z)
109
110     printf "%04d.%02d.%02d : ", dd.year, dd.month, dd.day
111     p = (phys / 2.0 + 0.5).to_i
112     e = (emot / 2.0 + 0.5).to_i
113     g = (geist / 2.0 + 0.5).to_i
114     graph = "." * 51
115     graph[25] = ?|
116     graph[p] = ?P
117     graph[e] = ?E
118     graph[g] = ?M
119     print graph, "\n"
120     dd = dd + 1
121   end
122   print "             -------------------------+-------------------------\n\n"
biorhythm.rb


```


I gzip'ed the biorhythm.rb file and updated the example to use` gzcat` (because of[this issue with running zcat on MacOS](https://serverfault.com/questions/570024/zcat-gzcat-works-in-linux-not-on-osx-general-linux-osx-compatibility) ). Running the same command on the new compressed file:


```text
$>   ruby sample/less.rb  -NR    -p   mental  --use-color   sample/biorhythm.rb.gz


```


And I get the same output above! Niiiiiice.


---


## Wrapping it up


Whew! That was a lot to uncover, but there is SO much more ahead of us. I've said it before, and I'll say it again! The Ruby source is not so scary! It's not all C code. There's quite a lot of **Ruby** code inside. And while it may be nice to reach for those external gems and tools, chances are you're missing out on what's already there.


---


Be sure to follow[@doximity_tech](https://twitter.com/doximity_tech) if you'd like to be notified about new blog posts.
