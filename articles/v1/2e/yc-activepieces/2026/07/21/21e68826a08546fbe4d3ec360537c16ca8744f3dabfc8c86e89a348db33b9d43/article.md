---
schema_version: "1.0.0"
document_id: "21e68826a08546fbe4d3ec360537c16ca8744f3dabfc8c86e89a348db33b9d43"
company_key: "yc-activepieces"
company: "Activepieces"
source_id: "yc-activepieces-rss-17c781695c1c"
canonical_url: "https://www.activepieces.com/blog/how-to-handle-errors-in-production-ai-agents"
published_at: "2026-07-10T10:30:23+00:00"
first_seen_at: "2026-07-20T23:19:46.791052+00:00"
fetched_at: "2026-07-28T21:08:37.946927+00:00"
content_hash: "sha256:f2794fe3f1712b1b8751fb89251532ca77db836a7dadb57ca50e70bfe83cc15d"
---

# How to handle errors in production AI agents

**Error handling** separates production AI agents from prototypes. Here's how to build resilience into every agent operation.


```text
# Basic retry with exponential backoff
import time
import random


![A vintage circuit breaker with one switch open, photographed in dramatic lighting to illustrate protection from system failures.](https://ap-hype-blog-media.nyc3.digitaloceanspaces.com/blog/1d9d0d12-0be8-49ba-a4c6-f063533124f4/2qlq8mrn94.png "AI illustration by Charlie")


def retry_with_backoff(func, max_retries=3, base_delay=1):
for attempt in range(max_retries):
try:
return func()
except Exception as e:
if attempt == max_retries:
raise e
delay = base_delay * (2 ** attempt) + random.uniform(0, 1)
time.sleep(delay)
```


## 1. Implement Circuit Breakers for External APIs


Circuit breakers prevent cascading failures by stopping requests to failing external APIs and returning cached responses or graceful degradation instead. When an API starts failing, the circuit breaker stops sending requests and returns cached responses or graceful degradation.


```text
class CircuitBreaker:
def __init__(self, failure_threshold=5, timeout=60):
self.failure_count = 0
self.failure_threshold = failure_threshold
self.timeout = timeout
self.last_failure_time = None
self.state = 'CLOSED'  # CLOSED, OPEN, HALF_OPEN


def call(self, func):
if self.state == 'OPEN':
if time.time() - self.last_failure_time > self.timeout:
self.state = 'HALF_OPEN'
else:
raise Exception("Circuit breaker is OPEN")


try:
result = func()
if self.state == 'HALF_OPEN':
self.state = 'CLOSED'
self.failure_count = 0
return result
except Exception as e:
self.failure_count += 1
self.last_failure_time = time.time()
if self.failure_count >= self.failure_threshold:
self.state = 'OPEN'
raise e
```


The circuit breaker tracks failure rates and blocks requests when a service becomes unreliable. This prevents your agent from hammering a failing API and gives the service time to recover.


**Real scenario** : Your agent processes customer support tickets by calling a sentiment analysis API. The API starts timing out during peak hours. Without a circuit breaker, your agent queues up hundreds of failed requests. With one, it switches to a simpler keyword-based fallback after five consecutive failures.


## 2. Build Structured Retry Logic with Jitter


Not all errors deserve the same retry strategy. Network timeouts need quick retries; rate limits need longer waits; authentication errors need human intervention.


```text
class RetryConfig:
def __init__(self):
self.strategies = {
'timeout': {'max_retries': 3, 'base_delay': 0.5, 'max_delay': 5},
'rate_limit': {'max_retries': 5, 'base_delay': 60, 'max_delay': 300},
'server_error': {'max_retries': 2, 'base_delay': 2, 'max_delay': 10},
'auth_error': {'max_retries': 0}  # Immediate escalation
}


def get_strategy(self, error_type):
return self.strategies.get(error_type, self.strategies['server_error'])


def smart_retry(func, error_classifier, config):
for attempt in range(config['max_retries'] + 1):
try:
return func()
except Exception as e:
error_type = error_classifier(e)
strategy = config.get_strategy(error_type)


if attempt >= strategy['max_retries']:
raise e


# Exponential backoff with jitter
delay = min(
strategy['base_delay'] * (2 ** attempt),
strategy['max_delay']
)
jitter = random.uniform(0.1, 0.3) * delay
time.sleep(delay + jitter)
```


