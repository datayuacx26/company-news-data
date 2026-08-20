---
schema_version: "1.0.0"
document_id: "9fa484299ffe341f9b7d465635d0f25368b9c639ff8ee6c88bf2afc5192dc7a9"
company_key: "yc-sendblue"
company: "Sendblue"
source_id: "yc-sendblue-news-import-cbfb84d5bb49"
canonical_url: "https://www.sendblue.com/blog/imessage-api-csharp"
published_at: "2026-03-29T00:00:00+00:00"
first_seen_at: "2026-07-22T13:05:24.258144+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:4d68562f36a8f06ad5ec97566a216bda76cbe61e2275289fbdd902fc153e7840"
---

# How to Send iMessages from C# / .NET

Create a typed HttpClient service for Sendblue. This is the recommended approach in .NET for managing HTTP connections efficiently:


` // Services/SendblueClient.cs using System.Net.Http.Json; using Microsoft.Extensions.Options; public class SendblueOptions { public string ApiKey { get; set; } = ""; public string ApiSecret { get; set; } = ""; public string BaseUrl { get; set; } = "https://api.sendblue.co"; } public class SendblueClient { private readonly HttpClient _http; public SendblueClient(HttpClient http, IOptions<SendblueOptions> opts) { _http = http; _http.BaseAddress = new Uri(opts.Value.BaseUrl); _http.DefaultRequestHeaders.Add("sb-api-key-id", opts.Value.ApiKey); _http.DefaultRequestHeaders.Add("sb-api-secret-key", opts.Value.ApiSecret); } public async Task<SendMessageResponse> SendMessageAsync( string number, string content, string? sendStyle = null, string? mediaUrl = null) { var payload = new Dictionary<string, string?> { \["number"\] = number, \["content"\] = content, \["send_style"\] = sendStyle, \["media_url"\] = mediaUrl, }; // Remove null values var filtered = payload .Where(kv => kv.Value != null) .ToDictionary(kv => kv.Key, kv => kv.Value); var response = await _http.PostAsJsonAsync("/api/send-message", filtered); response.EnsureSuccessStatusCode(); return await response.Content.ReadFromJsonAsync<SendMessageResponse>() ?? throw new Exception("Empty response from Sendblue"); } public async Task<EvaluateServiceResponse> EvaluateServiceAsync(string number) { var response = await _http.GetFromJsonAsync<EvaluateServiceResponse>( $"/api/evaluate-service?number={Uri.EscapeDataString(number)}"); return response ?? throw new Exception("Empty response"); } }`


Register the client in` Program.cs` :


` builder.Services.Configure<SendblueOptions>( builder.Configuration.GetSection("Sendblue")); builder.Services.AddHttpClient<SendblueClient>();`
