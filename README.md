# Static Code Analysis Report: Zomato: Food Delivery & Dining (com.application.zomato)

**Executive Summary:**

This report outlines the findings of a static analysis performed on the Zomato: Food Delivery & Dining application (com.application.zomato). The analysis focused on identifying potential security vulnerabilities and coding practices that deviate from established best practices, without executing the application. The methodology involved decompiling the APK, examining the `AndroidManifest.xml`, and scrutinizing the decompiled Java/Kotlin code. Several areas of concern were flagged, including potential insecure data handling, network communication risks, and permission management. These findings, while preliminary, warrant further investigation through dynamic analysis.

**Findings:**

-----------------------------------------------------------------
**Findings name:** Potential Plaintext Storage of API Keys
**CVSS Score:** 7.5 (High)
**Severity:** High
**CVSS Vector:** CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N
**Description of the finding:**
During code inspection, I observed several string resources and Java/Kotlin code segments that appeared to contain API keys. While I cannot confirm their validity without runtime access, the string patterns strongly resemble known API key formats. I found long, seemingly random alphanumeric strings within `strings.xml`. Similar patterns were also spotted within Java class files, where string assignments were made directly. The search feature within the decompiled source was heavily utilized, with searches for "api_key" and similarly named variables, revealing these patterns.
**Impact:**
* If the suspected API keys are valid, they could grant unauthorized access to Zomato's backend services.
* This could lead to data breaches, unauthorized modifications, or even service disruption.
**Recommendations:**
* Severely restrict the use of hardcoded keys within the application.
* Utilize environment variables or secure key management systems.
* Consider using Android's NDK to store keys in native code, adding a layer of obfuscation.
* Implement regular key rotation.
**References:**
* OWASP Mobile Top 10: M2 - Insecure Data Storage.
* Android Security Best Practices: [developer.android.com/topic/security/best-practices](https://developer.android.com/topic/security/best-practices)

-----------------------------------------------------------------
**Findings name:** Potential Overly Broad File System Permissions
**CVSS Score:** 4.3 (Medium)
**Severity:** Medium
**CVSS Vector:** CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:L/I:N/A:N
**Description of the finding:**
The `AndroidManifest.xml` file requests `WRITE_EXTERNAL_STORAGE`. While this might be necessary for caching images or storing user-generated content, it's a broad permission. Given the app's primary functionality, there's a