The jitter prevents the thundering herd problem when multiple agents retry simultaneously. Random delays spread the load and reduce the chance of synchronized failures.


## 3. Implement Graceful Degradation Patterns


Production agents need fallback behaviors when primary systems fail. Design each agent operation with multiple execution paths, from optimal to minimal viable.


```text
class GracefulAgent:
def __init__(self):
self.primary_llm = OpenAIClient()
self.fallback_llm = AnthropicClient()
self.local_cache = RedisCache()
self.simple_rules = RuleEngine()


def process_request(self, user_input):
# Try primary path with full context
try:
return self.primary_llm.generate(
prompt=self.build_full_prompt(user_input),
temperature=0.1
)
except Exception:
pass


# Fallback to secondary LLM with reduced context
try:
return self.fallback_llm.generate(
prompt=self.build_simple_prompt(user_input),
temperature=0.2
)
except Exception:
pass


# Check cache for similar requests
cached_response = self.local_cache.get_similar(user_input)
if cached_response:
return self.adapt_cached_response(cached_response, user_input)


# Final fallback to rule-based system
return self.simple_rules.process(user_input)
```


Each fallback layer trades sophistication for reliability. The rule-based system might only handle common cases, but it works when everything else fails.


## 4. Add Comprehensive Logging and Observability


Error handling without visibility is debugging in the dark. Log every retry attempt, fallback activation, and recovery action with enough context to reconstruct the failure path.


```text
import structlog
import uuid


logger = structlog.get_logger()


class ObservableAgent:
def __init__(self):
self.request_id = None


def process_with_observability(self, user_input):
self.request_id = str(uuid.uuid4())


logger.info(
"agent_request_started",
request_id=self.request_id,
input_length=len(user_input),
timestamp=time.time()
)


try:
result = self.process_request(user_input)
logger.info(
"agent_request_completed",
request_id=self.request_id,
success=True,
response_length=len(result)
)
return result


except Exception as e:
logger.error(
"agent_request_failed",
request_id=self.request_id,
error_type=type(e).__name__,
error_message=str(e),
stack_trace=traceback.format_exc()
)
raise


def log_retry_attempt(self, attempt_number, error_type, delay):
logger.warning(
"retry_attempt",
request_id=self.request_id,
attempt=attempt_number,
error_type=error_type,
retry_delay=delay
)
```


Structured logging makes error patterns visible. You can track which APIs fail most often, how retry strategies perform, and where graceful degradation activates.


## 5. Design Idempotent Operations


Agent actions should be safe to retry. If your agent sends an email, charges a credit card, or updates a database, running the same operation twice should not cause problems.


```text
class IdempotentAgent:
def __init__(self):
self.operation_log = {}


def send_notification(self, user_id, message, idempotency_key=None):
if not idempotency_key:
idempotency_key = hashlib.md5(
f"{user_id}:{message}:{int(time.time() / 3600)}"
.encode()
).hexdigest()


# Check if we've already processed this operation
if idempotency_key in self.operation_log:
logger.info(
"duplicate_operation_skipped",
idempotency_key=idempotency_key,
user_id=user_id
)
return self.operation_log[idempotency_key]


try:
result = self.email_service.send(user_id, message)
self.operation_log[idempotency_key] = result
return result
except Exception as e:
# Don't log failed operations as completed
logger.error(
"operation_failed",
idempotency_key=idempotency_key,
error=str(e)
)
raise
```


Idempotency keys prevent duplicate operations during retries. The key combines the operation parameters with a time window, so the same logical operation gets the same key within a reasonable timeframe.


## 6. Implement Health Checks and Self-Recovery


