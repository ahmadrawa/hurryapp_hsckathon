# دليل تعلم Laravel | Laravel Learning Guide

## نبذة عن Laravel | About Laravel

**العربية:**
Laravel هو إطار عمل PHP مفتوح المصدر، يُعتبر من أشهر وأقوى أطر العمل لتطوير تطبيقات الويب. تم إنشاؤه بواسطة Taylor Otwell في عام 2011، ويتميز بسهولة الاستخدام، البنية النظيفة، والوثائق الممتازة.

**English:**
Laravel is an open-source PHP framework and one of the most popular and powerful frameworks for web application development. Created by Taylor Otwell in 2011, it stands out for its ease of use, clean architecture, and excellent documentation.

---

## المميزات الرئيسية | Key Features

### 1. **Eloquent ORM**
- نظام قوي للتعامل مع قواعد البيانات بطريقة كائنية التوجه
- A powerful system for database interaction using object-oriented approach

### 2. **Blade Template Engine**
- محرك قوالب سهل ومرن
- Easy and flexible template engine

### 3. **Routing System**
- نظام توجيه بسيط وقوي
- Simple yet powerful routing system

### 4. **Migration & Seeding**
- إدارة قاعدة البيانات بطريقة منظمة
- Organized database management

### 5. **Authentication & Authorization**
- نظام مدمج للمصادقة والتفويض
- Built-in authentication and authorization system

---

## المتطلبات | Prerequisites

### التثبيت | Installation Requirements

```bash
# المتطلبات الأساسية | Basic Requirements
- PHP >= 8.1
- Composer
- MySQL/PostgreSQL/SQLite
- Web Server (Apache/Nginx)

# تثبيت Composer | Install Composer
# Visit: https://getcomposer.org/download/
```

---

## البدء مع Laravel | Getting Started with Laravel

### 1. إنشاء مشروع جديد | Create New Project

```bash
# باستخدام Composer | Using Composer
composer create-project laravel/laravel my-app

# أو باستخدام Laravel Installer | Or using Laravel Installer
composer global require laravel/installer
laravel new my-app

# الانتقال إلى المشروع | Navigate to project
cd my-app

# تشغيل الخادم | Start development server
php artisan serve
```

الآن يمكنك زيارة: `http://localhost:8000`
Now you can visit: `http://localhost:8000`

---

## هيكل المشروع | Project Structure

```
my-app/
├── app/                    # منطق التطبيق | Application logic
│   ├── Http/
│   │   ├── Controllers/   # المتحكمات | Controllers
│   │   └── Middleware/    # الوسيطات | Middleware
│   ├── Models/            # النماذج | Models
│   └── Providers/         # مزودي الخدمات | Service providers
├── config/                # ملفات الإعدادات | Configuration files
├── database/
│   ├── migrations/        # هجرات قاعدة البيانات | Database migrations
│   └── seeders/           # بيانات البذر | Database seeders
├── public/                # الملفات العامة | Public files
├── resources/
│   ├── views/             # ملفات Blade | Blade templates
│   ├── css/               # ملفات CSS | CSS files
│   └── js/                # ملفات JavaScript | JavaScript files
├── routes/
│   ├── web.php            # مسارات الويب | Web routes
│   └── api.php            # مسارات API | API routes
├── storage/               # الملفات المُخزنة | Storage files
├── tests/                 # الاختبارات | Tests
├── .env                   # المتغيرات البيئية | Environment variables
└── artisan                # أداة سطر الأوامر | Command-line tool
```

---

## الأساسيات | Fundamentals

### 1. التوجيه (Routing) | Routing

**ملف: `routes/web.php` | File: `routes/web.php`**

