---
schema_version: "1.0.0"
document_id: "98a11d815392d1f445adbfb383c98bb2d13b99865fddbb33a688edb5a7108bda"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/exploring-support-tooling"
published_at: "2024-04-25T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:f8458e4ae95e0235da4319dfeedf00e55e50eba47c1bf8b292f8176072098004"
---

# Exploring Support Tooling at Supabase: A Dive into SLA Buddy

## Introduction#


In database management and support operations, ensuring Service Level Agreement (SLA) compliance is paramount. Supabase, known for its innovative approach to database management and support, introduces SLA Buddy, a robust support tool aimed at efficient SLA enforcement. This blog post delves into the intricacies of SLA Buddy, shedding light on its functions, operations, and interactions within the Supabase ecosystem.


### Introducing SLA Buddy#


Supabase's commitment to innovation extends beyond database solutions; it encompasses robust support operations. SLA Buddy stands as a testament to Supabase's dedication to streamlining support processes and ensuring timely resolution of user queries.


## Dogfooding: The Birth of SLA Buddy#


Supabase firmly believes in dogfooding a philosophy that entails using one's own products internally. This approach played a pivotal role in the creation of SLA Buddy. Leveraging Supabase's suite of tools, including Edge Functions and Database functionalities, SLA Buddy was meticulously developed to meet the stringent demands of support operations.


## Understanding SLA Buddy's Functions#


SLA Buddy's core function revolves around enforcing SLAs effectively. Let's delve into its primary functions:


### SLA Enforcement#


SLA Buddy ensures SLA compliance through a series of intricate processes. This includes:


- **Slack Reminders** : Utilizing Slack reminders to prompt support engineers about impending SLA deadlines.
- **Calendar Checks** : Employing calendar integration to determine who's currently available to answer support tickets.


## Let's take a look at SLA Buddy's Operations#


To gain a deeper understanding of SLA Buddy's operations, let's take a look on the main diagram of operations:


### Watching Messages#


SLA Buddy actively monitors Slack channels using PostgreSQL functions like **` process_channels`** . This function scans Slack channels, handles new messages, and adds tasks to the queue for each new ticket that comes to the platform. Once the channel is scanned through the scan_channel edge function it adds rows to the` slack_watcher` table. There is a trigger function on that table that creates tasks for each ticket according to the SLA which depends on which channel that the message came from. Tickets have different SLAs, depending on both severity and the subscription level of the user opening the ticket.


`
_45


CREATE OR REPLACE FUNCTION "public"."insert_tasks"() RETURNS "trigger"


_45


LANGUAGE "plpgsql"


_45


AS $$


_45


declare


_45


escalationtimeintervals int\[\];


_45


currentinterval int;


_45


threadts text;


_45


_45


BEGIN


_45


IF new.channel_id <> '' THEN


_45


SELECT escalation_time INTO escalationtimeintervals


_45


FROM priority WHERE channel_id = new.channel_id;


_45


ELSE


_45


escalationtimeintervals := array\[10, 20, 35, 50\]; -- minutes


_45


END IF;


_45


-- INSERT tasks for each escalation level


_45


FOR i IN 1..4


_45


LOOP


_45


-- set the current escalation time interval


_45


currentinterval := escalationtimeintervals\[i\];


_45


-- format thread_ts as (epoch time as a big int) + '.' + ts_ms


_45


thread_timestamp := extract(epoch FROM new.ts)::bigint::text || '.' || new.ts_ms;


_45


_45


-- check IF ticket_type is not 'feedback'


_45


IF lower(new.ticket_type) <> 'feedback' THEN


_45


INSERT INTO checking_tasks_queue (http_verb, payload, due_time, replied)


_45


values (


_45


'POST',


_45


jsonb_build_object(


_45


'channel_id', new.channel_id,


_45


'thread_ts', thread_timestamp,


_45


'escalation_level', i,


_45


'ticket_id', new.ticket_number,


_45


'ticket_priority', new.ticket_priority,


_45


'ticket_type', new.ticket_type


_45


),


_45


new.ts + (currentinterval * interval '1 minute'),


_45


false


_45


);


_45


END IF;


_45


END LOOP;


_45


-- return the new slack_msg row


_45


return new;


_45


END;


_45


$$;


`


### **Verifying Due Tasks**#


The core function **` check_due_tasks_and_update()`** plays a pivotal role in task verification and status updating. It ensures that tasks are duly acknowledged, thereby facilitating timely resolution.


