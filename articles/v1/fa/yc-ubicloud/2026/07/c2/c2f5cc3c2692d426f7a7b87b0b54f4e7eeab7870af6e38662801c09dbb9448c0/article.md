---
schema_version: "1.0.0"
document_id: "c2f5cc3c2692d426f7a7b87b0b54f4e7eeab7870af6e38662801c09dbb9448c0"
company_key: "yc-ubicloud"
company: "Ubicloud"
source_id: "yc-ubicloud-news-import-c10303752e5c"
canonical_url: "https://www.ubicloud.com/blog/how-to-keep-documentation-screenshots-up-to-date-using-ruby"
published_at: null
first_seen_at: "2026-07-24T05:09:51.579811+00:00"
fetched_at: "2026-07-28T21:39:52.838477+00:00"
content_hash: "sha256:a074e6941ce205908e85a2c7e648f31f7dfe3a6e144f55877e00b0e84866194d"
---

# How to Keep Documentation Screenshots Up-to-date Using Ruby

## How to Keep Documentation Screenshots Up-to-date Using Ruby


November 26, 2025 · 4 min read


Jeremy Evans


Principal Software Engineer


Keeping the documentation up to date is one of the hardest problems with documenting. Whenever you make a change to the product, you need to check if you also need to update the documentation. This gets tricky when a change impacts several parts of the documentation. It's even harder if the sections needing updates aren't clear. Screenshots serve as a clear example of this problem. If the application's look changes, then all related screenshots need updating. This is why you see outdated screenshots when reviewing technical documentation.


The best way to avoid outdated screenshots in documentation is to set up automatic updates for all screenshots. That way, whenever a change is made to how the application looks, you can run a single command to regenerate all screenshots. In this post, I’ll explain how we tackle this issue at Ubicloud. We want to make sure that viewers of our documentation don’t see outdated screenshots.


It may not be obvious, but automating screenshot generation is like automating tests in many aspects. One aspect is motivation. You run automated tests to catch and fix regressions before they hit production. You also create screenshots automatically to replace outdated ones in your documentation. Another aspect is implementation. Automated screenshot generation is very similar to automated integration tests. Instead of using assertions, you navigate to pages and take screenshots.


Ubicloud's automated integration tests differ from screenshot generation. Unlike the latter, Ubicloud's tests don't use a real browser; they rely on rack-test, which is a browser simulator. Using rack-test is faster than using a real browser. Rack-test skips tasks that a real browser must handle, like creating a visual display of pages.


However, to take screenshots, you need to use a real browser. A great ruby library named ferrum offers a way to automate the use of Chrome. There’s another ruby library called cuprite. It builds on ferrum and lets you automate the use of Chrome using the same API used in automated integration tests. With cuprite, developers familiar with Capybara can quickly update the screenshot generation code. This is simple to do whenever new screenshots are added to the documentation.


Here's how we set up the use of cuprite with capybara. We need the capybara and cuprite libraries. Then, we set capybara to use cuprite as its default driver:


```text
require   "capybara"
require   "capybara/dsl"
require   "capybara/cuprite"


PORT =   8383


Capybara.exact = true
Capybara.default_selector = :css
Capybara.default_driver = :cuprite
Capybara.server_port = PORT
Capybara.register_driver(:cuprite) do |app|
Capybara::Cuprite::Driver.new(app, window_size: [  1200  ,   800  ], browser_options: {timeout:   15  }, base_url:   "http://localhost:#{PORT}"  )
end
```


Unfortunately, there are some challenging differences when using cuprite instead of rack-test. With rack-test, requests to the application are made similarly to method calls, and occur in the same thread that the tests use. As cuprite uses a real browser, the browser runs as a separate process, and must connect to a server. The server needs to be able to respond to the browser's requests, and must use a different thread or process than the tests use.


Here's how we set up the separate thread for the server. As in production, we'll use puma as the server, but unlike production, we'll limit puma to a single thread and process. As the screenshot generation code runs in the main thread, we must start the server in a separate thread. Since the server starts in a separate thread, we must wait for it to be ready before generating the screenshot. We'll use a Queue to synchronize access. The server will signal the main thread to continue once it has booted:


