
### **User-wise interaction on single post** ✅  

<!-- ### Toh chalo bhai isko samjh lete hain — "User Post Interaction System"   -->

---

## ✅ **System Design — Har User Apni Taraf se Mark kare**

| Post | User 1 Action      | User 2 Action       |
|------|-------------------|--------------------|
| Post 1 | Bookmarked + Starred | Important            |
| Post 2 | Important          | Starred + Bookmarked |

### ✅ **Solution Idea:**
Ek alag table banayenge jo har post + user ka record rakhe:
- user_id
- post_id
- is_bookmarked (boolean)
- is_starred (boolean)
- is_important (boolean)

---

## ✅ **1. Migration for User_Post_Interaction Table**

```php
Schema::create('post_user_actions', function (Blueprint $table) {
    $table->id();
    $table->foreignId('user_id')->constrained()->onDelete('cascade');
    $table->foreignId('post_id')->constrained()->onDelete('cascade');
    $table->boolean('is_bookmarked')->default(false);
    $table->boolean('is_starred')->default(false);
    $table->boolean('is_important')->default(false);
    $table->timestamps();

    $table->unique(['user_id', 'post_id']); // unique combination to avoid duplicates
});
```

---

## ✅ **2. Relation in Models**

### **User.php**
```php
public function postActions()
{
    return $this->hasMany(PostUserAction::class);
}
```

### **Post.php**
```php
public function userActions()
{
    return $this->hasMany(PostUserAction::class);
}
```

### **PostUserAction.php (New Model)**
```php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class PostUserAction extends Model
{
    protected $fillable = ['user_id', 'post_id', 'is_bookmarked', 'is_starred', 'is_important'];

    public function user()
    {
        return $this->belongsTo(User::class);
    }

    public function post()
    {
        return $this->belongsTo(Post::class);
    }
}
```

---

## ✅ **3. Livewire Component (PostActionComponent.php)**

```php
namespace App\Http\Livewire;

use Livewire\Component;
use App\Models\PostUserAction;
use Illuminate\Support\Facades\Auth;

class PostActionComponent extends Component
{
    public $postId;
    public $isBookmarked = false, $isStarred = false, $isImportant = false;

    public function mount($postId)
    {
        $this->postId = $postId;
        $action = PostUserAction::where('user_id', Auth::id())->where('post_id', $postId)->first();

        if ($action) {
            $this->isBookmarked = $action->is_bookmarked;
            $this->isStarred = $action->is_starred;
            $this->isImportant = $action->is_important;
        }
    }

    public function toggleBookmark()
    {
        $this->isBookmarked = !$this->isBookmarked;
        $this->saveAction();
    }

    public function toggleStar()
    {
        $this->isStarred = !$this->isStarred;
        $this->saveAction();
    }

    public function toggleImportant()
    {
        $this->isImportant = !$this->isImportant;
        $this->saveAction();
    }

    private function saveAction()
    {
        PostUserAction::updateOrCreate(
            ['user_id' => Auth::id(), 'post_id' => $this->postId],
            [
                'is_bookmarked' => $this->isBookmarked,
                'is_starred' => $this->isStarred,
                'is_important' => $this->isImportant,
            ]
        );
    }

    public function render()
    {
        return view('livewire.post-action-component');
    }
}
```

---

## ✅ **4. Blade File (post-action-component.blade.php)**

```blade
<div class="flex gap-4 items-center mt-4">
    <button wire:click="toggleBookmark" class="px-3 py-1 rounded 
        {{ $isBookmarked ? 'bg-yellow-500 text-white' : 'bg-gray-300' }}">
        {{ $isBookmarked ? 'Bookmarked' : 'Bookmark' }}
    </button>

    <button wire:click="toggleStar" class="px-3 py-1 rounded 
        {{ $isStarred ? 'bg-blue-500 text-white' : 'bg-gray-300' }}">
        {{ $isStarred ? 'Starred' : 'Star' }}
    </button>

    <button wire:click="toggleImportant" class="px-3 py-1 rounded 
        {{ $isImportant ? 'bg-red-500 text-white' : 'bg-gray-300' }}">
        {{ $isImportant ? 'Important' : 'Mark Important' }}
    </button>
</div>
```

---

## ✅ **5. Route & Usage Example**

### Route:
```php
Route::get('/posts/{post}', function (App\Models\Post $post) {
    return view('posts.show', compact('post'));
})->middleware('auth');
```

### View (posts/show.blade.php):
```blade
<h1 class="text-2xl font-bold">{{ $post->title }}</h1>
<p>{{ $post->content }}</p>

@livewire('post-action-component', ['postId' => $post->id])
```

---

## ✅ **6. Migrate Command**

```bash
php artisan make:model PostUserAction -m
php artisan migrate
```

---

## ✅ **7. Future Improvements Ideas**

| Feature Idea                     | Description                                     |
|----------------------------------|------------------------------------------------|
| **Count of total Bookmarks/Stars**| Har post pe total counts dikhaye.               |
| **User-specific Filter**          | Profile me dekho user ne kya kya mark kiya.    |
| **Icons instead of buttons**     | Beautiful icons (like star, bookmark).         |
| **Notification to Post Owner**  | Jab koi important kare, owner ko notify karo. |
| **Sorting by Importance**        | Important marked posts ko upar dikhaye.        |
| **Undo Options**                 | Actions ko undo ka button.                    |

---

## ✅ **Final Structure:**
```
app/
 └── Models/
     └── PostUserAction.php
 └── Http/
     └── Livewire/
         └── PostActionComponent.php
database/
 └── migrations/
     └── create_post_user_actions_table.php
resources/
 └── views/
     └── livewire/
         └── post-action-component.blade.php
     └── posts/
         └── show.blade.php
routes/
 └── web.php
```

---

## ⚙️ **Summary in simple words:**
- Ek post ko har user apne hisaab se alag alag Bookmark, Star, Important mark karega.
- Alag se pivot table handle karegi sab kuch.
- UI me buttons dikhayein jo user ka action reflect karein.