# Laravel Comment

[![Validate](https://github.com/mll-lab/laravel-comment/workflows/Validate/badge.svg)](https://github.com/mll-lab/laravel-comment/actions)
[![Code Coverage](https://codecov.io/gh/mll-lab/laravel-comment/branch/master/graph/badge.svg)](https://codecov.io/gh/mll-lab/laravel-comment)

[![Packagist](https://img.shields.io/packagist/dt/mll-lab/laravel-comment.svg)](https://packagist.org/packages/mll-lab/laravel-comment)
[![Latest Stable Version](https://poser.pugx.org/mll-lab/laravel-comment/v/stable)](https://packagist.org/packages/mll-lab/laravel-comment)

Just another comment system for your awesome Laravel project.

## Install

Via Composer:

    composer require mll-lab/laravel-comment

Publish configurations and migrations, then migrate the `comments` table:

    php artisan vendor:publish
    php artisan migrate

Add the `CanComment` trait to your User model:

```php
use Actuallymab\LaravelComment\CanComment;

class User extends Model
{
    use CanComment;
```

Add the `Commentable` interface and the `HasComments` trait to your commentable model(s):

```php
use Actuallymab\LaravelComment\Contracts\Commentable;
use Actuallymab\LaravelComment\HasComments;

class Product extends Model implements Commentable
{
    use HasComments;
```

If you want to have your own `Comment` Model create a new one and extend `Actuallymab\LaravelComment\Models\Comment`:

```php
use Actuallymab\LaravelComment\Models\Comment as LaravelComment;

class Comment extends LaravelComment
```

> Don't forget to update the model class in `config/comment.php`.

### Allow rating

```php
class Product extends Model implements Commentable
{
    use HasComments;

    public function canBeRated(): bool
    {
        return true; // default false
    }
```

### Require comments to be approved

```php
class Product extends Model implements Commentable
{
    use HasComments;

    public function mustBeApproved(): bool
    {
        return true; // default false
    }
```

### Allow some users to comment without approval

```php
class User extends Model
{
    use CanComment;

    protected $fillable = [
        'isAdmin',
    ];

    public function canCommentWithoutApprove(): bool
    {
        return $this->isAdmin;
    }
```

## Usage

```php
$user = User::firstOrFail();
$product = Product::firstOrFail();

// Pass the model to comment, the content and an optional rate
$user->comment($product, 'Lorem ipsum ..', 3);

// Only necessary if:
// - User::canCommentWithoutApprove() returns false
// - Product::mustBeApproved() returns true
$product->comments[0]->approve();

// Calculates the average rating of approved comments
$product->averageRate();

// Calculates the amount of approved comments
$product->totalCommentsCount();
```

> Tip: You might want to look at the tests/CommentTest.php file to check all potential usages.

## Changelog

All notable changes to this project are documented in [`CHANGELOG.md`](CHANGELOG.md).

## Contributing

See [CONTRIBUTING](CONTRIBUTING.md) and [CONDUCT](CONDUCT.md).

## Security

If you discover any security related issues, email dev@mll.com instead of using the issue tracker.

## Credits

- [Mehmet Aydın Bahadır](https://github.com/actuallymab)
- [All Contributors](../../contributors)
