شرح الباكجات:
express → لإنشاء السيرفر.


mongoose → للتعامل مع MongoDB.

dotenv → لإدارة المتغيرات البيئية.

bcryptjs → لتشفير كلمات المرور.

jsonwebtoken → لإنشاء الـ JWT authentication.

cors → للسماح بطلبات الـ API من أي دومين.

multer, multer-s3, aws-sdk → لرفع الملفات والصور.

nodemon → لإعادة تشغيل السيرفر تلقائيًا أثناء التطوير.

////////////////////////////////////////

ecommerce
│
├── models/              # نماذج MongoDB
│   ├── User.js
│   ├── Product.js
│   ├── Order.js
│   └── Category.js
|      └── User.js
│
├── controllers/         # المنطق الخاص بـ APIs
│   ├── authController.js
│   ├── productController.js
│   ├── orderController.js
│   └── categoryController.js
|      └── productController.js
|      └── cartController.js

│
├── routes/              # Routes
│   ├── authRoutes.js
│   ├── productRoutes.js
│   ├── orderRoutes.js
│   └── categoryRoutes.js
|      └── productRoutes.js
|      └── cartRoutes.js


│
├── middleware/          # Middleware
│   └── authMiddleware.js
│
├── config/              # إعدادات MongoDB
│   └── db.js
│
├── .env                 # ملف البيئة
├── server.js            # ملف بدء التشغيل
└── package.json





✅ النتيجة النهائية
الآن عند تسجيل الدخول، سيرجع الدور (role) مع بيانات المستخدم، مما يسمح لك بالتحكم في الصلاحيات:
✔️ المستخدم العادي (user): يستطيع شراء المنتجات فقط
✔️ الأدمن (admin): يستطيع 
إضافة، تعديل، وحذف الفئات، المنتجات، والطلبات





📌 تصميم قاعدة البيانات لمشروع E-commerce باستخدام MongoDB
بما إنك بتستخدم MongoDB، هتحتاج تعمل Collections بدل الجداول (لأن MongoDB NoSQL).

📂 هيكلة قاعدة البيانات
📌 هتكون عندك 4 Collections رئيسية:
1️⃣ users → تخزين بيانات المستخدمين
2️⃣ products → تخزين المنتجات
3️⃣ categories → تصنيف المنتجات
4️⃣ orders → تخزين الطلبات





📌 تصميم الجداول (Collections)
1️⃣ Collection: users (المستخدمين)
📌 لتخزين بيانات المستخدمين، هيكون عندك:

username: اسم المستخدم
email: البريد الإلكتروني (فريد unique)
password: كلمة المرور (بعد التشفير)
role: دور المستخدم (admin أو user)
createdAt: 
تاريخ التسجيل


ex======================
{
  "username": "ahmed123",
  "email": "ahmed@example.com",
  "password": "sddsdsdsd",
  "role": "user",
}





2️⃣ Collection: categories (التصنيفات)
📌 كل منتج بينتمي إلى تصنيف معين مثل "هواتف"، "ملابس"، فهنخزن:

name: اسم التصنيف
createdAt: وقت الإضافة
📍 الشكل النهائي في MongoDB:


{
  "name": "Electronics",
  "description":"dsdsds"
}




3️⃣ Collection: products (المنتجات)
📌 كل منتج بيكون له:

name: اسم المنتج
price: السعر
category: معرف التصنيف (category_id)
stock: الكمية المتاحة
description: وصف المنتج
images: صور المنتج
createdAt: 
وقت الإضافة


{
  "_id": "650ab5f2e23a5a7a4c8b4569",
  "name": "iPhone 15",
  "price": 1200,
  "category": "650ab4e7e23a5a7a4c8b4568",
  "stock": 50,
  "description": "New iPhone with A16 Bionic chip.",
  "images": [
    "https://example.com/iphone1.jpg",
    "https://example.com/iphone2.jpg"
  ],
  "createdAt": "2024-02-02T12:10:00Z"
}











4️⃣ Collection: orders (الطلبات)
📌 كل طلب بيكون مرتبط بـ مستخدم معين (user_id) ومنتجات (product_id) وكمان هيكون عندك:

user: معرف المستخدم اللي طلب المنتجات
products: قائمة بالمنتجات المطلوبة وعددها
totalPrice: إجمالي السعر
status: حالة الطلب (Pending, Shipped, Delivered)
createdAt: تاريخ الطلب



{
  "_id": "650ab6e4e23a5a7a4c8b4570",
  "user": "650ab3f0e23a5a7a4c8b4567",
  "products": [
    {
      "product": "650ab5f2e23a5a7a4c8b4569",
      "quantity": 2
    }
  ],
  "totalPrice": 2400,
  "status": "Pending",
  "createdAt": "2024-02-02T12:20:00Z"
}





📌 العلاقات بين الـ Collections
📍 العلاقات بتكون كالتالي:

كل Product بينتمي إلى Category (category_id).
كل Order مرتبط بـ User (user_id).
كل Order يحتوي على منتجات متعددة (product_id مع quantity).




















db.users.insertOne({
  "username": "ahmed123",
  "email": "ahmed@example.com",
  "password": "$2a$10$encryptedpassword",
  "role": "user",
  "createdAt": new Date()
})

db.categories.insertOne({
  "name": "Electronics",
  "createdAt": new Date()
})

db.products.insertOne({
  "name": "iPhone 15",
  "price": 1200,
  "category": ObjectId("650ab4e7e23a5a7a4c8b4568"),
  "stock": 50,
  "description": "New iPhone with A16 Bionic chip.",
  "images": ["https://example.com/iphone1.jpg"],
  "createdAt": new Date()
})

db.orders.insertOne({
  "user": ObjectId("650ab3f0e23a5a7a4c8b4567"),
  "products": [
    { "product": ObjectId("650ab5f2e23a5a7a4c8b4569"), "quantity": 2 }
  ],
  "totalPrice": 2400,
  "status": "Pending",
  "createdAt": new Date()
})