```php
<?php

use Illuminate\Support\Facades\Route;
use App\Http\Controllers\HomeController;
use App\Http\Controllers\UserController;

// مسار أساسي | Basic route
Route::get('/', function () {
    return view('welcome');
});

// مسار مع معامل | Route with parameter
Route::get('/user/{id}', function ($id) {
    return "User ID: " . $id;
});

// مسار مع متحكم | Route with controller
Route::get('/home', [HomeController::class, 'index']);

// مجموعة مسارات | Route group
Route::prefix('admin')->group(function () {
    Route::get('/dashboard', function () {
        return view('admin.dashboard');
    });
    
    Route::get('/users', [UserController::class, 'index']);
});

// أنواع المسارات المختلفة | Different route types
Route::get('/posts', [PostController::class, 'index']);      // عرض | Display
Route::post('/posts', [PostController::class, 'store']);     // إنشاء | Create
Route::put('/posts/{id}', [PostController::class, 'update']); // تحديث | Update
Route::delete('/posts/{id}', [PostController::class, 'destroy']); // حذف | Delete

// مسار الموارد (يُنشئ كل المسارات تلقائياً) | Resource route (creates all routes automatically)
Route::resource('articles', ArticleController::class);
```

---

### 2. المتحكمات (Controllers) | Controllers

**إنشاء متحكم | Create Controller:**

```bash
# إنشاء متحكم | Create controller
php artisan make:controller UserController

# إنشاء متحكم موارد | Create resource controller
php artisan make:controller PostController --resource
```

**مثال على متحكم | Controller Example:**

**ملف: `app/Http/Controllers/UserController.php` | File: `app/Http/Controllers/UserController.php`**

```php
<?php

namespace App\Http\Controllers;

use App\Models\User;
use Illuminate\Http\Request;

class UserController extends Controller
{
    // عرض كل المستخدمين | Display all users
    public function index()
    {
        $users = User::all();
        return view('users.index', compact('users'));
    }

    // عرض مستخدم واحد | Show single user
    public function show($id)
    {
        $user = User::findOrFail($id);
        return view('users.show', compact('user'));
    }

    // عرض نموذج الإنشاء | Show create form
    public function create()
    {
        return view('users.create');
    }

    // حفظ مستخدم جديد | Store new user
    public function store(Request $request)
    {
        $validated = $request->validate([
            'name' => 'required|max:255',
            'email' => 'required|email|unique:users',
            'password' => 'required|min:8',
        ]);

        $user = User::create([
            'name' => $validated['name'],
            'email' => $validated['email'],
            'password' => bcrypt($validated['password']),
        ]);

        return redirect()->route('users.show', $user->id)
                         ->with('success', 'User created successfully!');
    }

    // عرض نموذج التعديل | Show edit form
    public function edit($id)
    {
        $user = User::findOrFail($id);
        return view('users.edit', compact('user'));
    }

    // تحديث المستخدم | Update user
    public function update(Request $request, $id)
    {
        $user = User::findOrFail($id);
        
        $validated = $request->validate([
            'name' => 'required|max:255',
            'email' => 'required|email|unique:users,email,' . $id,
        ]);

        $user->update($validated);

        return redirect()->route('users.show', $user->id)
                         ->with('success', 'User updated successfully!');
    }

    // حذف المستخدم | Delete user
    public function destroy($id)
    {
        $user = User::findOrFail($id);
        $user->delete();

        return redirect()->route('users.index')
                         ->with('success', 'User deleted successfully!');
    }
}
```

---

### 3. العروض (Views) مع Blade | Views with Blade

**إنشاء ملف View | Create View File:**

**ملف: `resources/views/users/index.blade.php` | File: `resources/views/users/index.blade.php`**

```blade
<!DOCTYPE html>
<html>
<head>
    <title>المستخدمين | Users</title>
</head>
<body>
    <h1>قائمة المستخدمين | Users List</h1>

    @if(session('success'))
        <div class="alert alert-success">
            {{ session('success') }}
        </div>
    @endif

    <a href="{{ route('users.create') }}">إضافة مستخدم جديد | Add New User</a>

    <table>
        <thead>
            <tr>
                <th>الرقم | ID</th>
                <th>الاسم | Name</th>
                <th>البريد الإلكتروني | Email</th>
                <th>الإجراءات | Actions</th>
            </tr>
        </thead>
        <tbody>
            @foreach($users as $user)
                <tr>
                    <td>{{ $user->id }}</td>
                    <td>{{ $user->name }}</td>
                    <td>{{ $user->email }}</td>
                    <td>
                        <a href="{{ route('users.show', $user->id) }}">عرض | View</a>
                        <a href="{{ route('users.edit', $user->id) }}">تعديل | Edit</a>
                        
                        <form action="{{ route('users.destroy', $user->id) }}" method="POST" style="display: inline;">
                            @csrf
                            @method('DELETE')
                            <button type="submit">حذف | Delete</button>
                        </form>
                    </td>
                </tr>
            @endforeach

            @if($users->isEmpty())
                <tr>
                    <td colspan="4">لا يوجد مستخدمين | No users found</td>
                </tr>
            @endif
        </tbody>
    </table>
</body>
</html>
```

