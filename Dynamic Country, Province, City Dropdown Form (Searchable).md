

# ✅ Dynamic Country, Province, City Dropdown Form (Searchable)

## 💥 Final Feature List:
1. **Country -> Province -> City** relation.
2. **Searchable dropdowns** using native solutions (without Select2).
3. **Livewire powered dynamic update**.
4. Form submission ke sath data store ho.

---

## ✅ 1. Database Tables Structure

### 1. Countries Table
```php
Schema::create('countries', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->timestamps();
});
```

### 2. Provinces Table
```php
Schema::create('provinces', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->foreignId('country_id')->constrained()->onDelete('cascade');
    $table->timestamps();
});
```

### 3. Cities Table
```php
Schema::create('cities', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->foreignId('province_id')->constrained()->onDelete('cascade');
    $table->timestamps();
});
```

---

## ✅ 2. Models Relations

### Country.php
```php
public function provinces()
{
    return $this->hasMany(Province::class);
}
```

### Province.php
```php
public function country()
{
    return $this->belongsTo(Country::class);
}

public function cities()
{
    return $this->hasMany(City::class);
}
```

### City.php
```php
public function province()
{
    return $this->belongsTo(Province::class);
}
```

---

## ✅ 3. Livewire Component (LocationForm.php)

```bash
php artisan make:livewire LocationForm
```

### LocationForm.php

```php
namespace App\Http\Livewire;

use Livewire\Component;
use App\Models\Country;
use App\Models\Province;
use App\Models\City;

class LocationForm extends Component
{
    public $countries, $provinces = [], $cities = [];
    public $selectedCountry = null, $selectedProvince = null, $selectedCity = null;

    public function mount()
    {
        $this->countries = Country::orderBy('name')->get();
    }

    public function updatedSelectedCountry($country)
    {
        $this->provinces = Province::where('country_id', $country)->orderBy('name')->get();
        $this->cities = []; // reset cities
        $this->selectedProvince = null;
        $this->selectedCity = null;
    }

    public function updatedSelectedProvince($province)
    {
        $this->cities = City::where('province_id', $province)->orderBy('name')->get();
        $this->selectedCity = null;
    }

    public function save()
    {
        // Validation
        $this->validate([
            'selectedCountry' => 'required|exists:countries,id',
            'selectedProvince' => 'required|exists:provinces,id',
            'selectedCity' => 'required|exists:cities,id',
        ]);

        // Here you can save the data to user profile or anywhere
        // Example:
        // auth()->user()->update([
        //     'country_id' => $this->selectedCountry,
        //     'province_id' => $this->selectedProvince,
        //     'city_id' => $this->selectedCity,
        // ]);

        session()->flash('message', 'Location saved successfully!');
    }

    public function render()
    {
        return view('livewire.location-form');
    }
}
```

---

## ✅ 4. Blade File (resources/views/livewire/location-form.blade.php)

```blade
<div class="p-4 bg-white shadow rounded space-y-4">
    @if (session()->has('message'))
        <div class="bg-green-200 text-green-800 p-2 rounded">{{ session('message') }}</div>
    @endif

    <div>
        <label class="block mb-1">Country</label>
        <select wire:model="selectedCountry" class="w-full border rounded p-2">
            <option value="">Select Country</option>
            @foreach($countries as $country)
                <option value="{{ $country->id }}">{{ $country->name }}</option>
            @endforeach
        </select>
    </div>

    @if (!empty($provinces))
    <div>
        <label class="block mb-1">Province</label>
        <select wire:model="selectedProvince" class="w-full border rounded p-2">
            <option value="">Select Province</option>
            @foreach($provinces as $province)
                <option value="{{ $province->id }}">{{ $province->name }}</option>
            @endforeach
        </select>
    </div>
    @endif

    @if (!empty($cities))
    <div>
        <label class="block mb-1">City</label>
        <select wire:model="selectedCity" class="w-full border rounded p-2">
            <option value="">Select City</option>
            @foreach($cities as $city)
                <option value="{{ $city->id }}">{{ $city->name }}</option>
            @endforeach
        </select>
    </div>
    @endif

    <button wire:click="save" class="bg-blue-500 text-white px-4 py-2 rounded">Save</button>
</div>
```

---

## ✅ 5. Route

```php
Route::get('/location-form', function () {
    return view('location-form');
})->middleware('auth');
```

### **location-form.blade.php**
```blade
@extends('layouts.app')

@section('content')
    <h1 class="text-2xl font-bold mb-4">Select Location</h1>
    @livewire('location-form')
@endsection
```

---

## ✅ 6. Commands for Models and Migrations

```bash
php artisan make:model Country -m
php artisan make:model Province -m
php artisan make:model City -m
php artisan migrate
```

---

