# Laravel Blade Templates with Eloquent ORM (Lab 9)

## Overview
This lab extends the previous Blade Templates practice by integrating **Eloquent ORM** for real database interactions. Instead of using mocked data, we now fetch and manage data from a **MySQL database** using Laravel's ORM system. The lab covers **layouts, components, models, migrations, and CRUD operations** while ensuring clean, maintainable code.

## Learning Objectives
By completing this lab, students will:
- Understand the role of **Blade templates** in Laravel’s **MVC framework**.
- Utilize **Eloquent ORM** to interact with the database.
- Implement **CRUD operations** using Eloquent models.
- Develop **reusable Blade components**.
- Apply Laravel’s **best practices** for maintainable code.

## Project Setup & Requirements
### 1️⃣ Setup Laravel Project
#### Install Laravel using Composer:
```sh
composer create-project --prefer-dist laravel/laravel BladeLab
```
#### Navigate to the Project Directory:
```sh
cd BladeLab
```
#### Configure Environment Variables:
Edit the `.env` file and set up your **database connection**:
```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=
```
#### Run Laravel Development Server:
```sh
php artisan serve
```

## Eloquent ORM Setup
### 2️⃣ Create Database Table
#### **Generate Migration for `products` Table**
```sh
php artisan make:migration create_products_table --create=products
```
#### **Define Schema in Migration File**
```php
Schema::create('products', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->text('description')->nullable();
    $table->decimal('price', 8, 2);
    $table->integer('stock')->default(0);
    $table->timestamps();
});
```
#### **Run Migration**
```sh
php artisan migrate
```

### 3️⃣ Create Product Model
```sh
php artisan make:model Product
```
Modify `app/Models/Product.php`:
```php
class Product extends Model {
    protected $fillable = ['name', 'description', 'price', 'stock'];
}
```

### 4️⃣ Seed Database with Sample Data
```sh
php artisan make:seeder ProductSeeder
```
Modify `database/seeders/ProductSeeder.php`:
```php
DB::table('products')->insert([
    ['name' => 'Product A', 'description' => 'First product.', 'price' => 100, 'stock' => 10],
    ['name' => 'Product B', 'description' => 'Second product.', 'price' => 150, 'stock' => 20],
]);
```
Run the seeder:
```sh
php artisan db:seed --class=ProductSeeder
```

## Update Application to Use Eloquent ORM
### 5️⃣ Modify Controller to Fetch Data from Database
Modify `app/Http/Controllers/PageController.php`:
```php
use App\Models\Product;

class PageController extends Controller {
    public function home() {
        $products = Product::all();
        return view('home', compact('products'));
    }
}
```

### 6️⃣ Update Blade View to Display Real Data
Modify `resources/views/home.blade.php`:
```blade
@forelse($products as $product)
    <p>{{ $product->name }} - ${{ $product->price }}</p>
@empty
    <x-alert type="danger" message="No Products Found."/>
@endforelse
```

## **Testing & Validation**
- Start the server: `php artisan serve`
- Visit `/` and `/about` in the browser.
- Ensure data is displayed correctly from the database.
- Verify Blade components are rendering.

## **Screenshots**
**1.Output and data inserted**

<img width="1512" alt="Screenshot 2025-03-18 at 1 30 01 PM" src="https://github.com/user-attachments/assets/739e44a7-c524-47b7-a1f4-52a4ca4e5b2d" />

<img width="1512" alt="Screenshot 2025-03-18 at 1 30 37 PM" src="https://github.com/user-attachments/assets/3b98e1b0-12d6-4eac-9209-d6cce177641f" />

**2.About**
<img width="1512" alt="Screenshot 2025-03-18 at 1 31 44 PM" src="https://github.com/user-attachments/assets/cb2ebf65-a440-41dc-9e6f-3bff62d3f4d4" />


## **Submission Guidelines**
- **Push your project to GitHub**.
- Include this `README.md` file in the repository.
- Submit the **GitHub repository link** via the course portal.

## **Evaluation Criteria**
| Criteria | Marks |
|----------|-------|
| Proper Laravel project setup and configuration | 10 |
| Accurate creation of Blade layouts and views | 15 |
| Effective usage of Blade directives | 15 |
| Creation and integration of reusable components | 15 |
| Proper integration with Eloquent ORM | 20 |
| Rewriting application to utilize Eloquent ORM | 15 |
| Code quality, structure, and best practices | 10 |
| Documentation and adherence to submission guidelines | 10 |
| **Total Marks** | **100** |

---
## **Additional Notes**
- Follow **Laravel's best practices** for file structure and naming.
- Ensure **clean and readable** Blade templates.
- Use **comments** in Blade files to explain complex logic.
- **Maintain academic integrity** (plagiarism will result in disqualification).