**تخطيط عام | Master Layout:**

**ملف: `resources/views/layouts/app.blade.php` | File: `resources/views/layouts/app.blade.php`**

```blade
<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>@yield('title', 'Laravel App')</title>
    <link rel="stylesheet" href="{{ asset('css/app.css') }}">
</head>
<body>
    <nav>
        <ul>
            <li><a href="{{ route('home') }}">الرئيسية | Home</a></li>
            <li><a href="{{ route('users.index') }}">المستخدمين | Users</a></li>
        </ul>
    </nav>

    <main>
        @yield('content')
    </main>

    <footer>
        <p>&copy; 2024 My Laravel App</p>
    </footer>

    <script src="{{ asset('js/app.js') }}"></script>
</body>
</html>
```

**استخدام التخطيط | Using Layout:**

```blade
@extends('layouts.app')

@section('title', 'المستخدمين | Users')

@section('content')
    <h1>محتوى الصفحة | Page Content</h1>
    <p>هذا المحتوى سيُدرج في @yield('content')</p>
@endsection
```

---

### 4. النماذج (Models) و Eloquent ORM

**إنشاء نموذج | Create Model:**

```bash
# إنشاء نموذج فقط | Create model only
php artisan make:model Post

# إنشاء نموذج مع الهجرة | Create model with migration
php artisan make:model Post -m

# إنشاء نموذج مع الهجرة والمتحكم | Create model with migration and controller
php artisan make:model Post -mc

# إنشاء نموذج كامل (نموذج + هجرة + متحكم + سيدر + فاكتوري)
# Create complete model (model + migration + controller + seeder + factory)
php artisan make:model Post -a
```

**مثال على نموذج | Model Example:**

**ملف: `app/Models/Post.php` | File: `app/Models/Post.php`**

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
    use HasFactory;

    // الحقول القابلة للملء | Fillable fields
    protected $fillable = [
        'title',
        'content',
        'user_id',
        'published_at',
    ];

    // تحويل الحقول | Cast attributes
    protected $casts = [
        'published_at' => 'datetime',
    ];

    // العلاقة: المنشور ينتمي لمستخدم | Relationship: Post belongs to User
    public function user()
    {
        return $this->belongsTo(User::class);
    }

    // العلاقة: المنشور له تعليقات متعددة | Relationship: Post has many Comments
    public function comments()
    {
        return $this->hasMany(Comment::class);
    }

    // Accessor: تنسيق العنوان | Accessor: Format title
    public function getTitleAttribute($value)
    {
        return ucfirst($value);
    }

    // Mutator: تحويل العنوان لأحرف صغيرة قبل الحفظ
    // Mutator: Convert title to lowercase before saving
    public function setTitleAttribute($value)
    {
        $this->attributes['title'] = strtolower($value);
    }

    // Scope: فلترة المنشورات المنشورة | Scope: Filter published posts
    public function scopePublished($query)
    {
        return $query->whereNotNull('published_at');
    }
}
```

**استخدام Eloquent | Using Eloquent:**

```php
use App\Models\Post;
use App\Models\User;

// استعلامات بسيطة | Simple queries

// جلب كل السجلات | Get all records
$posts = Post::all();

// جلب سجل واحد | Get single record
$post = Post::find(1);
$post = Post::where('title', 'Laravel Guide')->first();

// إنشاء سجل جديد | Create new record
$post = Post::create([
    'title' => 'My First Post',
    'content' => 'This is the content',
    'user_id' => 1,
]);

// تحديث سجل | Update record
$post = Post::find(1);
$post->title = 'Updated Title';
$post->save();