```text
require   "puma/cli"
require   "nio"


queue = Queue.new
server = Puma::CLI.new([  "-s"  ,   "-e"  ,   "test"  ,   "-b"  ,   "tcp://localhost:#{PORT}"  ,   "-t"  ,   "1:1"  ,   "config.ru"  ])
server.launcher.events.on_booted { queue.push(nil) }
Thread.new do
server.launcher.run
end
queue.pop


```


When running automated tests, keep it simple. Use a transactional approach for screenshot generation. This way, any changes made to the test database will be rolled back after the screenshots are created. With rack-test, this approach is simple by using a transaction around each test. As long as each test uses the same thread, all database queries use the same connection, inside the same transaction. With cuprite, or any library that uses a real browser and separate server thread/process, this becomes challenging. Ubicloud relies on Sequel for database access. Sequel supports sharing the same connection safely between the main thread and the server thread.


We needed to adjust Ubicloud's Sequel setup a little. This forces Sequel to use just one connection when creating screenshots. In the screenshot generation code, we set an environment variable as a flag before we load Sequel:


```text
ENV[  "SHARED_CONNECTION"  ] =   "1"
```


We then modified the Sequel setup to use a single connection if that environment variable is set:


```text
max_connections =   1     if   ENV[  "SHARED_CONNECTION"  ] ==   "1"
```


We also enabled the use of Sequel's temporarily_release_connection extension if that environment variable is set:


```text
DB.extension :temporarily_release_connection   if   ENV[  "SHARED_CONNECTION"  ] ==   "1"
```


In the screenshot generation code, we wrap all screenshot creation in a single transaction. This transaction will automatically roll back when we exit. We also use savepoints for any transaction calls within the block. By default, a transaction in Sequel assigns the connection to the main thread. But to share this connection with the server thread, we release it back to the connection pool. This way, both the main and server threads can safely share access:


```text
DB.transaction(rollback: :always, auto_savepoint: true) do |conn|
DB.temporarily_release_connection(conn) do
RegenScreenshots.new.call
end
end
```


Finally, we get to the screenshot generation code. As shown above, we'll be using a RegenScreenshots class for the screenshot generation code:


```text
class     RegenScreenshots
#   code     in     examples     below
end


```


We want to make sure that the screenshot generation code regenerates all screenshots used in the documentation. For simplicity, the documentation saves all screenshots in a single directory, so we look at all files in that directory, and create a hash of screenshots:


```text
SCREENSHOT_DIR =   "../documentation/screenshots/"
SCREENSHOTS = Dir.children(SCREENSHOT_DIR).sort.  map   { |f| [f, true] }.to_h


```


Every time we take a screenshot, we provide the screenshot name, and we remove this screenshot from the hash. The actual screenshot is taken with save_screenshot, which is a capybara method that will ask the driver (cuprite in this case) to have the browser take the screenshot.


```text
def     screenshot  (  name  )
filename   = "#{  name  }.  png  "
path   =   File  .  join  (  SCREENSHOT_DIR, filename  )
save_screenshot  (  path:  )
puts   "  Saved     screenshot  :     #{name}"
SCREENSHOTS.delete(filename)
end


```


Then we write the code to create the necessary screenshots. This uses the normal capybara DSL to navigate between pages, taking screenshots whenever needed:


```text
include Capybara::DSL
def     call
visit   "/"
screenshot  (  "login"  )


click_link   "  Create     a     new     account  "
screenshot  (  "create_account"  )


password   =   SecureRandom  .  base64  (  48  )
fill_in   "  Full     Name  ",   with  :     "Demo"
fill_in   "Email Address"  ,   with  :   " [email protected]  "
fill_in   "Password"  ,   with  : password
fill_in   "Password Confirmation"  ,   with  : password
click_button   "Create Account"
screenshot(  "post_create_account"  )


# ...
end
```


At the end of the program, we check whether all screenshots used in the documentation have been regenerated, and if not, we issue a warning, so the developer regenerating the screenshots knows they need to update the screenshot program to include generation of that screenshot:


```text
unless RegenScreenshots::SCREENSHOTS.empty?
warn   "Missing screenshots:"  , RegenScreenshots::SCREENSHOTS.keys.sort
end
```


This automated screenshot generation program helps Ubicloud keep its documentation up to date. This way, it is easier for viewers to use the screenshots in the documentation. We have used this approach to keep our documentation screenshots up to date for the past 12 months.


Next up


[Quick Start - Build your own cloud](https://www.ubicloud.com/docs/quick-start/build-your-own-cloud)
