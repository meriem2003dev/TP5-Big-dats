
---

# * تقرير TP04: Docker — خطوات التنفيذ**

## **1. إنشاء حساب Docker Hub**

قمت بإنشاء حساب على منصة Docker Hub بهدف تخزين الصور (Images) الخاصة بالمشروع وإمكانية تحميلها وتشغيلها من أي جهاز آخر.

---

## **2. إنشاء Image خاصة بـ TP3**

أنشأت ملف **Dockerfile** داخل مجلد TP3 ليتم من خلاله بناء صورة المشروع.
يتضمن الملف:

* اختيار Python base image
* نسخ ملفات المشروع
* تثبيت المتطلبات باستخدام `requirements.txt`
* تحديد أمر تشغيل التطبيق

استخدمت الأمر التالي لبناء الصورة:

```bash
docker build -t mimi599/tp3-image:v1 .
```

---

## **3. رفع الصورة إلى Docker Hub**

بعد تسجيل الدخول باستخدام:

```bash
docker login
```

قمت برفع الصورة:

```bash
docker push mimi599/tp3-image:v1
```

---

## **4. تحميل الصورة من Docker Hub**

لتجربة تحميل الصورة واستخدامها، نفذت الأمر:

```bash
docker pull mimi599/tp3-image:v1
```

---

## **5. إنشاء ثلاث حاويات (Containers) من نفس الصورة**

أنشأت ثلاث حاويات مستقلة تعمل على منافذ مختلفة:

```bash
docker run -d --name cont1 -p 8081:80 mimi599/tp3-image:v1
docker run -d --name cont2 -p 8082:80 mimi599/tp3-image:v1
docker run -d --name cont3 -p 8083:80 mimi599/tp3-image:v1
```

---

## **6. إنشاء شبكة لربط الحاويات**

قمت بإنشاء شبكة Docker:

```bash
docker network create tp3-net
```

ثم ربطت الحاويات بهذه الشبكة:

```bash
docker network connect tp3-net cont1
docker network connect tp3-net cont2
docker network connect tp3-net cont3
```

---

## **7. التحقق من حالة الحاويات**

استخدمت الأمر:

```bash
docker ps
```

وتأكدت أن الحاويات الثلاث تعمل (Up) على المنافذ المحددة.

---

## **8. اختبار عمل التطبيق**

بعد تشغيل الحاويات، أصبحت الواجهات متاحة عبر:

* [http://localhost:8081](http://localhost:8081)
* [http://localhost:8082](http://localhost:8082)
* [http://localhost:8083](http://localhost:8083)