// أو | Or
Post::where('id', 1)->update(['title' => 'Updated Title']);

// حذف سجل | Delete record
$post = Post::find(1);
$post->delete();

// أو | Or
Post::destroy(1);
Post::destroy([1, 2, 3]);

// استعلامات متقدمة | Advanced queries

// مع شروط | With conditions
$posts = Post::where('user_id', 1)
            ->where('published_at', '>=', now()->subDays(7))
            ->orderBy('created_at', 'desc')
            ->get();

// مع الصفحات | With pagination
$posts = Post::paginate(15);

// مع العلاقات (Eager Loading) | With relationships (Eager Loading)
$posts = Post::with('user', 'comments')->get();

// باستخدام Scopes | Using scopes
$publishedPosts = Post::published()->get();

// عد السجلات | Count records
$count = Post::where('user_id', 1)->count();

// التجميع | Aggregations
$average = Post::avg('views');
$max = Post::max('views');
$sum = Post::sum('views');
```

---

### 5. هجرات قاعدة البيانات | Database Migrations

**إنشاء هجرة | Create Migration:**

```bash
# إنشاء جدول جديد | Create new table
php artisan make:migration create_posts_table

# تعديل جدول موجود | Modify existing table
php artisan make:migration add_status_to_posts_table --table=posts
```

**مثال على هجرة | Migration Example:**

**ملف: `database/migrations/2024_01_01_000000_create_posts_table.php`**

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    // تشغيل الهجرة | Run migration
    public function up(): void
    {
        Schema::create('posts', function (Blueprint $table) {
            $table->id();
            $table->string('title');
            $table->text('content');
            $table->foreignId('user_id')->constrained()->onDelete('cascade');
            $table->string('status')->default('draft');
            $table->integer('views')->default(0);
            $table->timestamp('published_at')->nullable();
            $table->timestamps();
            
            // فهرس | Index
            $table->index('status');
            $table->index('published_at');
        });
    }

    // التراجع عن الهجرة | Rollback migration
    public function down(): void
    {
        Schema::dropIfExists('posts');
    }
};
```

**تشغيل الهجرات | Run Migrations:**

```bash
# تشغيل كل الهجرات | Run all migrations
php artisan migrate

# التراجع عن آخر دفعة | Rollback last batch
php artisan migrate:rollback

# التراجع عن كل الهجرات | Rollback all migrations
php artisan migrate:reset

# التراجع والتشغيل مرة أخرى | Rollback and re-run
php artisan migrate:refresh

# التراجع والتشغيل مع البيانات الوهمية | Rollback, re-run with seeders
php artisan migrate:refresh --seed

# حالة الهجرات | Migration status
php artisan migrate:status
```

---

### 6. المصادقة (Authentication) | Authentication

**تثبيت Laravel Breeze (بسيط) | Install Laravel Breeze (Simple):**

```bash
composer require laravel/breeze --dev
php artisan breeze:install
php artisan migrate
npm install && npm run dev
```

**استخدام المصادقة اليدوية | Manual Authentication:**

```php
use Illuminate\Support\Facades\Auth;
use Illuminate\Support\Facades\Hash;
use App\Models\User;

// تسجيل الدخول | Login
if (Auth::attempt(['email' => $email, 'password' => $password])) {
    $request->session()->regenerate();
    return redirect()->intended('dashboard');
}

// تسجيل الخروج | Logout
Auth::logout();
$request->session()->invalidate();
$request->session()->regenerateToken();

// التحقق من المصادقة | Check authentication
if (Auth::check()) {
    // المستخدم مصادق عليه | User is authenticated
}

// الحصول على المستخدم الحالي | Get current user
$user = Auth::user();
$userId = Auth::id();

// حماية المسارات | Protect routes
Route::get('/dashboard', function () {
    return view('dashboard');
})->middleware('auth');
```

---

### 7. التحقق من البيانات (Validation) | Validation

