Static Code Analysis Report: Zomato: Food Delivery & Dining (com.application.zomato)

Executive Summary:

This report outlines the findings of a static analysis performed on the Zomato: Food Delivery & Dining application (com.application.zomato). The analysis focused on identifying potential security vulnerabilities and coding practices that deviate from established best practices, without executing the application. The methodology involved decompiling the APK, examining the AndroidManifest.xml, and scrutinizing the decompiled Java/Kotlin code. Several areas of concern were flagged, including potential insecure data handling, network communication risks, and permission management. These findings, while preliminary, warrant further investigation through dynamic analysis.

Findings:

Findings name: Potential Plaintext Storage of API Keys
CVSS Score: 7.5 (High)
Severity: High
CVSS Vector: CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N
Description of the finding:
During code inspection, I observed several string resources and Java/Kotlin code segments that appeared to contain API keys. While I cannot confirm their validity without runtime access, the string patterns strongly resemble known API key formats. I found long, seemingly random alphanumeric strings within strings.xml. Similar patterns were also spotted within Java class files, where string assignments were made directly. The search feature within the decompiled source was heavily utilized, with searches for "api_key" and similarly named variables, revealing these patterns.
Impact:

If the suspected API keys are valid, they could grant unauthorized access to Zomato's backend services.
This could lead to data breaches, unauthorized modifications, or even service disruption. Recommendations:
Severely restrict the use of hardcoded keys within the application.
Utilize environment variables or secure key management systems.
Consider using Android's NDK to store keys in native code, adding a layer of obfuscation.
Implement regular key rotation. References:
OWASP Mobile Top 10: M2 - Insecure Data Storage.
Android Security Best Practices: developer.android.com/topic/security/best-practices
Findings name: Potential Overly Broad File System Permissions
CVSS Score: 4.3 (Medium)
Severity: Medium
CVSS Vector: CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:L/I:N/A:N
Description of the finding:
The AndroidManifest.xml file requests WRITE_EXTERNAL_STORAGE. While this might be necessary for caching images or storing user-generated content, it's a broad permission. Given the app's primary functionality, there's a possibility it's being overused. I observed this permission directly within the manifest file after using an android manifest viewer tool.
Impact:

Malicious apps could potentially exploit this permission to write arbitrary files to the device's external storage.
This could lead to data corruption or the installation of malware. Recommendations:
Carefully evaluate the necessity of WRITE_EXTERNAL_STORAGE.
If possible, switch to scoped storage or internal storage.
If external storage is required, restrict access to specific directories.
Implement runtime permission requests for API levels 23 and above. References:
Android Permissions Overview: developer.android.com/guide/topics/permissions/overview
Android scoped storage.
Findings name: Potential Use of Deprecated Libraries
CVSS Score: 3.1 (Low)
Severity: Low
CVSS Vector: CVSS:3.1/AV:L/AC:H/PR:N/UI:N/S:U/C:L/I:N/A:N
Description of the finding:
During decompilation, I observed the inclusion of older, potentially deprecated Android libraries. These libraries may contain known vulnerabilities or lack recent security updates. Specifically, I noticed imports referencing org.apache.http.*, which has been deprecated in recent Android versions. I used dex2jar and jd-gui to decompile and view the imports of the java files.
Impact:

Using deprecated libraries can expose the application to known vulnerabilities.
It can also hinder compatibility with newer Android versions. Recommendations:
Replace deprecated libraries with their modern equivalents.
Keep all libraries updated to the latest stable versions.
Utilize dependency management tools to ensure consistent library versions. References:
Android developer documentation on library updates.
Dependency management tools like Gradle.
