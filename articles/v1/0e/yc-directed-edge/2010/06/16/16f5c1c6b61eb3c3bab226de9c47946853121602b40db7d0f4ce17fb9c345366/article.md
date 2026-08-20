---
schema_version: "1.0.0"
document_id: "16f5c1c6b61eb3c3bab226de9c47946853121602b40db7d0f4ce17fb9c345366"
company_key: "yc-directed-edge"
company: "Directed Edge"
source_id: "yc-directed-edge-rss-7c8e8fe81473"
canonical_url: "https://blog.directededge.com/2010/06/05/using-taps-without-running-a-taps-server/"
published_at: "2010-06-06T04:50:18+00:00"
first_seen_at: "2026-07-27T01:55:50.875550+00:00"
fetched_at: "2026-08-20T02:30:11.821086+00:00"
content_hash: "sha256:638633c54087864c081e545704ea2a841e09ce67a9047fbcd8733251ddc71b66"
---

# Using taps without running a taps server

So, the Ruby world has a nifty thinger for syncing up databases over the interwebs. It’s called[Taps](http://adam.heroku.com/past/2009/2/11/taps_for_easy_database_transfers/) , from the superheroes over at[Heroku](http://heroku.com/) . It’s great — you just run a little Sinatra-based server and then give the database URLs and it handles all of the plumbing.


But, you see, I was none-too-keen on having another long-running Ruby process, not to mention an open port with production database data lumbering around on it, so I thought I’d let you guys in on a little hack we produced internally to let you get all of the fun of taps, but without the taps server.


Basically it starts up the taps server on the remote server, tunnels the transfer over SSH, then sends a ctrl-c to the server to kill it’s done. It’s pull-only very intentionally — I want to push from a development database to a production database about like I want a hole in my head.


```text
#!/usr/bin/env ruby


require 'rubygems'
require 'active_support/secure_random'
require 'net/ssh'


SSH_USER = '[sshuser]'
SSH_HOST = '[dbhost]'


LOCAL_DB = 'mysql://[dbuser]:[dbpass]@localhost/[dbname]'
REMOTE_DB = 'mysql://[dbuser]:[dbpass]@localhost/[dbname]'


TAPS_USER = ActiveSupport::SecureRandom.hex(16)
TAPS_PASS = ActiveSupport::SecureRandom.hex(16)


URL = "http://#{TAPS_USER}:#{TAPS_PASS}@localhost:5000"


Net::SSH.start(SSH_HOST, SSH_USER, :compression => true) do |ssh|
ssh.forward.local(5000, 'localhost', 5000)


ready = false


channel = ssh.open_channel do |c|
c.request_pty
c.on_data { |c, data| ready = true if data =~ /port=5000/ }
c.exec("taps server #{REMOTE_DB} #{TAPS_USER} #{TAPS_PASS}")
end


finished = false


Thread.new do
sleep 0.1 until ready
system "taps pull #{LOCAL_DB} #{URL}"
finished = true
end


ssh.loop(0.1) do
channel.send_data(Net::SSH::Connection::Term::VINTR) if finished
!finished
end
end


```


Substitute in the right values in the constants up at the top and you’ve got a nifty way to securely use taps without leaving a server running.