```php
use Illuminate\Http\Request;

public function store(Request $request)
{
    // التحقق الأساسي | Basic validation
    $validated = $request->validate([
        'title' => 'required|max:255',
        'content' => 'required',
        'email' => 'required|email|unique:users',
        'age' => 'required|integer|min:18|max:100',
        'website' => 'nullable|url',
        'image' => 'required|image|mimes:jpeg,png,jpg|max:2048',
        'password' => 'required|min:8|confirmed',
        'terms' => 'accepted',
    ]);

    // رسائل مخصصة | Custom messages
    $validated = $request->validate([
        'title' => 'required|max:255',
        'email' => 'required|email',
    ], [
        'title.required' => 'العنوان مطلوب | Title is required',
        'title.max' => 'العنوان طويل جداً | Title is too long',
        'email.required' => 'البريد الإلكتروني مطلوب | Email is required',
        'email.email' => 'البريد الإلكتروني غير صحيح | Invalid email',
    ]);

    // في Blade عرض الأخطاء | In Blade display errors
    @error('title')
        <div class="alert alert-danger">{{ $message }}</div>
    @enderror

    // عرض كل الأخطاء | Display all errors
    @if ($errors->any())
        <div class="alert alert-danger">
            <ul>
                @foreach ($errors->all() as $error)
                    <li>{{ $error }}</li>
                @endforeach
            </ul>
        </div>
    @endif
}
```

---

## المواضيع المتقدمة | Advanced Topics

### 1. العلاقات في Eloquent | Eloquent Relationships

```php
// واحد لواحد | One to One
class User extends Model {
    public function profile() {
        return $this->hasOne(Profile::class);
    }
}

class Profile extends Model {
    public function user() {
        return $this->belongsTo(User::class);
    }
}

// واحد لمتعدد | One to Many
class Post extends Model {
    public function comments() {
        return $this->hasMany(Comment::class);
    }
}

class Comment extends Model {
    public function post() {
        return $this->belongsTo(Post::class);
    }
}

// متعدد لمتعدد | Many to Many
class User extends Model {
    public function roles() {
        return $this->belongsToMany(Role::class);
    }
}

class Role extends Model {
    public function users() {
        return $this->belongsToMany(User::class);
    }
}

// استخدام العلاقات | Using relationships
$user = User::find(1);
$profile = $user->profile;
$posts = $user->posts;
$roles = $user->roles;

// إنشاء سجلات مرتبطة | Create related records
$user->posts()->create([
    'title' => 'New Post',
    'content' => 'Content here'
]);

// ربط السجلات (Many to Many) | Attach records (Many to Many)
$user->roles()->attach($roleId);
$user->roles()->detach($roleId);
$user->roles()->sync([1, 2, 3]);
```

---

### 2. الوسيطات (Middleware) | Middleware

**إنشاء وسيط | Create Middleware:**

```bash
php artisan make:middleware CheckAge
```

**مثال على وسيط | Middleware Example:**

```php
<?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Http\Request;

class CheckAge
{
    public function handle(Request $request, Closure $next)
    {
        if ($request->age <= 18) {
            return redirect('home');
        }

        return $next($request);
    }
}
```

---

## أفضل الممارسات | Best Practices

### 1. الأمان | Security

```php
// استخدم CSRF Protection | Use CSRF Protection
@csrf

// استخدم Mass Assignment Protection | Use Mass Assignment Protection
protected $fillable = ['name', 'email'];
protected $guarded = ['id', 'is_admin'];

// تشفير كلمات المرور | Hash passwords
$user->password = Hash::make($password);
$user->password = bcrypt($password);

// تنظيف المدخلات | Sanitize inputs
$clean = strip_tags($input);
$clean = htmlspecialchars($input);
```

### 2. الأداء | Performance

```php
// استخدم Eager Loading لتجنب N+1 Problem
// Use Eager Loading to avoid N+1 Problem
$posts = Post::with('user', 'comments')->get();

// استخدم التخزين المؤقت | Use caching
$users = Cache::remember('users', 3600, function () {
    return User::all();
});

// استخدم القوائم المجزأة | Use pagination
$posts = Post::paginate(15);
```

---

## أوامر Artisan المفيدة | Useful Artisan Commands

