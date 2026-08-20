---
schema_version: "1.0.0"
document_id: "343724da20324a6df7b3bfb949a021e034d00c5f436375c674dce1d0d13bf13b"
company_key: "yc-sendblue"
company: "Sendblue"
source_id: "yc-sendblue-news-import-cbfb84d5bb49"
canonical_url: "https://www.sendblue.com/blog/imessage-api-php"
published_at: "2026-03-29T00:00:00+00:00"
first_seen_at: "2026-07-22T13:05:24.258144+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:dcf8dbd258144677c86caefc53931b4adffef6e6faa7565b787b35f11df361b3"
---

# How to Send iMessages from PHP (Laravel & WordPress)

Laravel's HTTP client makes API calls much cleaner. Create a service class:


` <?php // app/Services/SendblueService.php namespace App\\Services; use Illuminate\\Support\\Facades\\Http; class SendblueService { private string $baseUrl = 'https://api.sendblue.co'; public function sendMessage( string $number, string $content, ?string $sendStyle = null, ?string $mediaUrl = null ): array { $payload = compact('number', 'content'); if ($sendStyle) $payload\['send_style'\] = $sendStyle; if ($mediaUrl) $payload\['media_url'\] = $mediaUrl; $response = Http::withHeaders(\[ 'sb-api-key-id' => config('services.sendblue.key'), 'sb-api-secret-key' => config('services.sendblue.secret'), \])->post("{$this->baseUrl}/api/send-message", $payload); return $response->json(); } public function evaluateService(string $number): array { $response = Http::withHeaders(\[ 'sb-api-key-id' => config('services.sendblue.key'), 'sb-api-secret-key' => config('services.sendblue.secret'), \])->get("{$this->baseUrl}/api/evaluate-service", \[ 'number' => $number, \]); return $response->json(); } }`


Add your credentials to` config/services.php` :


` // config/services.php 'sendblue' => \[ 'key' => env('SENDBLUE_API_KEY'), 'secret' => env('SENDBLUE_API_SECRET'), \],`


Now use it anywhere in your Laravel app:


` // In a controller or command: $sendblue = new SendblueService(); $sendblue->sendMessage( number: '+15551234567', content: 'Your appointment is confirmed for tomorrow at 2pm.', sendStyle: 'celebration' );`
