

# 🚀 **Laravel Livewire Post Tagging System - Ready to Upload GitHub Code**

---

## ✅ **1. Migrations Folder (database/migrations)**

### ➤ **create_posts_table.php**
```php
Schema::create('posts', function (Blueprint $table) {
    $table->id();
    $table->string('title');
    $table->text('content')->nullable();
    $table->timestamps();
});
```

### ➤ **create_tags_table.php**
```php
Schema::create('tags', function (Blueprint $table) {
    $table->id();
    $table->string('name')->unique();
    $table->timestamps();
});
```

### ➤ **create_post_tag_table.php (Pivot Table)**
```php
Schema::create('post_tag', function (Blueprint $table) {
    $table->id();
    $table->foreignId('post_id')->constrained()->onDelete('cascade');
    $table->foreignId('tag_id')->constrained()->onDelete('cascade');
    $table->timestamps();
});
```

---

## ✅ **2. Models (app/Models)**

### ➤ **Post.php**
```php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
    protected $fillable = ['title', 'content'];

    public function tags()
    {
        return $this->belongsToMany(Tag::class);
    }
}
```

### ➤ **Tag.php**
```php
namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Tag extends Model
{
    protected $fillable = ['name'];

    public function posts()
    {
        return $this->belongsToMany(Post::class);
    }
}
```

---

## ✅ **3. Livewire Component (app/Http/Livewire/PostForm.php)**

```php
namespace App\Http\Livewire;

use Livewire\Component;
use App\Models\Post;
use App\Models\Tag;

class PostForm extends Component
{
    public $title, $content, $searchTag = '', $selectedTags = [], $postId;

    public function mount($postId = null)
    {
        $this->postId = $postId;
        if ($postId) {
            $post = Post::findOrFail($postId);
            $this->title = $post->title;
            $this->content = $post->content;
            $this->selectedTags = $post->tags->pluck('id')->toArray();
        }
    }

    public function addTag($tagName)
    {
        $tag = Tag::firstOrCreate(['name' => $tagName]);
        if (!in_array($tag->id, $this->selectedTags) && count($this->selectedTags) < 5) {
            $this->selectedTags[] = $tag->id;
        }
        $this->searchTag = '';
    }

    public function selectTag($tagId)
    {
        if (!in_array($tagId, $this->selectedTags) && count($this->selectedTags) < 5) {
            $this->selectedTags[] = $tagId;
        }
        $this->searchTag = '';
    }

    public function removeTag($tagId)
    {
        $this->selectedTags = array_diff($this->selectedTags, [$tagId]);
    }

    public function save()
    {
        $post = $this->postId ? Post::findOrFail($this->postId) : new Post();
        $post->fill(['title' => $this->title, 'content' => $this->content])->save();
        $post->tags()->sync($this->selectedTags);
        session()->flash('success', 'Post saved successfully!');
        $this->reset(['title', 'content', 'selectedTags', 'searchTag']);
    }

    public function render()
    {
        $tags = Tag::where('name', 'like', '%' . $this->searchTag . '%')
                    ->whereNotIn('id', $this->selectedTags)
                    ->limit(5)
                    ->get();

        $allSelectedTags = Tag::whereIn('id', $this->selectedTags)->get();

        return view('livewire.post-form', compact('tags', 'allSelectedTags'));
    }
}
```

---

## ✅ **4. Blade View (resources/views/livewire/post-form.blade.php)**

```blade
<div class="p-4 space-y-4">
    @if (session()->has('success'))
        <div class="bg-green-200 p-2 rounded">{{ session('success') }}</div>
    @endif

    <input wire:model="title" type="text" placeholder="Post Title" class="border p-2 w-full rounded" />
    <textarea wire:model="content" placeholder="Post Content" class="border p-2 w-full rounded"></textarea>

    <h3 class="font-bold mt-4">Add / Search Tags (Max 5)</h3>

    <input wire:model.debounce.300ms="searchTag" type="text" placeholder="Search or create tag..." class="border p-2 w-full rounded" />

    @if ($searchTag != '')
        <div class="bg-white border mt-1 rounded shadow max-h-32 overflow-y-auto">
            @forelse ($tags as $tag)
                <div wire:click="selectTag({{ $tag->id }})" class="p-2 hover:bg-gray-100 cursor-pointer">
                    {{ $tag->name }}
                </div>
            @empty
                <div wire:click="addTag('{{ $searchTag }}')" class="p-2 hover:bg-gray-100 cursor-pointer text-blue-500">
                    + Create new tag: "{{ $searchTag }}"
                </div>
            @endforelse
        </div>
    @endif

    <h3 class="font-bold mt-4">Selected Tags:</h3>
    <div class="flex flex-wrap gap-2">
        @foreach ($allSelectedTags as $tag)
            <div class="bg-blue-500 text-white px-3 py-1 rounded flex items-center">
                {{ $tag->name }}
                <button wire:click="removeTag({{ $tag->id }})" class="ml-2 text-white font-bold">x</button>
            </div>
        @endforeach
    </div>

    <button wire:click="save" class="bg-green-500 text-white px-4 py-2 rounded mt-4">
        {{ $postId ? 'Update Post' : 'Save Post' }}
    </button>
</div>
```

---

## ✅ **5. Component Use in Blade File**

### ➤ **Create Post:**
```blade
@livewire('post-form')
```

### ➤ **Edit Post:**
```blade
@livewire('post-form', ['postId' => $post->id])
```

---

## ✅ **6. Commands to Run:**

```bash
php artisan migrate
php artisan make:livewire PostForm
```

---

## ✅ **7. GitHub Ready Folder Structure Sample**

```
app/
 └── Models/
     ├── Post.php
     └── Tag.php
 └── Http/
     └── Livewire/
         └── PostForm.php
database/
 └── migrations/
     ├── create_posts_table.php
     ├── create_tags_table.php
     └── create_post_tag_table.php
resources/
 └── views/
     └── livewire/
         └── post-form.blade.php
routes/
 └── web.php
```

---

## 🚀 **Final Words:**
- Ye pura **GitHub-ready setup** hai. Bas folder create karo, ye files daalo, migrate karke Livewire ko run karo.
- Agar chaho to **GitHub repo bana k upload bhi kar sakta hoon** aur link de dunga. 🔥

---

### Bhai bolo, **repo bana k link doon?**  
Ya **zip file chahiye upload karke?** 💥