Production agents need to monitor their own health and recover from degraded states. Build health checks that verify critical dependencies and trigger recovery actions.


```text
class HealthMonitor:
def __init__(self, agent):
self.agent = agent
self.health_checks = [
self.check_llm_connectivity,
self.check_database_connection,
self.check_memory_usage,
self.check_queue_depth
]
self.recovery_actions = {
'llm_down': self.switch_to_fallback_llm,
'db_connection_lost': self.reconnect_database,
'memory_high': self.clear_caches,
'queue_backed_up': self.increase_processing_rate
}


def run_health_check(self):
health_status = {}


for check in self.health_checks:
try:
status = check()
health_status[check.__name__] = status


if not status['healthy']:
self.trigger_recovery(status['issue'])


except Exception as e:
health_status[check.__name__] = {
'healthy': False,
'error': str(e)
}


return health_status


def check_llm_connectivity(self):
try:
response = self.agent.llm_client.generate(
"Test prompt",
max_tokens=5,
timeout=10
)
return {'healthy': True}
except Exception as e:
return {
'healthy': False,
'issue': 'llm_down',
'error': str(e)
}


def trigger_recovery(self, issue_type):
recovery_action = self.recovery_actions.get(issue_type)
if recovery_action:
logger.info(
"triggering_recovery",
issue_type=issue_type,
action=recovery_action.__name__
)
recovery_action()
```


Health checks run on a schedule and after failed operations. They catch problems before they cascade and trigger automatic recovery when possible.


## 7. Handle Partial Failures in Multi-Step Operations


Complex agent workflows involve multiple API calls, database updates, and external integrations. When step three fails, you need to handle the partial completion gracefully.


```text
class TransactionalAgent:
def __init__(self):
self.compensation_handlers = {}


def execute_workflow(self, steps, context):
completed_steps = []


try:
for i, step in enumerate(steps):
logger.info(
"executing_step",
step_index=i,
step_name=step.__name__,
context_id=context.get('id')
)


result = step(context)
completed_steps.append((i, step, result))
context.update(result)


return context


except Exception as e:
logger.error(
"workflow_failed",
failed_step=i,
completed_steps=len(completed_steps),
error=str(e)
)


# Compensate completed steps in reverse order
for step_index, step_func, step_result in reversed(completed_steps):
try:
compensator = self.compensation_handlers.get(step_func.__name__)
if compensator:
compensator(step_result, context)
logger.info(
"compensation_completed",
step_index=step_index,
step_name=step_func.__name__
)
except Exception as comp_error:
logger.error(
"compensation_failed",
step_index=step_index,
error=str(comp_error)
)


raise e
```


Compensation handlers undo completed steps when later steps fail. This prevents partial state corruption and makes workflows safe to retry from the beginning.


The difference between a demo and production is not the happy path. It's how gracefully your agent handles the inevitable failures, retries operations safely, and recovers from degraded states. Build these patterns from day one, not after the first outage.


## Frequently asked questions


### What is a circuit breaker in AI agent error handling?


A circuit breaker prevents cascading failures by stopping requests to failing external APIs and returning cached responses or graceful degradation instead. It tracks failure rates and blocks requests when a service becomes unreliable.


### How do you implement retry logic with jitter for production AI agents?


Use different retry strategies based on error type: quick retries for timeouts, longer waits for rate limits, no retries for auth errors. Add random jitter to prevent thundering herd problems when multiple agents retry simultaneously.


### What is graceful degradation in AI agent error handling?


Graceful degradation provides fallback behaviors when primary systems fail. Each agent operation has multiple execution paths from optimal (primary LLM) to minimal viable (rule-based system).


### Why are idempotent operations important for AI agents?


Idempotent operations are safe to retry without causing duplicate effects. Using idempotency keys prevents duplicate emails, charges, or database updates when operations are retried after failures.


### How do you handle partial failures in multi-step AI agent workflows?


Use compensation handlers that undo completed steps when later steps fail. Execute steps in sequence, track completions, and run compensators in reverse order if any step fails to prevent partial state corruption.
