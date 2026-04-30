# Part A Task 1 and Task 3 Notes

This note is aligned with the current `assignment2-spec-2.pdf`, `assignment2-rubric-2.pdf`, `Presentation Script (Part A).md`, and `Assignment 2 (Part A) Report.docx`.

## Scope Check

For Part A, the spec says the only vulnerability class that will be graded is an insecure network transport or TLS configuration issue that could enable a MITM attack. The rubric also rewards strong code or config evidence and clear impact reasoning, and warns against mixing in unrelated vulnerabilities.

## Task 1: Unpack and Decompile the Network APK

The APK does not need to be decompiled again because the existing JADX output is already available in `parta/a2_case1-jadx/`.

### Tools and workflow used

- `jadx`
  Used to inspect decompiled Java and app resources.
- `rg`
  Used to search for manifest flags, network-related code, WebView usage, HTTP URLs, and TLS validation logic.
- Static inspection of the existing decoded output
  The provided JADX output already exposes the files needed for Task 1.

### Decompilation confirmation

The required artefacts are present in the existing output:

- Manifest:
  `parta/a2_case1-jadx/resources/AndroidManifest.xml`
- Main activity:
  `parta/a2_case1-jadx/sources/com/example/mastg_test0019/MainActivity.java`
- At least one network-request class/path:
  `parta/a2_case1-jadx/sources/com/example/mastg_test0019/MainActivity.java`

### Brief decompilation summary

We used the already-available JADX project output for `a2_case1.apk` rather than re-running the tool. From that output we confirmed access to the decoded manifest, the launch activity, and the app's network path. The main activity is `com.example.mastg_test0019.MainActivity`, and it contains the WebView request path that loads external content from `http://www.example.com`.

## Task 3: Find and Explain the Network Vulnerability

### Finding

The intended vulnerability is an insecure transport and TLS configuration issue that enables MITM-style attacks. The active insecure path is:

- cleartext traffic is permitted at the app level
- the app loads a plaintext HTTP URL into a WebView
- the WebView ignores TLS certificate errors by calling `proceed()`

Together, these behaviours allow an on-path attacker to read or modify content delivered to the app.

### Concise static evidence

1. Cleartext traffic is explicitly allowed in the manifest.

File:
`parta/a2_case1-jadx/resources/AndroidManifest.xml`

Evidence:

```xml
<uses-permission android:name="android.permission.INTERNET"/>
...
<application
    ...
    android:usesCleartextTraffic="true"
```

Important lines:

- line 13: `android.permission.INTERNET`
- line 26: `android:usesCleartextTraffic="true"`
- line 31: launch activity is `com.example.mastg_test0019.MainActivity`

2. The main activity loads an HTTP URL into a WebView.

File:
`parta/a2_case1-jadx/sources/com/example/mastg_test0019/MainActivity.java`

Evidence:

```java
WebView webView = (WebView) findViewById(R.id.webview);
...
webView.loadUrl("http://www.example.com");
```

Important lines:

- line 31: the `WebView` is obtained
- line 38: `webView.loadUrl("http://www.example.com");`

3. The WebView bypasses TLS warnings instead of failing closed.

File:
`parta/a2_case1-jadx/sources/com/example/mastg_test0019/MainActivity.java`

Evidence:

```java
public void onReceivedSslError(WebView webView2, SslErrorHandler sslErrorHandler, SslError sslError) {
    sslErrorHandler.proceed();
}
```

Important lines:

- line 34: `onReceivedSslError(...)`
- line 35: `sslErrorHandler.proceed();`

4. Supporting evidence: an always-true hostname verifier is present.

File:
`parta/a2_case1-jadx/sources/com/example/mastg_test0019/MainActivity.java`

Evidence:

```java
new HostnameVerifier() {
    @Override
    public boolean verify(String str, SSLSession sSLSession) {
        return true;
    }
};
```

Important lines:

- lines 39 to 43: `verify(...)` always returns `true`

This is supporting evidence of unsafe TLS validation logic. However, the strongest confirmed active path is still the manifest cleartext setting plus the HTTP WebView load plus the SSL error bypass.

### Plain-language explanation

An on-path attacker, such as someone on the same public Wi-Fi, can intercept traffic between the phone and `www.example.com`. Because the app loads `http://www.example.com`, that traffic is not encrypted. The attacker can read the content being loaded and modify the response before it reaches the app.

This is especially risky because the content is rendered inside the app's WebView. If an attacker changes the page, the user may believe the malicious content is trusted app content. The TLS error handler makes this worse because even if the app later uses HTTPS, the WebView is configured to continue after certificate errors instead of blocking the connection.

### Realistic impact

The confirmed impact is MITM-based disclosure and modification of WebView content. A realistic attacker could:

- read plaintext HTTP requests and responses
- tamper with server responses before the app renders them
- inject misleading or phishing-style content into the WebView
- impersonate a server more easily if TLS errors are ignored

For this APK, the static evidence confirms insecure transport behaviour, but it does not prove that credentials or personal data are definitely sent. So the safest wording is that the app enables content disclosure and modification, and could support credential or session theft if the same insecure path were used with sensitive pages.

## Recommended wording for the report or video

Use wording close to this:

> The primary Part A vulnerability is insecure transport handling that enables MITM-style attacks. The manifest permits cleartext traffic, the main activity loads `http://www.example.com` into a WebView, and the WebView continues on TLS certificate errors by calling `SslErrorHandler.proceed()`. This allows an on-path attacker to observe or modify content rendered inside the app.

## Screenshot Checklist

The spec and rubric do not explicitly require screenshots for Task 1 or Task 3. Code and config evidence is sufficient. However, screenshots are useful for the report or recorded presentation.

If you want screenshots, capture these exact locations:

1. `parta/a2_case1-jadx/resources/AndroidManifest.xml`
   Show lines 13, 18 to 29, and 30 to 37 so the tutor can see `INTERNET`, `usesCleartextTraffic="true"`, and `MainActivity`.
2. `parta/a2_case1-jadx/sources/com/example/mastg_test0019/MainActivity.java`
   Show lines 31 to 38 so the tutor can see the `WebView`, `onReceivedSslError`, `proceed()`, and `loadUrl("http://www.example.com")`.
3. Optional supporting screenshot:
   Show lines 39 to 43 in `MainActivity.java` for the always-true `HostnameVerifier`.

Save screenshots under:

- `parta/screenshots/manifest-cleartext-mainactivity.png`
- `parta/screenshots/mainactivity-http-sslerror.png`
- `parta/screenshots/mainactivity-hostnameverifier.png`

## Final alignment check

This analysis is consistent with the existing `Presentation Script (Part A).md` and `Assignment 2 (Part A) Report.docx`. The safest high-scoring approach is to keep the discussion tightly focused on the intended insecure transport and TLS issue, supported by concise manifest and code evidence.
