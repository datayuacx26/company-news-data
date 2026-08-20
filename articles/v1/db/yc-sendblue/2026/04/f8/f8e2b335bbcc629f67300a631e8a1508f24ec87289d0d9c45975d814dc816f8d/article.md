---
schema_version: "1.0.0"
document_id: "f8e2b335bbcc629f67300a631e8a1508f24ec87289d0d9c45975d814dc816f8d"
company_key: "yc-sendblue"
company: "Sendblue"
source_id: "yc-sendblue-news-import-cbfb84d5bb49"
canonical_url: "https://www.sendblue.com/blog/imessage-api-kotlin"
published_at: "2026-04-05T00:00:00+00:00"
first_seen_at: "2026-07-22T13:05:24.258144+00:00"
fetched_at: "2026-07-28T21:38:24.318832+00:00"
content_hash: "sha256:cdb9192cba65faed0d645ded5530e66f720fe282c0f0f10b1c3f69c51391687d"
---

# iMessage API in Kotlin — Send Blue Bubbles from Android Apps (2026)

Use Kotlin coroutines with OkHttp's` enqueue` wrapped in a` suspendCancellableCoroutine` :


` import kotlinx.coroutines.suspendCancellableCoroutine import okhttp3.* import okhttp3.MediaType.Companion.toMediaType import okhttp3.RequestBody.Companion.toRequestBody import org.json.JSONObject import kotlin.coroutines.resume import kotlin.coroutines.resumeWithException object SendblueClient { private val client = OkHttpClient() private const val API_URL = "https://api.sendblue.co/api/send-message" private const val API_KEY_ID = BuildConfig.SENDBLUE_API_KEY_ID private const val API_SECRET_KEY = BuildConfig.SENDBLUE_API_SECRET_KEY suspend fun sendMessage( toNumber: String, content: String, sendStyle: String? = null, mediaUrl: String? = null ): JSONObject = suspendCancellableCoroutine { cont -> val json = JSONObject().apply { put("number", toNumber) put("content", content) sendStyle?.let { put("send_style", it) } mediaUrl?.let { put("media_url", it) } } val body = json.toString() .toRequestBody("application/json".toMediaType()) val request = Request.Builder() .url(API_URL) .post(body) .addHeader("sb-api-key-id", API_KEY_ID) .addHeader("sb-api-secret-key", API_SECRET_KEY) .build() val call = client.newCall(request) cont.invokeOnCancellation { call.cancel() } call.enqueue(object : Callback { override fun onFailure(call: Call, e: IOException) { cont.resumeWithException(e) } override fun onResponse(call: Call, response: Response) { val body = response.body?.string() ?: "{}" cont.resume(JSONObject(body)) } }) } } // Call from a ViewModel or coroutine scope: viewModelScope.launch { try { val result = SendblueClient.sendMessage( toNumber = "+14155551234", content = "Hello from Android via iMessage!", sendStyle = "invisible" ) Log.d("Sendblue", "Status: ${result.getString("status")}") } catch (e: Exception) { Log.e("Sendblue", "Failed", e) } }`
