# Static Code Analysis Report: Zomato: Food Delivery & Dining (com.application.zomato)

**Executive Summary:**

This report details the static code analysis of the "Zomato: Food Delivery & Dining" application (com.application.zomato), a widely used food delivery and restaurant discovery app with over 100 million downloads on the Google Play Store. The analysis focused on potential security vulnerabilities and coding practice issues without executing the app, primarily by decompiling the APK and examining the code, manifest, and resources. Key findings include potential issues related to data storage, network communication, and permission handling. Static analysis limitations are acknowledged, and dynamic analysis is recommended for validation.

**Findings:**

-----------------------------------------------------------------
**Findings name:** Potential Insecure Network Communication via HTTP
**CVSS Score:** 8.1 (High)
**Severity:** High
**CVSS Vector:** CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:N
**Description of the finding:**
The decompiled code may reveal instances where the application uses HTTP instead of HTTPS for network communication, potentially exposing sensitive data transmitted between the app and the server. This could include user credentials, order details, and payment information.
**Screenshots and Proof of concepts:**
(Hypothetical code example)

* Hypothetical code example (Java/Kotlin):
    ```java
    URL url = new URL("[http://api.zomato.com/order](https://www.google.com/search?q=http://api.zomato.com/order)"); // Potential use of HTTP
    HttpURLConnection connection = (HttpURLConnection) url.openConnection();
    // ...
    ```
* If the above HTTP URL is used when transferring sensitive data, it is a vulnerability.
**Impact:**
* Man-in-the-middle attacks: Attackers can intercept and modify network traffic.
* Data exposure: Sensitive information can be read or stolen.
* Account compromise: User credentials can be compromised.
**Recommendations:**
* Enforce HTTPS for all network communication.
* Implement certificate pinning to prevent certificate spoofing.
* Use secure network libraries.
* Regularly audit network traffic for security vulnerabilities.
**References:**
* OWASP Mobile Top 10: M1 - Improper Platform Usage - [https://owasp.org/www-project-mobile-top-10/](https://owasp.org/www-project-mobile-top-10/)
* Android Developers: Network Security Configuration - [https://developer.android.com/training/articles/security-config](https://developer.android.com/training/articles/security-config)
-----------------------------------------------------------------
**Findings name:** Potential Exported Components Vulnerability
**CVSS Score:** 7.3 (High)
**Severity:** High
**CVSS Vector:** CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:L/A:L
**Description of the finding:**
The AndroidManifest.xml may contain exported activities, services, or broadcast receivers that are not properly protected. This could allow malicious apps to interact with these components, potentially leading to data leakage or unauthorized actions.
**Screenshots and Proof of concepts:**
(Hypothetical manifest example)

* Hypothetical Manifest Example:
    ```xml
    <activity android:name=".PaymentActivity" android:exported="true" />
    ```
* If PaymentActivity is exported without proper permission checks, any application on the device could launch it and potentially manipulate payment flow.
**Impact:**
* Unauthorized access to sensitive functionalities.
* Data injection or manipulation.
* Denial of service.
**Recommendations:**
* Set `android:exported="false"` for components that do not need to be accessed by other apps.
* Implement proper permission checks for exported components.
* Use `android:permission` attribute to restrict access.
* Validate all input received from external sources.
**References:**
* Android Developers: `<activity>` - [https://developer.android.com/guide/topics/manifest/activity-element](https://developer.android.com/guide/topics/manifest/activity-element)
* Android Developers: `<service>` - [https://developer.android.com/guide/topics/manifest/service-element](https://developer.android.com/guide/topics/manifest/service-element)
* Android Developers: `<receiver>` - [https://developer.android.com/guide/topics/manifest/receiver-element](https://developer.android.com/guide/topics/manifest/receiver-element)
-----------------------------------------------------------------
**Findings name:** Potential Improper Handling of Sensitive Data in Logs
**CVSS Score:** 5.3 (Medium)
**Severity:** Medium
**CVSS Vector:** CVSS:3.1/AV:L/AC:L/PR:N/UI:N/S:U/C:L/I:N/A:N
**Description of the finding:**
The decompiled code may reveal instances where sensitive data, such as user IDs, session tokens, or payment details, are logged using `Log.d` or other logging methods. These logs can be accessed by other apps with sufficient permissions or through ADB, potentially exposing sensitive information.
**Screenshots and Proof of concepts:**
(Hypothetical code example)

* Hypothetical code example (Java/Kotlin):
    ```java
    Log.d("Payment", "User ID: " + userId + ", Transaction ID: " + transactionId);
    ```
* If the above log statement is in production code, it is a vulnerability.
**Impact:**
* Data leakage: Sensitive information can be exposed to attackers.
* Privacy violation: User data can be compromised.
* Compliance issues.
**Recommendations:**
* Avoid logging sensitive data in production builds.
* Use appropriate log levels (e.g., `Log.e` for errors) and filter logs in production.
* Implement secure logging mechanisms.
* Use tools to scan for sensitive data in logs.
**References:**
* Android Developers: Log - [https://developer.android.com/reference/android/util/Log](https://developer.android.com/reference/android/util/Log)
* OWASP Mobile Top 10: M3 - Insecure Communication - [https://owasp.org/www-project-mobile-top-10/](https://owasp.org/www-project-mobile-top-10/)