`
_90


CREATE OR REPLACE FUNCTION "public"."check_due_tasks_and_update"() RETURNS "void"


_90


LANGUAGE "plpgsql"


_90


AS $$


_90


DECLARE


_90


_task RECORD;


_90


_response JSONB;


_90


_response_row JSONB;


_90


_ticket_id text;


_90


_have_replied BOOLEAN;


_90


_ticket_array text;


_90


_lock_key CONSTANT int := 42;


_90


_lock_acquired boolean;


_90


BEGIN


_90


-- Try to acquire the advisory lock


_90


_lock_acquired := pg_try_advisory_lock(_lock_key);


_90


IF NOT _lock_acquired THEN


_90


RAISE NOTICE 'Could not acquire lock. Another instance is running. Exiting function...';


_90


RETURN;


_90


END IF;


_90


_90


-- Call create_ticket_array()


_90


RAISE NOTICE 'Calling create_ticket_array()';


_90


_ticket_array := public.create_ticket_array();


_90


_90


-- Check IF _ticket_array is '\[\]'


_90


IF _ticket_array = '\[\]' THEN


_90


RAISE NOTICE 'No tickets to process. Exiting function...';


_90


-- Release the advisory lock


_90


PERFORM pg_advisory_unlock(_lock_key);


_90


RETURN;


_90


END IF;


_90


_90


-- Call help_plataform_wrapper() using _ticket_array


_90


RAISE NOTICE 'Calling help_plataform_wrapper()';


_90


_response := public.help_plataform_wrapper(_ticket_array);


_90


_90


-- Check IF _response is NULL


_90


IF _response IS NULL THEN


_90


RAISE NOTICE 'Response is NULL. Exiting function...';


_90


-- Release the advisory lock


_90


PERFORM pg_advisory_unlock(_lock_key);


_90


RETURN;


_90


END IF;


_90


_90


-- Process the response


_90


FOR _response_row IN SELECT * FROM jsonb_array_elements(_response)


_90


LOOP


_90


_ticket_id := _response_row->>'ticket_id';


_90


_have_replied := (_response_row->>'have_replied')::BOOLEAN;


_90


RAISE NOTICE 'Processing response for ticket_id: %, have_replied: %', _ticket_id, _have_replied;


_90


IF _have_replied THEN


_90


RAISE NOTICE 'Ticket % has a reply. Updating...', _ticket_id;


_90


-- Perform actions for replied tickets


_90


UPDATE public.checking_tasks_queue


_90


SET replied_at = NOW(), replied = TRUE


_90


WHERE payload->>'ticket_id' = _ticket_id;


_90


ELSE


_90


RAISE NOTICE 'Ticket % has no reply. Taking actions...', _ticket_id;


_90


-- Perform actions for no reply


_90


SELECT * INTO _task FROM public.checking_tasks_queue


_90


WHERE payload->>'ticket_id' = _ticket_id AND status = '' AND due_time <= NOW()


_90


ORDER BY due_time ASC


_90


LIMIT 1;


_90


_90


IF FOUND THEN


_90


RAISE NOTICE 'Sending Slack notification for ticket %', _ticket_id;


_90


-- Use EXCEPTION to handle duplicate keys


_90


BEGIN


_90


INSERT INTO post_to_slack_log(payload) VALUES (_task.payload);


_90


PERFORM slack_post_wrapper(_task.payload);


_90


EXCEPTION


_90


WHEN unique_violation THEN


_90


RAISE NOTICE 'Duplicate entry for ticket %. Skipping...', _ticket_id;


_90


WHEN OTHERS THEN


_90


RAISE NOTICE 'Error while inserting into post_to_slack_log. Skipping...';


_90


RAISE NOTICE '% %', SQLERRM, SQLSTATE;


_90


END;


_90


-- Update the status to 'sent' after calling slack_post_wrapper


_90


UPDATE public.checking_tasks_queue


_90


SET status = 'sent'


_90


WHERE id = _task.id;


_90


ELSE


_90


RAISE NOTICE 'Task for ticket % not found!', _ticket_id;


_90


END IF;


_90


END IF;


_90


END LOOP;


_90


-- Release the advisory lock


_90


PERFORM pg_advisory_unlock(_lock_key);


_90


END;


_90


$$;


`


### Posting SLA Enforcement Messages on Slack#


SLA Buddy employs the Edge Function **` post_ticket_escalation`** to post SLA enforcement messages on Slack. This integration with PostgreSQL functions ensures streamlined execution and effective communication with support engineers.


## Interactions with Support Members#


