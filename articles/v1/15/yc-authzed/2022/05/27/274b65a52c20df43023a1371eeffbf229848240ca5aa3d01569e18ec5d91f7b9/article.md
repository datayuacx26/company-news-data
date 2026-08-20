---
schema_version: "1.0.0"
document_id: "274b65a52c20df43023a1371eeffbf229848240ca5aa3d01569e18ec5d91f7b9"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/automatic-release-notification"
published_at: "2022-05-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:03:24.473366+00:00"
content_hash: "sha256:10238ddeb38536eaef75433c32a53a840dd26b6eb77fee7ddaad8c43d95b173f"
---

# Automatic release notification in SpiceDB and zed

Keeping infrastructure software up to date has traditionally been a manual process: it was often part of somone's job description to check whether a new version of the software was available, verify its compatibility and deploy the update. However, this process has an Achilles heel: if the person (or people) responsible for checking for updates fails to do so, older versions of the software can languish.


Want to see the code behind this blog post? Take a look at


[the releases package in SpiceDB](https://github.com/authzed/spicedb/tree/main/pkg/releases)


and


[the version module in cobrautil](https://github.com/jzelinskie/cobrautil/blob/main/version.go)


Given the importance and benefits of keeping the software supply chain up to date, the Authzed team embarked on a small project recently to make it absolutely clear when updates for[SpiceDB](https://github.com/authzed/spicedb) are available.


This blog post briefly discusses how[SpiceDB](https://github.com/authzed/spicedb) and[zed](https://github.com/authzed/zed) check for new versions and display that state to the end-user. The code used here is generally useful to any Go program that needs similar functionality.


## The release process


[SpiceDB](https://github.com/authzed/spicedb) is Authzed's open source,[Zanzibar](https://authzed.com/blog/what-is-zanzibar/) inspired database for fine-grained permissions. Releases of SpiceDB are found[on GitHub](https://github.com/authzed/spicedb/releases) , with built images automatically pushed to[various](https://hub.docker.com/r/authzed/spicedb)[container registries](https://quay.io/authzed/spicedb) .


Using GitHub to publish releases has made the process for SpiceDB nearly seamless. The Authzed team quickly realized, however, that it left one major item to be desired: notification. While we did discover that GitHub provides an[atom feed of available versions](https://github.com/authzed/spicedb/releases.atom) , the technology is no longer widely used, and as far as we knew, there was no way to easily indicate (outside of notifications in the[SpiceDB Discord](https://authzed.com/discord) ) to users of SpiceDB that new versions were available.


As new versions of SpiceDB can result in major performance improvements, new features, and even fix[security issues](https://authzed.com/blog/using-github-to-manage-your-first-cve/) , we felt it was important to make the availability of new versions as transparent to end users as possible.


Thus we embarked on a project to make both SpiceDB and zed display to the end user if the currently running version of SpiceDB can be updated.


## Looking up releases


The first step in this process was to allow SpiceDB or zed to determine the latest version available. Fortunately for us, the[GitHub API](https://docs.github.com/en/rest) provides a` releases` endpoint which lists all available releases for a project, as well as providing an endpoint returning just the latest release.


Even **more** fortunate was the discovery of a[golang GitHub API client](https://github.com/google/go-github) , which allowed for extremely easy interaction with the GitHub API.


Getting the latest released version of SpiceDB, therefore, became[a simple GetLatestRelease function call](https://github.com/authzed/spicedb/blob/79fc52198fc9733b5f97d6f9ce4b0448bd0b3928/pkg/releases/releases.go#L31) :


go


1


2


```text
client := github.NewClient( nil  )
release, _, err := client.Repositories.GetLatestRelease(ctx,  "authzed"  ,  "spicedb"  )


```


go


1


2


```text
client := github.NewClient( nil  )


```


However, there was one issue with this endpoint: the latest release endpoint returns the last **published** non-prerelease release. If SpiceDB publishes a point release of an **older** version (say to backfill a bug fix), the older version will be reported as the latest released version! To counter this issue, we have instituted a policy to publish newer versions of the latest release in this scenario to ensure the real latest release is always returned. Suggestion to the GitHub API team: add an ability to sort by semantic version as well!


## Getting the version of the binary


Once we have the latest released version, the next step was to retrieve the binary version for comparison.


Fortunately, we were again lucky that most of the infrastructure we needed had already been built: by setting the version into a build flag in Go, we were able to retrieve it at runtime. Furthermore, Go 1.18 provides the ability to access the git SHA used to build the binary **automatically** , which we were able to use as a fallback.


All of the above abilities were packaged into the[cobrautil](https://github.com/jzelinskie/cobrautil/blob/main/version.go) , the package SpiceDB uses to reduce CLI boilerplate package, so retrieving the version of the binary was[similarly easy to do](https://github.com/authzed/spicedb/blob/79fc52198fc9733b5f97d6f9ce4b0448bd0b3928/pkg/releases/versions.go#L14) :


go


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


```text
import   (
"runtime/debug"
"github.com/jzelinskie/cobrautil"
)


// CurrentVersion returns the current version of the binary.
func    CurrentVersion  ()    ( string  ,  error  ) {
bi, ok := debug.ReadBuildInfo()
if   !ok {
return    ""  , fmt.Errorf( "failed to read BuildInfo because the program was compiled with Go %s"  , runtime.Version())
}


return   cobrautil.VersionWithFallbacks(bi),  nil
}


```


go


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


```text
import   (
"runtime/debug"
"github.com/jzelinskie/cobrautil"
)


// CurrentVersion returns the current version of the binary.
func    CurrentVersion  ()    ( string  ,  error  ) {
bi, ok := debug.ReadBuildInfo()
if   !ok {
}


return   cobrautil.VersionWithFallbacks(bi),  nil
}


```


## Comparing versions


With the ability to retrieve the latest version, and the current version, the next step was to define a means for SpiceDB to compare versions. Fortunately, SpiceDB's versions (roughly) correspond to the use of[semver](https://semver.org/) , making the comparison between versions simple.


We were able, therefore, to define[a CheckIsLatestVersion function for comparing versions](https://github.com/authzed/spicedb/blob/79fc52198fc9733b5f97d6f9ce4b0448bd0b3928/pkg/releases/versions.go#L42) of SpiceDB:


go


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


17


18


19


20


21


22


23


24


25


26


27


28


29


30


31


```text
func    CheckIsLatestVersion  (
ctx context.Context,
getCurrentVersion  func  ()    ( string  ,  error  ),
getLatestRelease  func  (context.Context)    (*Release,  error  ),
) (SoftwareUpdateState,  string  , *Release,  error  ) {


currentVersion, err := getCurrentVersion()
if   err !=  nil   {
return   Unknown, currentVersion,  nil  , err
}


if   currentVersion ==  ""   || !semver.IsValid(currentVersion) {
return   UnreleasedVersion, currentVersion,  nil  ,  nil
}


release, err := getLatestRelease(ctx)
if   err !=  nil   {
return   Unknown, currentVersion,  nil  , err
}


if   !semver.IsValid(release.Version) {
return   Unknown, currentVersion,  nil  , err
}


if   semver.Compare(currentVersion, release.Version) <  0   {
return   UpdateAvailable, currentVersion, release,  nil
}


return   UpToDate, currentVersion, release,  nil


}


```


go


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


17


18


19


20


21


22


23


24


25


26


27


28


29


30


31


```text
func    CheckIsLatestVersion  (
ctx context.Context,
getCurrentVersion  func  ()    ( string  ,  error  ),
getLatestRelease  func  (context.Context)    (*Release,  error  ),
) (SoftwareUpdateState,  string  , *Release,  error  ) {


currentVersion, err := getCurrentVersion()
if   err !=  nil   {
return   Unknown, currentVersion,  nil  , err
}


if   currentVersion ==  ""   || !semver.IsValid(currentVersion) {
return   UnreleasedVersion, currentVersion,  nil  ,  nil
}


release, err := getLatestRelease(ctx)
if   err !=  nil   {
return   Unknown, currentVersion,  nil  , err
}


if   !semver.IsValid(release.Version) {
return   Unknown, currentVersion,  nil  , err
}


if   semver.Compare(currentVersion, release.Version) <  0   {
return   UpdateAvailable, currentVersion, release,  nil
}


return   UpToDate, currentVersion, release,  nil


}


```


Note an essential detail of this function: it takes in function closures to retrieve the latest and current versions, allowing it to elide some calls when not necessary. It also provides an easy means of testing by faking those calls.


## Reporting to the user in SpiceDB


SpiceDB could now be designed to emit whether the running version was behind the latest available version with the comparison and loading functions complete.


The check was added via the use of a new pre-run function[CheckAndLogRunE](https://github.com/authzed/spicedb/blob/a2e9e40fae53b49a6fb183aa0da338011e1eaba6/pkg/releases/cli.go#L20) which (unless disabled by the` --skip-release-check` flag) grabs the latest release version from GitHub and compares it to the current version, logging if the version is out of date, or the version being run is not a released version:


sh


1


```text
3:00PM WRN this version of SpiceDB is out of  date  . See: https://github.com/authzed/spicedb/releases/tag/v1.7.1  this-version=1.7.0 latest-released-version=1.7.1


```


sh


1


```text


```


## Reporting to the user in zed


Adding version checking to zed presented a slightly larger challenge: zed could easily make use of the` CheckIsLatestVersion` function for comparison, as it was included in the published` releases` package. Likewise, zed could also use` ​​GetLatestRelease` to retrieve the latest version of SpiceDB.


However, zed could not use` GetCurrentVersion` , as that would return the current version of **zed** instead of SpiceDB. Thus, to support this use case, we needed to add a way for SpiceDB to return the running version to the caller of its APIs.


Fortunately, SpiceDB's API is implemented via gRPC, which has the concept of **middleware** for injecting custom logic into both client and server operations.


To add support for returning the current version,[a new server side middleware](https://github.com/authzed/spicedb/pull/572) was added to SpiceDB:


go


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


17


18


19


20


21


22


23


24


25


26


27


28


29


30


31


32


33


34


35


36


37


38


39


40


41


42


43


44


45


46


47


48


49


```text
import   (
"context"


"github.com/authzed/authzed-go/pkg/requestmeta"
"github.com/authzed/authzed-go/pkg/responsemeta"
"github.com/grpc-ecosystem/go-grpc-middleware/v2/interceptors"
"github.com/rs/zerolog/log"
"google.golang.org/grpc"
"google.golang.org/grpc/metadata"


"github.com/authzed/spicedb/pkg/releases"
)


type   handleServerVersion  struct   {
isEnabled  bool
}


func    (r *handleServerVersion)    ServerReporter(ctx context.Context, _ interceptors.CallMeta) (interceptors.Reporter, context.Context) {
if   r.isEnabled {
if   md, ok := metadata.FromIncomingContext(ctx); ok {
if   _, isRequestingVersion := md[ string  (requestmeta.RequestServerVersion)]; isRequestingVersion {
version, err := releases.CurrentVersion()
if   err !=  nil   {
log.Ctx(ctx).Err(err).Msg( "could not load current software version"  )
return   interceptors.NoopReporter{}, ctx
}


err = responsemeta.SetResponseHeaderMetadata(ctx,  map  [responsemeta.ResponseMetadataHeaderKey] string  {
responsemeta.ServerVersion: version,
})
if   err !=  nil   {
log.Ctx(ctx).Err(err).Msg( "could not report metadata"  )
}
}
}
}


return   interceptors.NoopReporter{}, ctx
}


// UnaryServerInterceptor returns a new interceptor which handles server version requests.
func    UnaryServerInterceptor  (isEnabled  bool  )    grpc.UnaryServerInterceptor {
return   interceptors.UnaryServerInterceptor(&handleServerVersion{isEnabled})
}


// StreamServerInterceptor returns a new interceptor which handles server version requests.
func    StreamServerInterceptor  (isEnabled  bool  )    grpc.StreamServerInterceptor {
return   interceptors.StreamServerInterceptor(&handleServerVersion{isEnabled})
}


```


go


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


17


18


19


20


21


22


23


24


25


26


27


28


29


30


31


32


33


34


35


36


37


38


39


40


41


42


43


44


45


46


47


48


49


```text
import   (
"context"


"github.com/authzed/authzed-go/pkg/requestmeta"
"github.com/authzed/authzed-go/pkg/responsemeta"
"github.com/grpc-ecosystem/go-grpc-middleware/v2/interceptors"
"github.com/rs/zerolog/log"
"google.golang.org/grpc"
"google.golang.org/grpc/metadata"


"github.com/authzed/spicedb/pkg/releases"
)


type   handleServerVersion  struct   {
isEnabled  bool
}


if   r.isEnabled {
if   md, ok := metadata.FromIncomingContext(ctx); ok {
version, err := releases.CurrentVersion()
if   err !=  nil   {
log.Ctx(ctx).Err(err).Msg( "could not load current software version"  )
return   interceptors.NoopReporter{}, ctx
}


responsemeta.ServerVersion: version,
})
if   err !=  nil   {
log.Ctx(ctx).Err(err).Msg( "could not report metadata"  )
}
}
}
}


return   interceptors.NoopReporter{}, ctx
}


return   interceptors.UnaryServerInterceptor(&handleServerVersion{isEnabled})
}


return   interceptors.StreamServerInterceptor(&handleServerVersion{isEnabled})
}


```


This middleware checks a specialized header on incoming requests, and, if present (and the middleware is not disabled via a flag), returns the server's current version in a gRPC response header.


Likewise, to perform the check on the zed side,[a new client side middleware](https://github.com/authzed/zed/pull/114) was used:


go


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


17


18


19


20


21


22


23


24


25


26


27


28


29


30


31


32


33


34


35


36


37


38


39


40


41


42


43


44


45


46


47


48


49


50


51


52


53


54


55


56


```text
import   (
"context"
"time"


"github.com/authzed/authzed-go/pkg/requestmeta"
"github.com/authzed/authzed-go/pkg/responsemeta"
"github.com/rs/zerolog/log"
"google.golang.org/grpc"
"google.golang.org/grpc/metadata"


"github.com/authzed/spicedb/pkg/releases"
)


func    CheckServerVersion  (
ctx context.Context,
method  string  ,
req, reply  interface  {},
cc *grpc.ClientConn,
invoker grpc.UnaryInvoker,
callOpts ...grpc.CallOption,
)     error   {
var   headerMD metadata.MD
ctx = requestmeta.AddRequestHeaders(ctx, requestmeta.RequestServerVersion)
err := invoker(ctx, method, req, reply, cc,  append  (callOpts, grpc.Header(&headerMD))...)
if   err !=  nil   {
return   err
}


version := headerMD.Get( string  (responsemeta.ServerVersion))
if    len  (version) ==  0   {
log.Debug().Msg( "error reading server version response header; it may be disabled on the server"  )
}  else    if    len  (version) ==  1   {
currentVersion := version[ 0  ]


rctx, cancel := context.WithTimeout(context.Background(), time.Second* 2  )
defer   cancel()


state, _, release, cerr := releases.CheckIsLatestVersion(rctx,  func  ()    ( string  ,  error  ) {
return   currentVersion,  nil
}, releases.GetLatestRelease)
if   cerr !=  nil   {
log.Debug().Err(cerr).Msg( "error looking up currently released version"  )
}  else   {
switch   state {
case   releases.UpdateAvailable:
log.Warn().Str( "this-version"  , currentVersion).Str( "latest-released-version"  , release.Version).Msgf( "the version of SpiceDB being called is out of date. See: %s"  , release.ViewURL)
return    nil


// … more cases handled here …
}
}
}


return   err
}


```


go


1


2


3


4


5


6


7


8


9


10


11


12


13


14


15


16


17


18


19


20


21


22


23


24


25


26


27


28


29


30


31


32


33


34


35


36


37


38


39


40


41


42


43


44


45


46


47


48


49


50


51


52


53


54


55


56


```text
import   (
"context"
"time"


"github.com/authzed/authzed-go/pkg/requestmeta"
"github.com/authzed/authzed-go/pkg/responsemeta"
"github.com/rs/zerolog/log"
"google.golang.org/grpc"
"google.golang.org/grpc/metadata"


"github.com/authzed/spicedb/pkg/releases"
)


func    CheckServerVersion  (
ctx context.Context,
method  string  ,
req, reply  interface  {},
cc *grpc.ClientConn,
invoker grpc.UnaryInvoker,
callOpts ...grpc.CallOption,
)     error   {
var   headerMD metadata.MD
ctx = requestmeta.AddRequestHeaders(ctx, requestmeta.RequestServerVersion)
if   err !=  nil   {
return   err
}


version := headerMD.Get( string  (responsemeta.ServerVersion))
if    len  (version) ==  0   {
}  else    if    len  (version) ==  1   {
currentVersion := version[ 0  ]


rctx, cancel := context.WithTimeout(context.Background(), time.Second* 2  )
defer   cancel()


return   currentVersion,  nil
}, releases.GetLatestRelease)
if   cerr !=  nil   {
log.Debug().Err(cerr).Msg( "error looking up currently released version"  )
}  else   {
switch   state {
case   releases.UpdateAvailable:
return    nil


// … more cases handled here …
}
}
}


return   err
}


```


The client side middleware sends the header to the server and, if it receives back a response with the server's version, logs the result of a call` CheckIsLatestVersion` .


## Final thoughts


Adding release version checking and notification to SpiceDB and zed was a reasonably straightforward engineering project, but important design decisions allowed the library to be reusable, both for zed and others.


However, automatic release notification is only the first step to automating the running of SpiceDB.


[Sign up for our mailing list](http://eepurl.com/hEeb6z) to receive monthly SpiceDB and Authzed product updates and keep your eyes on this blog for more information about new developments!


Want to be notified directly when new releases of SpiceDB are available?[Join the SpiceDB Discord](https://authzed.com/discord) to receive a direct notification whenever a new release is made!


## Additional Reading


If you’re interested in learning more about Authorization and Google Zanzibar, we recommend reading the following posts:


- [Understanding Google Zanzibar: A Comprehensive Overview](https://authzed.com/blog/what-is-google-zanzibar)
- [A Primer on Modern Enterprise Authorization (AuthZ) Systems](https://authzed.com/blog/authz-primer)
- [Fine-Grained Access Control: Can You Go Too Fine?](https://authzed.com/blog/fine-grained-access-control)
- [Relationship Based Access Control (ReBAC): Using Graphs to Power your Authorization System](https://authzed.com/blog/exploring-rebac)
- [Pitfalls of JWT Authorization](https://authzed.com/blog/pitfalls-of-jwt-authorization)


On this page


- The release process
- Looking up releases
- Getting the version of the binary
- Comparing versions
- Reporting to the user in SpiceDB
- Reporting to the user in zed
- Final thoughts
- Additional Reading


## Related


[Engineering SpiceDB v1.39 Released The SpiceDB v1.39 release delivers enhanced monitoring through native histograms, smarter health checks, and transaction metadata improvement. Dec 20, 2024 · 3 min](https://authzed.com/blog/spicedb-v1-39-released)[Engineering SpiceDB v1.39 Released The SpiceDB v1.39 release delivers enhanced monitoring through native histograms, smarter health checks, and transaction metadata improvement. Sam Kim · Dec 20, 2024 · 3 min](https://authzed.com/blog/spicedb-v1-39-released)


[Engineering LookupSubjects and SpiceDB v1.12.0 Product Updates for July & August Sep 20, 2022 · 3 min](https://authzed.com/blog/lookup-subjects)[Engineering LookupSubjects and SpiceDB v1.12.0 Product Updates for July & August Joey Schorr · Sep 20, 2022 · 3 min](https://authzed.com/blog/lookup-subjects)


[Engineering Defining Systems Lucidly Authzed's new configuration language Jul 14, 2021 · 4 min](https://authzed.com/blog/defining-systems-lucidly)[Engineering Defining Systems Lucidly Authzed's new configuration language Joey Schorr · Jul 14, 2021 · 4 min](https://authzed.com/blog/defining-systems-lucidly)
