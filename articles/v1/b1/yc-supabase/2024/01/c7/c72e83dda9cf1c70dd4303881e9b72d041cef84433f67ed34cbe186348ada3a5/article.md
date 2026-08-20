---
schema_version: "1.0.0"
document_id: "c72e83dda9cf1c70dd4303881e9b72d041cef84433f67ed34cbe186348ada3a5"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/elixir-clustering-using-postgres"
published_at: "2024-01-09T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T22:26:19.240569+00:00"
content_hash: "sha256:80d54f713ddd569f977a1574ed96e6655ae67ba5af173341f883823a7b2a2ba3"
---

# Elixir clustering using Postgres

Elixir offers a powerful feature by allowing multiple nodes to communicate between them without extra services in the middle, reducing the overall complexity of your system.


However, when it comes to connecting the servers, there seems to be a barrier of entry that many people encounter, including ourselves, on how to provide the name discovery required to connect said servers. We have released our approach to solving this problem by open-sourcing[libcluster Postgres Strategy](https://github.com/supabase/libcluster_postgres) and today, we explore the motivations behind its creation and the methodologies employed in its development.


## Why do we need a distributed Erlang Cluster?#


At Supabase, we use clustering in all of our Elixir projects which include[Logflare](https://supabase.com/docs/guides/database/extensions/wrappers/logflare) ,[Supavisor](https://supabase.com/blog/supavisor-postgres-connection-pooler) and[Realtime](https://supabase.com/docs/guides/realtime) . With multiple servers connected, we can load shed, create globally distributed services, and provide the best service to our customers so we’re closer to them geographically and to their instances, reducing overall latency.


To achieve a connected cluster, we wanted to be as cloud-agnostic as possible. This makes our self-hosting options more accessible. We don’t want to introduce extra services to solve this single issue - Postgres is the logical way to achieve it.


The other piece of the puzzle was already built by the Erlang community being the defacto library to facilitate the creation of connected Elixir servers:[libcluster](https://github.com/bitwalker/libcluster) .


## What is libcluster?#


[libcluster](https://github.com/bitwalker/libcluster) is the go-to package for connecting multiple BEAM instances and setting up healing strategies. libcluster provides out-of-the-box strategies and it allows users to define their own strategies by implementing a simple behavior that defines cluster formation and healing according to the supporting service you want to use.


## How did we use Postgres?#


Postgres provides an event system using two commands:[NOTIFY](https://www.postgresql.org/docs/current/sql-notify.html) and[LISTEN](https://www.postgresql.org/docs/current/sql-listen.html) so we can use them to propagate events within our Postgres instance.


To use these features, you can use psql itself or any other Postgres client. Start by listening on a specific channel, and then notify to receive a payload.


`
_10


postgres=# LISTEN channel;


_10


LISTEN


_10


postgres=# NOTIFY channel, 'payload';


_10


NOTIFY


_10


Asynchronous notification "channel" with payload "payload" received from server process with PID 326.


`


Now we can replicate the same behavior in Elixir and[Postgrex](https://hex.pm/packages/postgrex) within IEx (Elixir's interactive shell).


`
_16


Mix.install(\[{:postgrex, "~> 0.17.3"}\])


_16


config = \[


_16


hostname: "localhost",


_16


username: "postgres",


_16


password: "postgres",


_16


database: "postgres",


_16


port: 5432


_16


\]


_16


{:ok, db_notification_pid} = Postgrex.Notifications.start_link(config)


_16


Postgrex.Notifications.listen!(db_notification_pid, "channel")


_16


{:ok, db_conn_pid} = Postgrex.start_link(config)


_16


Postgrex.query!(db_conn_pid, "NOTIFY channel, 'payload'", \[\])


_16


_16


receive do msg -> IO.inspect(msg) end


_16


# Mailbox will have a message with the following content:


_16


# {:notification, #PID<0.223.0>, #Reference<0.57446457.3896770561.212335>, "channel", "test"}


`


## Building the strategy#


Using the libcluster` Strategy` behavior, inspired by[this GitHub repository](https://github.com/kevbuchanan/libcluster_postgres) and knowing how` NOTIFY/LISTEN` works, implementing a strategy becomes straightforward:


1. We send a` NOTIFY` to a channel with our` node()` address to a configured channel


`
_21


# lib/cluster/strategy/postgres.ex:52


_21


def handle_continue(:connect, state) do


_21


with {:ok, conn} <- Postgrex.start_link(state.meta.opts.()),


_21


{:ok, conn_notif} <- Postgrex.Notifications.start_link(state.meta.opts.()),


_21


{_, _} <- Postgrex.Notifications.listen(conn_notif, state.config\[:channel_name\]) do


_21


Logger.info(state.topology, "Connected to Postgres database")


_21


_21


meta = %{


_21


state.meta


_21


| conn: conn,


_21


conn_notif: conn_notif,


_21


heartbeat_ref: heartbeat(0)


_21


}


_21


_21


{:noreply, put_in(state.meta, meta)}


_21


else


_21


reason ->


_21


Logger.error(state.topology, "Failed to connect to Postgres: #{inspect(reason)}")


_21


{:noreply, state}


_21


end


_21


end


`


1. We actively listen for new` {:notification, pid, reference, channel, payload}` messages and connect to the node received in the payload


`
_16


# lib/cluster/strategy/postgres.ex:80


_16


def handle_info({:notification, _, _, _, node}, state) do


_16


node = String.to_atom(node)


_16


_16


if node != node() do


_16


topology = state.topology


_16


Logger.debug(topology, "Trying to connect to node: #{node}")


_16


_16


case Strategy.connect_nodes(topology, state.connect, state.list_nodes, \[node\]) do


_16


:ok -> Logger.debug(topology, "Connected to node: #{node}")


_16


{:error, _} -> Logger.error(topology, "Failed to connect to node: #{node}")


_16


end


_16


end


_16


_16


{:noreply, state}


_16


end


`


1. Finally, we configure a heartbeat that is similar to the first message sent for cluster formation so libcluster is capable of heal if need be


`
_10


# lib/cluster/strategy/postgres.ex:73


_10


def handle_info(:heartbeat, state) do


_10


Process.cancel_timer(state.meta.heartbeat_ref)


_10


Postgrex.query(state.meta.conn, "NOTIFY #{state.config\[:channel_name\]}, '#{node()}'", \[\])


_10


ref = heartbeat(state.config\[:heartbeat_interval\])


_10


{:noreply, put_in(state.meta.heartbeat_ref, ref)}


_10


end


`


These three simple steps allow us to connect as many nodes as needed, regardless of the cloud provider, by utilizing something that most projects already have: a Postgres connection.


## Conclusion#


In this post, we have described our approach to connecting multiple nodes in Elixir using Postgres. We have also made this strategy available for anyone to use. Please check the code at[github.com/supabase/libcluster_postgres](https://github.com/supabase/libcluster_postgres)


A special thank you to[@gotbones](https://twitter.com/gotbones) for creating libcluster and[@kevinbuch_](https://twitter.com/kevinbuch_) for the original inspiration for this strategy.


## More Supabase Realtime#


- [Realtime docs](https://supabase.com/docs/guides/realtime)
- [Realtime: Multiplayer Edition](https://supabase.com/blog/supabase-realtime-multiplayer-general-availability)
- [Video - How to subscribe to real-time changes on your database](https://www.youtube.com/watch?v=2rUjcmgZDwQ)
- [Video - Listening to real-time changes on the database with Flutter and Supabase](https://www.youtube.com/watch?v=gboTC2lcgzw)