```bash
# عرض كل الأوامر | List all commands
php artisan list

# تنظيف الكاش | Clear cache
php artisan cache:clear
php artisan config:clear
php artisan route:clear
php artisan view:clear

# إنشاء المفاتيح | Generate keys
php artisan key:generate

# عرض المسارات | List routes
php artisan route:list

# إنشاء رابط للتخزين | Create storage link
php artisan storage:link

# إنشاء وحدة تحكم | Create controller
php artisan make:controller PostController

# إنشاء نموذج | Create model
php artisan make:model Post -m

# إنشاء هجرة | Create migration
php artisan make:migration create_posts_table

# تشغيل الاختبارات | Run tests
php artisan test
```

---

## مصادر التعلم | Learning Resources

### الوثائق الرسمية | Official Documentation
- **الموقع الرسمي | Official Website:** https://laravel.com
- **الوثائق | Documentation:** https://laravel.com/docs
- **Laracasts (فيديوهات تعليمية | Video Tutorials):** https://laracasts.com

### دورات مجانية | Free Courses
- **Laravel من الصفر (عربي) | Laravel from Scratch (Arabic):**
  - YouTube channels: TheNewBaghdad, Elzero Web School, Muhammed Essa
- **Laravel Bootcamp:** https://bootcamp.laravel.com
- **Laravel Daily:** https://laraveldaily.com

### الكتب | Books
- **Laravel: Up & Running** by Matt Stauffer
- **Laravel: From Apprentice To Artisan** by Taylor Otwell

### المجتمع | Community
- **Laravel Arabic Community:** Facebook groups and forums
- **Stack Overflow:** https://stackoverflow.com/questions/tagged/laravel
- **Laravel.io Forum:** https://laravel.io/forum
- **Discord:** Laravel community server

### الأدوات المساعدة | Helpful Tools
- **Laravel Debugbar:** تصحيح الأخطاء | Debugging
- **Laravel IDE Helper:** دعم IDE | IDE support
- **Laravel Telescope:** مراقبة التطبيق | Application monitoring
- **Laravel Horizon:** إدارة الطوابير | Queue management

---

## مثال مشروع كامل | Complete Project Example

### نظام مدونة بسيط | Simple Blog System

```bash
# إنشاء المشروع | Create project
laravel new blog

# إنشاء النماذج والهجرات | Create models and migrations
php artisan make:model Post -mcr
php artisan make:model Comment -mc

# تشغيل الهجرات | Run migrations
php artisan migrate

# إنشاء المصادقة | Create authentication
php artisan breeze:install
npm install && npm run dev

# تشغيل السيرفر | Start server
php artisan serve
```

---

## الخاتمة | Conclusion

**العربية:**
Laravel هو إطار عمل قوي وسهل الاستخدام يساعدك على بناء تطبيقات ويب احترافية بسرعة وكفاءة. من خلال هذا الدليل، قدمنا لك الأساسيات والمفاهيم المهمة للبدء في تعلم Laravel. استمر في الممارسة وبناء المشاريع لتطوير مهاراتك!

**English:**
Laravel is a powerful and user-friendly framework that helps you build professional web applications quickly and efficiently. Through this guide, we've provided you with the fundamentals and important concepts to start learning Laravel. Keep practicing and building projects to develop your skills!

---

## نصائح نهائية | Final Tips

1. **ابدأ بمشروع صغير | Start with a small project**
   - نظام مدونة بسيط | Simple blog system
   - نظام إدارة المهام | Task management system

2. **مارس بانتظام | Practice regularly**
   - اقرأ الكود واكتب الكود يومياً | Read and write code daily

3. **انضم للمجتمع | Join the community**
   - شارك في المنتديات | Participate in forums
   - ساعد الآخرين | Help others

4. **اتبع أفضل الممارسات | Follow best practices**
   - اكتب كود نظيف | Write clean code
   - استخدم Git للتحكم بالنسخ | Use Git for version control

5. **تعلم باستمرار | Keep learning**
   - تابع التحديثات الجديدة | Follow new updates
   - تعلم تقنيات متقدمة | Learn advanced techniques

---

**بالتوفيق في رحلتك مع Laravel! 🚀**
**Good luck on your Laravel journey! 🚀**

**Made with ❤️ for Arabic & English Learners**
