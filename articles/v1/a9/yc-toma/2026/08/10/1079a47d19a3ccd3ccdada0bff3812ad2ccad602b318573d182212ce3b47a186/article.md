---
schema_version: "1.0.0"
document_id: "1079a47d19a3ccd3ccdada0bff3812ad2ccad602b318573d182212ce3b47a186"
company_key: "yc-toma"
company: "Toma"
source_id: "yc-toma-rss-1eeeda85d3f8"
canonical_url: "https://www.toma.com/blog/multi-threaded-audio-processing-in-node-js"
published_at: "2026-08-07T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:33.368971+00:00"
fetched_at: "2026-07-28T21:49:03.166047+00:00"
content_hash: "sha256:974757a24265cf552941d1cf28828709a78527288fa1fafa124f9ee31a5e64a0"
---

# Multi-threaded Audio Processing in Node.js: Lessons from Building a High-throughput Voice AI System

[← Blog](https://www.toma.com/blog)


# Multi-threaded Audio Processing in Node.js: Lessons from Building a High-throughput Voice AI System


How we scaled realtime audio processing to handle 80ms chunks with sub-500ms latency using Bun runtime and worker threads.


By Anthony · July 2025


## The Event Loop Problem: Why Single-threaded Fails for Audio


JavaScript's event loop is well-designed for I/O-intensive applications, but it becomes a bottleneck for CPU-intensive workloads, such as realtime audio processing. Let's understand why:


### The Event Loop Constraint


Node.js runs on a single main thread with an event loop that processes callbacks from various phases:


```text
┌───────────────────────────┐
┌─>│           timers          │  ← setTimeout, setInterval
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │     pending callbacks     │  ← I/O callbacks deferred to next iteration
│  │       idle, prepare       │  ← internal use only
│  │           poll            │  ← fetch new I/O events
│  │           check           │  ← setImmediate callbacks
└──┤      close callbacks      │  ← socket.destroy(), etc.
└───────────────────────────┘


```


### The Audio Processing Challenge


Our voice AI system needs to simultaneously:


- Process incoming audio streams at 16kHz/16-bit (32KB/second per call)
- Handle multiple audio tracks (human voice, AI responses, background effects)
- Perform realtime operations like Fast Fourier Transforms, volume mixing, and format conversion
- Maintain microsecond-precise audio synchronization across tracks
- Scale to hundreds of concurrent calls without audio dropouts


The math is unforgiving. For a single phone call processing 80ms audio chunks:


```text
// Per-call audio processing requirements
const SAMPLE_RATE = 16000; // Hz
const CHUNK_DURATION_MS = 80; // milliseconds
const BITS_PER_SAMPLE = 16;
const AUDIO_TRACKS = 4; // human, AI, background, effects


// Calculations
const samplesPerChunk = SAMPLE_RATE * (CHUNK_DURATION_MS / 1000); // 1,280 samples
const bytesPerChunk = samplesPerChunk * (BITS_PER_SAMPLE / 8); // 2,560 bytes
const totalSamplesPerChunk = samplesPerChunk * AUDIO_TRACKS; // 5,120 samples


// Scale to 100 concurrent calls
const totalSamplesPerTick = totalSamplesPerChunk * 100; // 512,000 samples every 80ms


// Processing budget: Must complete in <80ms to avoid audio dropouts
const processingBudget = 80; // milliseconds - hard realtime constraint


```


### Why the Event Loop Fails


When CPU-intensive audio processing blocks the event loop:


```text
// This blocks the event loop for ~50ms
function processAudioChunk(audioBuffer: Buffer): Buffer {
// Complex DSP operations
const fftResult = performFFT(audioBuffer);           // ~15ms
const filtered = applyNoiseReduction(fftResult);     // ~10ms
const normalized = normalizeAudio(filtered);         // ~5ms
const mixed = mixWithBackground(normalized);         // ~10ms
const compressed = applyCompression(mixed);          // ~10ms
return compressed;
}


// Meanwhile, other operations are starved:
setTimeout(() => {
console.log("This timer is delayed!"); // Fires 50ms late
}, 10);


// WebSocket responses are delayed
websocket.on('message', (data) => {
// This handler waits 50ms before processing
handleIncomingMessage(data);
});


```


The event loop's single-threaded nature means audio processing blocks all other operations, causing:


- Dropped audio frames when processing takes >80ms
- Delayed WebSocket responses to telephony providers
- Missed timer callbacks for realtime synchronization
- Cascading latency across the entire system


## Threads vs Processes: The Architecture Decision


When moving beyond the single-threaded event loop, we had two options: child processes or worker threads. Let's examine why we chose threads.


### Child Processes: The Traditional Approach


```text
// Child process approach


class AudioProcessorWithProcesses {
private processes: Map<string, ChildProcess> = new Map();


async processAudio(audioData: Buffer): Promise<Buffer> {
// Spawn a new process for each audio chunk
const process = spawn('node', ['./audio-processor.js']);


// Send data via stdin/stdout
process.stdin.write(audioData);


return new Promise((resolve, reject) => {
let result = Buffer.alloc(0);
process.stdout.on('data', (chunk) => {
result = Buffer.concat([result, chunk]);
});
process.on('close', (code) => {
if (code === 0) {
resolve(result);
} else {
reject(new Error(`Process exited with code ${code}`));
}
});
});
}
}


```


### Process Advantages


- Complete isolation: Process crashes don't affect the main application
- Separate memory space: No shared memory corruption risk
- Platform stability: Mature IPC mechanisms


### Process Disadvantages


- High overhead: Process creation takes 10–50ms
- Memory duplication: Each process duplicates the V8 heap
- IPC serialization: Data must be serialized/deserialized across process boundaries
- Resource limits: OS limits on process count (typically ~1000s)


### Worker Threads: The Modern Solution


```text
// Worker thread approach


class AudioProcessorWithThreads {
private worker: Worker;
private nextId = 1;
private promises = new Map<number, { resolve: Function; reject: Function }>();


constructor() {
// Create a persistent worker thread
this.worker = new Worker(__filename);
// Handle responses from worker
this.worker.on('message', (msg: any) => {
const { id, result, error } = msg;
const promise = this.promises.get(id);
if (promise) {
if (error) {
promise.reject(new Error(error));
} else {
promise.resolve(result);
}
this.promises.delete(id);
}
});
}


async processAudio(audioData: Buffer): Promise<Buffer> {
return new Promise((resolve, reject) => {
const id = this.nextId++;
this.promises.set(id, { resolve, reject });
// Send to worker thread - no serialization overhead for Buffers
this.worker.postMessage({
id,
method: 'processAudio',
audioData
});
});
}
}


// Worker thread code (same file, different execution context)
if (!isMainThread && parentPort) {
parentPort.on('message', async (msg) => {
const { id, method, audioData } = msg;
try {
// CPU-intensive processing happens here
const result = await performAudioProcessing(audioData);
parentPort.postMessage({ id, result });
} catch (error) {
parentPort.postMessage({ id, error: error.message });
}
});
}


```


### Thread Advantages


- Shared memory space: Direct buffer sharing without serialization
- Low creation overhead: Thread creation takes <1ms
- Efficient communication: SharedArrayBuffer for zero-copy data transfer
- Resource efficiency: Threads share the same V8 isolate resources


### Thread Disadvantages


- Shared memory risks: Potential for memory corruption
- Limited isolation: Thread crashes can affect the main process
- Debugging complexity: Race conditions and deadlocks


### Why Threads Won for Audio Processing


For our realtime audio workload, threads provide critical advantages:


1. **Shared Memory Performance** : Audio buffers can be shared directly via SharedArrayBuffer
2. **Low Latency** : Thread communication overhead is <1ms vs 10–50ms for processes
3. **Memory Efficiency** : No V8 heap duplication for hundreds of concurrent audio streams
4. **Resource Scaling** : Can create thousands of threads vs hundreds of processes


## Why We Chose Bun Over Node.js


Before diving into our multi-threading implementation, let's examine why we chose Bun as our runtime foundation, particularly for CPU-intensive workloads.


### JavaScript Engine Performance


**Node.js uses V8 (Google's JavaScript engine)**


- Optimized for Chrome's web workloads
- Excellent JIT compilation for typical web applications
- Heavy garbage collection pressure under high throughput


**Bun uses JavaScriptCore (Apple's JavaScript engine)**


- Optimized for performance-critical applications
- Lower memory overhead per JavaScript context
- Better performance for CPU-intensive operations


### Benchmark Results for Our Workload


Based on our internal benchmarks and this[public data](https://dev.to/hamzakhan/rust-vs-go-vs-bun-vs-nodejs-the-ultimate-2024-performance-showdown-2jml) :


```text
// Audio processing benchmark (1000 iterations)
const audioBuffer = Buffer.alloc(1280 * 2); // 80ms @ 16kHz


// Node.js performance
console.time('node-audio-processing');
for (let i = 0; i < 1000; i++) {
processAudioChunk(audioBuffer);
}
console.timeEnd('node-audio-processing');
// Result: ~116 seconds


// Bun performance
console.time('bun-audio-processing');
for (let i = 0; i < 1000; i++) {
processAudioChunk(audioBuffer);
}
console.timeEnd('bun-audio-processing');
// Result: ~46 seconds (2.5x faster)


```


### HTTP Server Performance (Critical for WebSocket connections)


- Bun: 52,000+ requests/second
- Node.js: ~14,000 requests/second
- Improvement: 377% better throughput for realtime telephony connections


### Startup Performance


- Bun: ~100ms cold start
- Node.js: ~300ms cold start
- Impact: Faster auto-scaling in our microservice architecture


### The Memory Trade-off


Bun consumes more memory (+75% in our testing)


```text
// Memory usage comparison for 100 concurrent audio streams
const memoryUsage = process.memoryUsage();


// Node.js
console.log(memoryUsage);
// { rss: 40MB, heapUsed: 25MB, heapTotal: 30MB }


// Bun
console.log(memoryUsage);
// { rss: 70MB, heapUsed: 44MB, heapTotal: 53MB }


```


This trade-off is acceptable for our use case because:


- Memory is cheaper than CPU cycles in a distributed infrastructure
- CPU-bound workloads benefit more from execution speed than memory efficiency
- Horizontal scaling allows us to add more instances rather than optimize memory


## The Architecture: Worker Thread Proxy Pattern


Our solution employs a worker thread proxy pattern that isolates CPU-intensive audio processing while maintaining a clean API for the main thread, leveraging JavaScript's event loop for I/O operations.


### System Architecture


```text
Main Thread (Event Loop)
├── WebSocket I/O (telephony providers)
├── HTTP Server (REST API)
├── Timer Management (synchronization)
├── AudioProcessorProxy (thread coordination)
│   ├── Worker Thread 1 (audio processing)
│   ├── Worker Thread 2 (audio processing)
│   └── Worker Thread N (audio processing)
└── Database I/O (call metadata)


```


### Core Implementation


#### 1. Main Thread Proxy (Event Loop Optimized)


```text


private worker: Worker;
private nextRequestId = 1;
private pendingRequests = new Map<number, {
resolve: Function;
reject: Function;
timestamp: number;
}>();


constructor() {
this.worker = new Worker(__filename);
this.setupMessageHandling();
this.setupErrorHandling();
}


private setupMessageHandling(): void {
this.worker.on('message', (msg: WorkerResponse) => {
const { requestId, result, error } = msg;
const request = this.pendingRequests.get(requestId);


if (request) {
// Clean up request tracking
this.pendingRequests.delete(requestId);


// Track processing time
const processingTime = Date.now() - request.timestamp;
this.metrics.histogram('audio.processing.duration', processingTime);


if (error) {
request.reject(new Error(error));
} else {
request.resolve(result);
}
}
});
}


// Non-blocking method that returns immediately
async processAudioChunk(audioData: Buffer, options: ProcessingOptions): Promise<Buffer> {
return new Promise((resolve, reject) => {
const requestId = this.nextRequestId++;


// Track request for response correlation
this.pendingRequests.set(requestId, {
resolve,
reject,
timestamp: Date.now()
});


// Send to worker thread - this is async and non-blocking
this.worker.postMessage({
requestId,
method: 'processAudioChunk',
audioData,
options
});
});
}


async mixAudioTracks(tracks: AudioTrack[]): Promise<Buffer> {
return this.sendToWorker('mixAudioTracks', { tracks });
}


async applyEffects(audioData: Buffer, effects: AudioEffect[]): Promise<Buffer> {
return this.sendToWorker('applyEffects', { audioData, effects });
}


private async sendToWorker(method: string, args: any): Promise<any> {
return new Promise((resolve, reject) => {
const requestId = this.nextRequestId++;
this.pendingRequests.set(requestId, {
resolve,
reject,
timestamp: Date.now()
});
this.worker.postMessage({
requestId,
method,
...args
});
});
}
}


```


#### 2. Worker Thread Implementation (CPU-Intensive Processing)


```text
// This code runs in the worker thread context
if (!isMainThread && parentPort) {
class AudioProcessor {
private audioBuffer: SharedArrayBuffer;
private processingMetrics: Map<string, number> = new Map();


constructor() {
// Pre-allocate shared memory for audio processing
this.audioBuffer = new SharedArrayBuffer(1024 * 1024 * 10); // 10MB
}


// This is CPU-intensive work that would block the event loop
const startTime = performance.now();
try {
// Convert to Int16Array for processing
const samples = new Int16Array(audioData.buffer);


// Apply processing pipeline
let processedSamples = samples;
if (options.enableNoiseReduction) {
processedSamples = await this.applyNoiseReduction(processedSamples);
}
if (options.enableVolumeNormalization) {
processedSamples = await this.normalizeVolume(processedSamples);
}
if (options.enableCompression) {
processedSamples = await this.applyCompression(processedSamples);
}


// Convert back to Buffer
const result = Buffer.from(processedSamples.buffer);


// Track processing time
const processingTime = performance.now() - startTime;
this.processingMetrics.set('lastProcessingTime', processingTime);


return result;
} catch (error) {
throw new Error(`Audio processing failed: ${error.message}`);
}
}


private async applyNoiseReduction(samples: Int16Array): Promise<Int16Array> {
// CPU-intensive FFT-based noise reduction
const fftSize = 1024;
const result = new Int16Array(samples.length);


for (let i = 0; i < samples.length; i += fftSize) {
const chunk = samples.slice(i, i + fftSize);
// Apply FFT
const frequencyDomain = this.fft(chunk);
// Apply noise reduction filter
const filtered = this.applyNoiseFilter(frequencyDomain);
// Apply inverse FFT
const timeDomain = this.ifft(filtered);
// Copy back to result
result.set(timeDomain, i);
}


return result;
}


private async mixAudioTracks(tracks: AudioTrack[]): Promise<Buffer> {
// Mix multiple audio tracks with precise timing
const maxLength = Math.max(...tracks.map(t => t.audioData.length));
const mixedSamples = new Int16Array(maxLength / 2);


for (const track of tracks) {
const trackSamples = new Int16Array(track.audioData.buffer);
const volume = track.volume || 1.0;


for (let i = 0; i < trackSamples.length; i++) {
// Mix with volume control and clipping prevention
mixedSamples[i] = Math.max(-32768, Math.min(32767,
mixedSamples[i] + (trackSamples[i] * volume)
));
}
}


return Buffer.from(mixedSamples.buffer);
}


// Placeholder for FFT implementation
private fft(samples: Int16Array): Complex[] {
// Implement Fast Fourier Transform
// This is CPU-intensive and would block the event loop
return [];
}


private ifft(frequencies: Complex[]): Int16Array {
// Implement Inverse Fast Fourier Transform
return new Int16Array(0);
}
}


// Message handling in worker thread
const processor = new AudioProcessor();
parentPort.on('message', async (msg: WorkerRequest) => {
const { requestId, method, ...args } = msg;


try {
// Route to appropriate processing method
let result;
switch (method) {
case 'processAudioChunk':
result = await processor.processAudioChunk(args.audioData, args.options);
break;
case 'mixAudioTracks':
result = await processor.mixAudioTracks(args.tracks);
break;
case 'applyEffects':
result = await processor.applyEffects(args.audioData, args.effects);
break;
default:
throw new Error(`Unknown method: ${method}`);
}


// Send result back to main thread
parentPort.postMessage({
requestId,
result
});
} catch (error) {
// Send error back to main thread
parentPort.postMessage({
requestId,
error: error.message
});
}
});
}


```


## Advanced Threading Optimizations


### 1. SharedArrayBuffer for Zero-Copy Operations


For maximum performance, we use SharedArrayBuffer to eliminate serialization overhead:


```text
class OptimizedAudioProcessor {
private sharedBuffer: SharedArrayBuffer;
private sharedView: Int16Array;


constructor() {
// Create shared memory accessible by both threads
this.sharedBuffer = new SharedArrayBuffer(1024 * 1024 * 4); // 4MB
this.sharedView = new Int16Array(this.sharedBuffer);
}


async processWithSharedMemory(audioData: Buffer): Promise<Buffer> {
// Copy audio data to shared memory
const audioSamples = new Int16Array(audioData.buffer);
this.sharedView.set(audioSamples, 0);


// Send offset and length instead of copying data
return this.sendToWorker('processSharedAudio', {
offset: 0,
length: audioSamples.length
});
}
}


// Worker thread processes shared memory directly
if (!isMainThread && parentPort) {
parentPort.on('message', async (msg) => {
if (msg.method === 'processSharedAudio') {
const { offset, length } = msg;


// Access shared memory directly - no serialization!
const sharedView = new Int16Array(msg.sharedBuffer);
const audioSamples = sharedView.subarray(offset, offset + length);


// Process in-place
for (let i = 0; i < audioSamples.length; i++) {
audioSamples[i] = audioSamples[i] * 0.8; // Apply volume
}


// Result is already in shared memory
parentPort.postMessage({
requestId: msg.requestId,
result: 'processed'
});
}
});
}


```


### 2. Thread Pool Management


For handling multiple concurrent audio streams:


```text
class AudioProcessorPool {
private workers: Worker[] = [];
private roundRobinIndex = 0;
private workerLoad = new Map<Worker, number>();


constructor(poolSize: number = 4) {
for (let i = 0; i < poolSize; i++) {
const worker = new Worker(__filename);
this.workers.push(worker);
this.workerLoad.set(worker, 0);
worker.on('message', (msg) => {
// Decrease load count when work completes
const currentLoad = this.workerLoad.get(worker) || 0;
this.workerLoad.set(worker, Math.max(0, currentLoad - 1));
});
}
}


private getNextWorker(): Worker {
// Load balancing: choose worker with lowest load
let minLoad = Infinity;
let selectedWorker = this.workers[0];


for (const worker of this.workers) {
const load = this.workerLoad.get(worker) || 0;
if (load < minLoad) {
minLoad = load;
selectedWorker = worker;
}
}


// Increase load count
this.workerLoad.set(selectedWorker, minLoad + 1);


return selectedWorker;
}


async processAudio(audioData: Buffer): Promise<Buffer> {
const worker = this.getNextWorker();


return new Promise((resolve, reject) => {
const requestId = Date.now() + Math.random();
const timeout = setTimeout(() => {
reject(new Error('Worker processing timeout'));
}, 5000);


const messageHandler = (msg: any) => {
if (msg.requestId === requestId) {
clearTimeout(timeout);
worker.off('message', messageHandler);
if (msg.error) {
reject(new Error(msg.error));
} else {
resolve(msg.result);
}
}
};


worker.on('message', messageHandler);
worker.postMessage({
requestId,
method: 'processAudio',
audioData
});
});
}
}


```


### 3. Memory Management and Garbage Collection


Worker threads require careful memory management:


```text
class MemoryManagedAudioProcessor {
private bufferPool: Buffer[] = [];
private maxPoolSize = 100;
private gcInterval: NodeJS.Timeout;


constructor() {
// Pre-allocate buffer pool
for (let i = 0; i < this.maxPoolSize; i++) {
this.bufferPool.push(Buffer.alloc(1024 * 4)); // 4KB buffers
}


// Periodic garbage collection
this.gcInterval = setInterval(() => {
if (global.gc) {
global.gc();
}
}, 10000); // Every 10 seconds
}


private getBuffer(size: number): Buffer {
// Reuse existing buffers when possible
for (let i = 0; i < this.bufferPool.length; i++) {
const buffer = this.bufferPool[i];
if (buffer && buffer.length >= size) {
this.bufferPool[i] = null; // Mark as used
return buffer.slice(0, size);
}
}


// Allocate new buffer if pool is empty
return Buffer.alloc(size);
}


private returnBuffer(buffer: Buffer): void {
// Return buffer to pool for reuse
for (let i = 0; i < this.bufferPool.length; i++) {
if (!this.bufferPool[i]) {
this.bufferPool[i] = buffer;
return;
}
}
}


async processAudio(audioData: Buffer): Promise<Buffer> {
const workBuffer = this.getBuffer(audioData.length);
try {
// Process audio using pooled buffer
audioData.copy(workBuffer);


// Perform processing...
const result = await this.performProcessing(workBuffer);


return result;
} finally {
// Always return buffer to pool
this.returnBuffer(workBuffer);
}
}


destroy(): void {
clearInterval(this.gcInterval);
this.bufferPool = [];
}
}


```


## Integration with Realtime Voice AI Pipeline


Our worker thread architecture integrates seamlessly with the broader voice AI pipeline, leveraging the event loop for I/O and threads for CPU-intensive work:


```text
// Main thread handles I/O and coordination


private audioProcessor: AudioProcessorProxy;
private websocket: WebSocket;
private llmClient: LLMClient;
private ttsClient: TTSClient;


constructor() {
this.audioProcessor = new AudioProcessorProxy();
this.setupWebSocketHandling();
}


private setupWebSocketHandling(): void {
// Event loop handles WebSocket I/O efficiently
this.websocket.on('message', async (audioData: Buffer) => {
// This is non-blocking - audio processing happens in worker thread
const processedAudio = await this.audioProcessor.processAudioChunk(audioData, {
enableNoiseReduction: true,
enableVolumeNormalization: true
});


// Parallel processing while audio is being processed
const [transcription, vadResult] = await Promise.all([
this.transcribeAudio(processedAudio),
this.analyzeVoiceActivity(processedAudio)
]);


if (vadResult.isSpeaking) {
// Generate AI response
const aiResponse = await this.llmClient.generateResponse(transcription);


// Convert to audio (also uses worker thread)
const aiAudio = await this.ttsClient.generateAudio(aiResponse);


// Mix with background audio
const mixedAudio = await this.audioProcessor.mixAudioTracks([
{ audioData: aiAudio, volume: 0.8 },
{ audioData: this.backgroundAudio, volume: 0.2 }
]);


// Send back via WebSocket (event loop I/O)
this.websocket.send(mixedAudio);
}
});
}


private async transcribeAudio(audioData: Buffer): Promise<string> {
// I/O operation - handled by event loop
return await this.makeHTTPRequest('/transcribe', audioData);
}


private async analyzeVoiceActivity(audioData: Buffer): Promise<VADResult> {
// CPU-intensive operation - delegated to worker thread
return await this.audioProcessor.analyzeVoiceActivity(audioData);
}
}


```


## Performance Monitoring and Observability


We instrument our worker threads with comprehensive metrics to understand performance characteristics using Datadog. Below is a provider-agnostic implementation:


```text
class InstrumentedAudioProcessor {
private metrics = {
processedChunks: 0,
averageProcessingTime: 0,
memoryUsage: 0,
errorRate: 0
};


constructor() {
// Monitor worker thread performance
setInterval(() => {
this.reportMetrics();
}, 1000);
}


async processAudio(audioData: Buffer): Promise<Buffer> {
const startTime = performance.now();
const initialMemory = process.memoryUsage().heapUsed;
try {
const result = await this.performProcessing(audioData);


// Track success metrics
const processingTime = performance.now() - startTime;
this.updateMetrics(processingTime, true);
return result;
} catch (error) {
// Track error metrics
this.updateMetrics(0, false);
throw error;
} finally {
// Monitor memory usage
const finalMemory = process.memoryUsage().heapUsed;
this.metrics.memoryUsage = finalMemory - initialMemory;
}
}


private updateMetrics(processingTime: number, success: boolean): void {
this.metrics.processedChunks++;


if (success) {
// Moving average for processing time
this.metrics.averageProcessingTime =
(this.metrics.averageProcessingTime * 0.9) + (processingTime * 0.1);
} else {
this.metrics.errorRate =
(this.metrics.errorRate * 0.9) + (0.1);
}
}


private reportMetrics(): void {
console.log('Worker Thread Metrics:', {
processedChunks: this.metrics.processedChunks,
averageProcessingTime: `${this.metrics.averageProcessingTime.toFixed(2)}ms`,
memoryUsage: `${(this.metrics.memoryUsage / 1024 / 1024).toFixed(2)}MB`,
errorRate: `${(this.metrics.errorRate * 100).toFixed(2)}%`
});
}
}


```


## Production Results and Lessons Learned


### Performance Achievements


Our multi-threaded audio processing system now handles:


- 500+ concurrent voice calls across our infrastructure
- <80ms processing latency for realtime audio chunks
- 99.9% uptime for the audio processing pipeline
- <1% CPU usage per call on our worker threads
- 2.5x performance improvement over single-threaded Node.js


### Key Lessons Learned


#### 1. Event Loop + Worker Threads = Optimal Architecture


The combination of JavaScript's event loop for I/O operations and worker threads for CPU-intensive processing provides the best of both worlds:


- Non-blocking I/O for WebSocket connections and database operations
- Parallel processing for audio DSP operations
- Resource efficiency through proper separation of concerns


#### 2. Bun's Performance Gains Are Real and Significant


Our switch to Bun provided measurable improvements:


- 2.5x faster execution for audio processing workloads
- 377% better HTTP throughput for WebSocket connections
- Faster startup times for our microservice architecture
- Lower garbage collection pressure under high load


#### 3. SharedArrayBuffer Is a Game-Changer


Using SharedArrayBuffer for audio data eliminates serialization overhead:


- Zero-copy data transfer between threads
- Predictable performance characteristics
- Reduced memory allocation pressure


#### 4. Thread Pools Require Careful Management


Worker thread pools need sophisticated load balancing:


- Round-robin scheduling causes hotspots
- Load-based distribution provides better balance
- Graceful degradation when workers fail


#### 5. Memory Management Is Critical


Pre-allocating buffers and using object pools prevents:


- Garbage collection pauses during audio processing
- Memory leaks in long-running audio streams
- Buffer allocation overhead in hot paths


## Debugging and Troubleshooting


Common issues we encountered and solutions:


```text
// Issue: Worker thread deadlocks
class DeadlockSafeProcessor {
private requestTimeout = 5000; // 5 second timeout


async processWithTimeout(audioData: Buffer): Promise<Buffer> {
return new Promise((resolve, reject) => {
const timer = setTimeout(() => {
reject(new Error('Worker thread timeout - possible deadlock'));
}, this.requestTimeout);
this.processAudio(audioData)
.then(resolve)
.catch(reject)
.finally(() => clearTimeout(timer));
});
}
}


// Issue: Memory leaks in worker threads
class MemoryLeakSafeProcessor {
private cleanupInterval: NodeJS.Timeout;


constructor() {
// Periodic cleanup
this.cleanupInterval = setInterval(() => {
this.cleanup();
}, 30000); // Every 30 seconds
}


private cleanup(): void {
// Force garbage collection
if (global.gc) {
global.gc();
}


// Clear any lingering references
this.clearCaches();
}


destroy(): void {
clearInterval(this.cleanupInterval);
this.cleanup();
}
}


```


## Conclusion


Multi-threaded audio processing with worker threads represents a significant architectural evolution from single-threaded Node.js applications. By combining JavaScript's excellent event loop for I/O operations with worker threads for CPU-intensive tasks, and choosing Bun for its superior performance characteristics, we've built a system that scales to handle realtime voice AI at production scale.


The key insights from our implementation:


1. **Understand your workload** : CPU-intensive tasks need threads, I/O tasks need the event loop
2. **Choose the right runtime** : Bun's performance advantages compound at scale
3. **Optimize data transfer** : SharedArrayBuffer eliminates serialization overhead
4. **Monitor aggressively** : Worker threads require different monitoring strategies
5. **Plan for failure** : Graceful degradation and timeouts are essential


The patterns shown here are broadly applicable to any high-throughput, low-latency system that combines I/O operations with CPU-intensive processing.


Want to work on problems like this? Toma is disrupting the $2T automotive industry with agentic AI. We're looking for senior engineers who love solving complex challenges in realtime systems. Check out our[jobs page](https://www.toma.com/jobs) to learn more.


*Disclaimer: This post represents our engineering approach as of 2025. Technologies, benchmarks, and implementation details may evolve as we continue to optimize our systems.*