SLA Buddy fosters seamless interactions between support engineers and the tool itself. Through Slack threads, support members can postpone the next steps in the escalation process by 30 min by` @mentioning` the bot in the thread. We also pushed a[guide on how to interact with mentions](https://supabase.com/docs/guides/functions/examples/slack-bot-mention) in Slack as part of the bot's development.


The bot won't get disarmed until a response is sent in the ticket because we believe that even if the Support Engineer is unable to help the user, they can at least triage and set expectations for the next steps in the ticket like escalating to a specific team.


## Watching Support Events#


Another crucial aspect of SLA Buddy is its ability to monitor support events seamlessly. At Supabase we have the concept of Embedded Support when a member of the support team will work on more advanced tickets related to a specific Supabase product such as Edge Functions, Dashboard, Storage, Auth, Realtime etc.


The shift information about Support Engineers is hosted in a Google Calendar. This information is retrieved using the following function:


`
_35


CREATE OR REPLACE FUNCTION "public"."get_embedded_event_names"


_35


("date_param" timestamp with time zone DEFAULT "now"())


_35


RETURNS "jsonb"


_35


LANGUAGE "plpgsql" SECURITY DEFINER


_35


SET "search_path" TO ''


_35


AS $$


_35


DECLARE


_35


target_date timestamp with time zone := COALESCE(date_param, now());


_35


start_date timestamp with time zone := target_date + INTERVAL '2 hours';


_35


end_date timestamp with time zone := start_date + INTERVAL '1 day' - INTERVAL '1 millisecond';


_35


time_min text := to_char(start_date, 'YYYY-MM-DD"T"HH24:MI:SS.MS"Z"');


_35


time_max text := to_char(end_date, 'YYYY-MM-DD"T"HH24:MI:SS.MS"Z"');


_35


base_url text;


_35


api_url text;


_35


response jsonb;


_35


events jsonb; -- Change the declaration to jsonb


_35


embedded_event_names text\[\];


_35


BEGIN


_35


SELECT decrypted_secret


_35


INTO base_url


_35


FROM vault.decrypted_secrets


_35


WHERE name = 'calendar_base_url';


_35


_35


api_url := base_url || '&timeMin=' || time_min || '&timeMax=' || time_max;


_35


_35


select "content"::jsonb into response from extensions.http_get(api_url);


_35


events := response->'items'; -- Remove the typecast to ::jsonb


_35


_35


SELECT ARRAY_AGG(event->>'summary')


_35


INTO embedded_event_names


_35


FROM jsonb_array_elements(events) AS event -- Use jsonb_array_elements function


_35


WHERE (event->>'summary') ILIKE '%embedded%';


_35


RETURN COALESCE(to_jsonb(embedded_event_names)::text,'\[\]');


_35


END;


_35


$$;


`


**Escalation Logic**


SLA Buddy's escalation logic is defined in 4 steps of escalation going from a more narrow set of Support Engineers to the Head of Success. Here's the progression:


Target Level Action Timeline


Enterprise 1 Non-embedded support 10 min


2 On-shift support 20 min


3 @group-support 35 min


4 @head of success 50 min


Team 1 Non-embedded support 1 hour


2 On-shift support 3 hours


3 @group-support 6 hours


4 @head of success 12 hours


## Conclusion#


SLA Buddy is a core operational component for Supabase support operations, keeping the whole team informed and engaged, and assisting with prioritizing tickets by their SLA restrictions.


We are firm believers in letting technology streamline operational work and allowing humans to focus on solving real problems, and SLA Buddy is a great example of that.


## Final Thoughts#


SLA Buddy started a passion project, born from a need to ensure that we're providing top-quality support to Supabase's users. We're big fans of personal exploration and kaizen incremental change.


And we're not done with SLA Buddy. It'll grow and evolve as Supabase grows, and our needs and the needs of our users change. Because it's built on Supabase features, it'll be easy to update and maintain, and it'll provide more and more value to our internal operations, we hope it might provide some value to you, too. We're also big believers in the Open Source community, and welcome any feedback or ideas you might have to make SLA Buddy even better for everyone.


## More Resources About Slack and Edge Functions#


- [GitHub Repo: SLA Buddy](https://github.com/mansueli/slabuddy)
- [Docs Edge Functions: slack mention reply](https://supabase.com/docs/guides/functions/examples/slack-bot-mention)
- [Docs Edge Functions: geting started](https://supabase.com/docs/guides/functions/getting-started)
- [Docs Edge Functions: debugging](https://supabase.com/docs/guides/functions/debugging)
- [Slack Consolidate: a slackbot build with Python & Supabase](https://supabase.com/blog/slack-consolidate-slackbot-to-consolidate-messages)
