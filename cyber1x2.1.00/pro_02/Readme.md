# Exercise 02: SSRF & LFI Vulnerabilities[cite: 6, 7]

## 1. Introduction and Vulnerability Definitions
As part of our exploration into web application security and referencing OWASP guidelines[cite: 7], this project focuses on two critical vulnerabilities that we successfully chained together:
*   **SSRF (Server-Side Request Forgery):** A flaw that occurs when a web application fetches a remote resource without validating the user-supplied URL. It allows us to coerce the server into making arbitrary requests on our behalf.
*   **LFI (Local File Inclusion):** A vulnerability that allows an attacker to read local files on the server running the application.

## 2. Methodology: How We Reached the Solution
To solve this challenge, we followed a structured analytical approach:
1.  **Environment Analysis:** We started the provided Docker container and accessed the web application via the browser[cite: 6]. 
2.  **Identifying the Vector:** We observed that the web interface provides a feature to fetch content from a user-provided URL through an "Enter URL" input field[cite: 7]. This indicates that the backend server is acting as a client to fetch external resources.
3.  **Chaining the Vulnerabilities:** We hypothesized that if the backend does not strictly validate the input to only allow `http://` or `https://` schemes, we could abuse it. By changing the protocol to `file://`, we can transition from a simple SSRF (forcing the server to execute our request) to an LFI (instructing the server to read its own local files).

## 3. Execution and Confirmation
To prove the existence of this vulnerability, we crafted and injected the following payload into the input field:
`file:///etc/passwd`

Upon submitting the request, the application successfully returned the full content of the Linux user configuration file, displaying the expected output starting with `root:x:0:0:root:/root:/bin/bash`[cite: 7]. This output definitively confirmed that the server processed our unauthorized protocol, bypassed standard web directories, and exposed its internal file system.

## 4. Exploitation Scenarios[cite: 8]
Based on our findings, this vulnerability can lead to severe consequences in real-world applications:
1.  **Local File Disclosure:** An attacker can systematically read sensitive files from the server, such as application source codes, configuration files containing database credentials, or system files.
2.  **Internal Network Reconnaissance (Pivoting):** An attacker can use the vulnerable server as a proxy to bypass external firewalls. By supplying internal IP addresses (e.g., `http://192.168.1.10/admin` or `http://localhost:80`), the attacker can map the internal network or access internal administrative dashboards. They can also target cloud metadata endpoints (e.g., AWS metadata at `169.254.169.254`).

---

# التحدي الثاني: ثغرات SSRF و LFI[cite: 6, 7]

## 1. مقدمة وتعريف الثغرات
كجزء من استكشافنا لأمن تطبيقات الويب وبالرجوع إلى إرشادات OWASP[cite: 7]، يركز هذا المشروع على ثغرتين حرجتين قمنا بربطهما معاً بنجاح:
*   **SSRF (تزوير طلبات الخادم):** ثغرة تحدث عندما يقوم تطبيق الويب بجلب مورد خارجي دون التحقق من الرابط الذي أدخله المستخدم. هذا يسمح لنا بإجبار الخادم على تنفيذ طلبات نيابة عنا.
*   **LFI (تضمين الملفات المحلية):** ثغرة تسمح للمهاجم بقراءة الملفات المحلية الموجودة على الخادم الذي يستضيف التطبيق.

## 2. المنهجية: كيف وصلنا إلى الحل
لحل هذا التحدي، اتبعنا نهجاً تحليلياً منظماً:
1.  **تحليل البيئة:** قمنا بتشغيل حاوية Docker المطلوبة ودخلنا إلى تطبيق الويب عبر المتصفح[cite: 6].
2.  **تحديد مسار الهجوم:** لاحظنا أن واجهة الويب توفر ميزة لجلب المحتوى بناءً على رابط يقدمه المستخدم من خلال حقل "Enter URL"[cite: 7]. هذا يشير إلى أن الخادم الخلفي يتصرف كعميل (Client) لجلب موارد خارجية.
3.  **ربط الثغرات (Chaining):** افترضنا أنه إذا كان الخادم الخلفي لا يتحقق بصرامة من المدخلات للسماح فقط ببروتوكولات `http://` أو `https://`، فيمكننا استغلال ذلك. من خلال تغيير البروتوكول إلى `file://`، يمكننا الانتقال من ثغرة SSRF بسيطة (إجبار الخادم على تنفيذ طلبنا) إلى ثغرة LFI (توجيه الخادم لقراءة ملفاته المحلية).

## 3. التنفيذ والتأكيد
لإثبات وجود هذه الثغرة، قمنا بصياغة وإدخال الحمولة (Payload) التالية في حقل الرابط:
`file:///etc/passwd`

بمجرد إرسال الطلب، قام التطبيق بنجاح بإرجاع المحتوى الكامل لملف تكوين مستخدمي نظام لينكس، وعرض المخرجات المتوقعة التي تبدأ بـ `root:x:0:0:root:/root:/bin/bash`[cite: 7]. هذا المخرج أكد بشكل قاطع أن الخادم قام بمعالجة البروتوكول غير المصرح به، وتجاوز مجلدات الويب القياسية، وكشف عن نظام ملفاته الداخلي.

## 4. سيناريوهات الاستغلال[cite: 8]
بناءً على ما توصلنا إليه، يمكن أن تؤدي هذه الثغرة إلى عواقب وخيمة في التطبيقات الواقعية:
1.  **كشف الملفات المحلية:** يمكن للمهاجم قراءة ملفات حساسة من الخادم بشكل منهجي، مثل الأكواد المصدرية للتطبيق، أو ملفات الإعدادات التي تحتوي على كلمات مرور قواعد البيانات، أو ملفات النظام.
2.  **استطلاع الشبكة الداخلية (Pivoting):** يمكن للمهاجم استخدام الخادم المصاب كوكيل (Proxy) لتجاوز جدران الحماية الخارجية. من خلال إدخال عناوين IP داخلية (مثل `http://192.168.1.10/admin` أو `http://localhost:80`)، يمكن للمهاجم فحص الشبكة الداخلية أو الوصول إلى لوحات تحكم الإدارة الداخلية. يمكنهم أيضاً استهداف نقاط النهاية الخاصة ببيانات الخدمات السحابية الوصفية (مثل AWS على `169.254.169.254`).
