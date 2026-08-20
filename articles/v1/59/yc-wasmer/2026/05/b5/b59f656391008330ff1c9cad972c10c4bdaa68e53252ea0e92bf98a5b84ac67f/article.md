---
schema_version: "1.0.0"
document_id: "b59f656391008330ff1c9cad972c10c4bdaa68e53252ea0e92bf98a5b84ac67f"
company_key: "yc-wasmer"
company: "Wasmer"
source_id: "yc-wasmer-news-import-0c661488008f"
canonical_url: "https://wasmer.io/posts/how-webassembly-is-powering-wordpress"
published_at: "2026-05-20T13:33:00+00:00"
first_seen_at: "2026-07-22T19:24:02.861490+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:5197bbc8dd1c50f79431b16ae710e5b3fa2dc06fed7521fcc254363f9de19b33"
---

# How WebAssembly is powering WordPress

In less than a year,[WordPress.com](http://wordpress.com/) released WP Playground and WP Studio.


But …did you know that both **WordPress Playground** (the in-browser WordPress playground) and **WP Studio** (a local dev app by WordPress) leverage WebAssembly to run WordPress without needing a PHP server or MySQL database?


The entire WordPress stack (PHP runtime, web server logic, and database) runs in a fully-sandboxed web environment, either in a browser or an Electron-based desktop app thanks to WebAssembly.


> 👉 If you are not familiar with WebAssembly, we recommend taking a look at this video: “WebAssembly on 100 seconds”:[https://www.youtube.com/watch?v=cbB3QEwWMlA](https://www.youtube.com/watch?v=cbB3QEwWMlA)


In this article we will explore the technical implementation details: how WordPress is executed via WebAssembly, the key components involved, performance considerations, how filesystem/database operations work, and the main drawbacks (like reliance on SQLite and performance gaps with native execution).


## Running WordPress via WebAssembly


Instead of rewriting WordPress itself to JavaScript to run in the browser, the simplest solution was to compile the PHP *interpreter* (along with needed extensions) to WebAssembly, as WebAssembly can run seamlessly in the browser. That assures full compatibility with any existing PHP program.


The WordPress Playground project built a custom pipeline to compile the PHP engine (C source) using **Emscripten** (a C/C++-to-Wasm toolchain) to WebAssembly:[wordpress.github.io](https://wordpress.github.io/wordpress-playground/developers/architecture/wasm-php-overview/#:~:text=WebAssembly%20PHP) .


The result of compiling PHP to WebAssembly produces a` .wasm` binary (for example,` php-8.2.wasm` for PHP 8.2) that can be loaded in the browser or any Javascript environment. WordPress’s PHP code (the core, plugins, themes) remains exactly the same. The compiled PHP interpreter in *Wasm* reads and executes those scripts at runtime, just like PHP would on a server.


> 👉 Note: Using Emscripten is not the only way to compile PHP to WebAssembly. At Wasmer we also released[WASIX](https://wasix.org/) , which sets a universal system call layer for interacting with WebAssembly and allows running the programs both in the browser and the server. Read further to see the advantages and drawbacks of each strategy.


To launch WordPress, the environment needs all the usual files and a database. WordPress Playground distributes pre-packaged WordPress releases optimized for the browser: core files are bundled into a small ZIP (around 5MB compressed, down from ~70MB). This bundle includes a pre-configured` wp-config.php` set to use SQLite (since MySQL isn’t available) and even has the installation steps pre-run to simplify the creation process.


In the browser, the Playground code fetches the ZIP for the requested WordPress version, unpacks it into an **in-memory virtual filesystem** , and then invokes the PHP-Wasm binary to execute WordPress. Essentially, the browser becomes a self-contained server: “PHP is compiled to WebAssembly (PHP-WASM), MySQL is replaced by SQLite, and server functionality is replaced by JavaScript APIs”.


When you navigate the Playground’s demo site, an iframe with a Javascript Service worker captures WordPress’s generated HTML and displays it, making it feel like a normal browsing experience even though WordPress is running inside a sandboxed WebAssembly VM in your browser.


## Key WebAssembly Components and Architecture


Several WebAssembly-related technologies make this possible:


- **[Emscripten](https://emscripten.org/)** : The PHP interpreter (and its C extensions) are compiled to Wasm using Emscripten. Emscripten provides the low-level *libc* , file I/O, and other OS emulation layers that PHP needs. The Playground team created a custom build pipeline for PHP, enabling support for features like the PHP CLI SAPI and needed extensions (SQLite, OpenSSL, libxml, etc.) in the Wasm binary. They also use Emscripten’s **[Asyncify](https://kripken.github.io/blog/wasm/2019/07/16/asyncify.html)** feature in certain builds to deal with blocking PHP calls. For example, in Node.js, Asyncify is used so that synchronous socket functions in PHP can work with async JS APIs (by pausing the Wasm execution while JS completes an operation)
- **WebAssembly Runtimes** : In browsers, the Wasm module runs on the built-in WebAssembly engine (like Chrome’s V8 or Firefox’s SpiderMonkey). These engines JIT-compile WebAssembly to native machine code for near-native speed execution.
- **Service Workers** : In the browser WordPress Playground, a combination of web technologies orchestrate the WordPress experience. A Service Worker is used to intercept requests (like when WordPress in the iframe tries to load an image or navigate to a new page) and serve the responses from the in-memory WordPress instance, as if it were a web server. The WordPress admin or site frontend is loaded in an` <iframe>` , and the Playground’s JS infrastructure makes sure any request in that iframe (e.g. to` wp-admin/admin-ajax.php` or a page link) is routed to the PHP/Wasm for processing, then the HTML output is delivered back. This is how WordPress can appear to function normally, despite no actual server – the browser itself is *pretending* to be the server using the Playground’s coordination logic


# Major Drawbacks


The current approach used in WordPress Playground and WP Studio has some major drawbacks:


- Emscripten only makes WordPress work in browser-like environments (like WordPress Playground being used in the browser and WordPress Studio using Electron), leaving **server-side** usage unsupported.
- **Performance** : compiling PHP to WebAssembly could involve a 3-6x slowdown when executing PHP programs if not done efficiently. In the current build of PHP by WordPress, OpCache is not enabled in the Wasm build of PHP that is powering WordPress Playground and WordPress Studio. So you will see a significant slowdown when running WordPress using Playground or WordPress studio if you try to use some heavy-weight plugins (like Elementor or Woocommerce).
- **WP Cron** doesn’t work on WordPress Playground or WordPress studio, which means that many plugins (such as SimplyStatic and more) will not work in your setup.
- **SQLite** : to not require the user to have a MySQL server installed (and because of direct socket connections are currently impossible from browser environments), they replaced MySQL with SQLite. However, SQLite is not fully compatible with MySQL. That means that some plugins will simply not work on the WP Playground and Studio.


## How Wasmer solved the drawbacks


Thankfully, at Wasmer we solved each of these drawbacks to allow a near-native experience when running and hosting WordPress via Wasmer and WebAssembly:


-


Thanks to compiling PHP to WebAssembly using[WASIX](https://wasix.org/) instead of Emscripten, we can now run WordPress on servers, in the browser and even embedded, without changing any setup. This is the first universal container of WordPress that can run **seamlessly on the server** or in a browser.


-


**Performance** : at Wasmer we enabled maximum performance when running PHP. We have done it in various ways:


- Thanks to using Wasm Exceptions (instead of Asyncify) for longjmp/setjmp calls used by PHP, we **improved the runtime speed by 2x** .
- Thanks to our usage of[OPCache in WebAssembly](https://wasmer.io/posts/running-php-blazingly-fast-at-the-edge-with-wasm) , we **improved the runtime speed by 3x** .
- Thanks to using[Wasmer’s Instaboot](https://wasmer.io/posts/announcing-instaboot-instant-cold-starts-for-serverless-apps) , we **improved the startup speed by 10x** when compared to other Cloud Providers


We have detailed more of this on our last presentation in Wasm I/O 2025:[Video](https://www.youtube.com/watch?v=DgLZF8eubLI) (slides:[https://speakerdeck.com/syrusakbary/making-php-extremely-fast-in-webassembly](https://speakerdeck.com/syrusakbary/making-php-extremely-fast-in-webassembly) )


-


**Cron jobs** : Wasmer added support for cron jobs out of the box, this allows running wp-cron jobs without an issue.


-


**SQLite** : as WASIX allows direct socket connections, you can now run WordPress with MySQL via WebAssembly. Thanks to that we can allow reusing exactly the same code from all the plugins.


With all the drawbacks solved, you now can benefit of a great WordPress hosting in Wasmer that runs fast, is safe and supports the whole world of WordPress.


# One last thing…


At Wasmer, we have been pioneering WebAssembly in server side and browser environments since our inception.


Thanks to our unique technology, we can now run PHP at near-native speeds on the server.


But we want this technology to be available for everyone, as it allows us to offer incredibly fast hosting at affordable prices.


When you deploy new WordPress sites with Wasmer, you will be automatically using WebAssembly under the hood, and will get all the speed improvements for free.


Deploy and serve your WordPress sites for free and experience it yourself!


[https://wasmer.io/wordpress-hosting](https://wasmer.io/wordpress-hosting)