## ✅ 7. Future Improvements Ideas

| Feature Idea                        | Description                                    |
|-------------------------------------|------------------------------------------------|
| **Searchable Dropdown (Tom Select)** | Live search in dropdown, light alternative to Select2. |
| **Auto Fill User Profile**           | Agar pehle se data hai, pre-fill.              |
| **Dependent Loading Animation**     | Loading spinner jab dusra dropdown load ho.   |
| **AJAX / Lazy Loading**              | Large country/city list ko AJAX se laana.     |
| **Add City/Province Option**        | Agar user city nahi mile to add option.       |

---

## ✅ 8. Plugins Suggestion (Optional Searchable Dropdown)

Agar dropdown ko searchable banana hai without jQuery:
- [Tom Select](https://tom-select.js.org/)
- [Choices.js](https://joshuajohnson.co.uk/Choices/)
- Pure Alpine.js based search dropdown (I can give that too)

---

## 🚀 **Conclusion:**
- **Dynamic filtering** ✅  
- **Searchable ready** ✅  
- **Pure Livewire & Tailwind** ✅  
- Future ready expandable 🔥  

---


# ✅ **Sources for Free Country, State, City Data**

### 1. **GitHub Repositories (Free CSV/SQL Files)**

| Source                         | Data Included                    | Format     |
|--------------------------------|----------------------------------|------------|
| **dr5hn/countries-states-cities-database** | Countries, States (Provinces), Cities | JSON/SQL/CSV |
| **samayo/country-state-city**  | Countries, States, Cities        | SQL/JSON   |

🔗 **Link**:  
- [dr5hn/countries-states-cities-database](https://github.com/dr5hn/countries-states-cities-database)
- [samayo/country-state-city](https://github.com/samayo/country-state-city)

### Example from `dr5hn`:
- ✅ **countries.json**
- ✅ **states.json**
- ✅ **cities.json**

You can **import this data directly in your database** using seeder ya direct SQL file!

---

### 2. **API Based Solutions (Live Data via API)**

| API Name                              | Free?       | Description                                   |
|---------------------------------------|-------------|----------------------------------------------|
| **Countries Now API**                  | ✅ Free     | Countries, States, Cities via API            |
| **GeoDB Cities API (RapidAPI)**        | Limited Free| Cities, Countries API                        |
| **RestCountries API**                  | ✅ Free     | Countries only                               |

**Countries Now API** Link: https://countriesnow.space/

> Example API:  
```bash
GET https://countriesnow.space/api/v0.1/countries/states
```

But **API ka limitation hota hai**:
- Har time API call -> slow system
- API down toh system down!

⚠️ So, **best option = local database copy**!

---

## ✅ **Step 1: Download Dataset (Best Recommended)**

**Recommended Repo**:  
➡️ [https://github.com/dr5hn/countries-states-cities-database](https://github.com/dr5hn/countries-states-cities-database)

**Steps**:
1. Repo open karo.
2. **Download ZIP** ya clone karo:
```bash
git clone https://github.com/dr5hn/countries-states-cities-database.git
```
3. Usme **SQL file milegi** (`countries+states+cities.sql`).
4. **Import SQL file in phpMyAdmin or MySQL Workbench**.

---

## ✅ **Step 2: Import in Your Database**

Agar terminal se karna chaho:

```bash
mysql -u root -p your_database_name < countries+states+cities.sql
```

Bas! Tumhare database me **Countries, States, Cities ka real data** ready!

---

## ✅ **Step 3: Connect to Your Laravel Models**

### Countries Model:
```php
class Country extends Model
{
    public function provinces()
    {
        return $this->hasMany(Province::class, 'country_id');
    }
}
```

### Provinces Model:
```php
class Province extends Model
{
    public function country()
    {
        return $this->belongsTo(Country::class, 'country_id');
    }

    public function cities()
    {
        return $this->hasMany(City::class, 'state_id'); // check if key is 'state_id'
    }
}
```

### Cities Model:
```php
class City extends Model
{
    public function province()
    {
        return $this->belongsTo(Province::class, 'state_id'); // check key name
    }
}
```

---

## ✅ **Step 4: Now use in Livewire**

Jesa pehle bata diya, Livewire me fetch karo aur show karo.

---

## ✅ **Quick Summary in Roman Urdu:**

1. ✅ **GitHub se real data download karo** (best option).
2. ✅ SQL file ko apne database me import karo.
3. ✅ Laravel models ke relations set karo.
4. ✅ Livewire component me dynamic dropdown bana do.

---

## 🚀 **Bonus Tip:**
- Agar tum chaho to **sirf Pakistan ka data bhi nikal sakte ho** usi dataset me filter karke!
- Warna **global system banega to poora world ka data ready hoga.**

---