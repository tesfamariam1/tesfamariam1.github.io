---
title: Laravel Role and Permission
date: 2023-01-31 +03:00
categories: [Laravel, Role, Permission]
tags: [laravel,role,permission,spatie]
---
# Installation in Laravel

This package can be used with Laravel 6.0 or higher.

PACKAGE |	LARAVEL VERSION
-------- | ------------
^5.8 |	7,8,9,10
^5.7 |	7,8,9
^5.4-^5.6 | 	7,8
5.0-5.3 | 	6,7,8
^4	| 6,7,8
^3	| 5.8

# Installing
1. Consult the Prerequisites page for important considerations regarding your User models!
2. This package publishes a config/permission.php file. If you already have a file by that name, you must rename or remove it.
3. You can install the package via composer:
```php
composer require spatie/laravel-permission
```
4. Optional: The service provider will automatically get registered. Or you may manually add the service provider in your config/app.php file:
```php
'providers' => [
    // ...
    Spatie\Permission\PermissionServiceProvider::class,
];
```
5. You should publish the migration and the config/permission.php config file with:
```php
php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider"
```
6. NOTE: If you are using UUIDs, see the Advanced section of the docs on UUID steps, before you continue. It explains some changes you may want to make to the migrations and config file before continuing. It also mentions important considerations after extending this package's models for UUID capability. If you are going to use teams feature, you have to update your config/permission.php config file and set 'teams' => true,, if you want to use a custom foreign key for teams you must change team_foreign_key.

7. Clear your config cache. This package requires access to the permission config. Generally it's bad practice to do config-caching in a development environment. If you've been caching configurations locally, clear your config cache with either of these commands:
```php
 php artisan optimize:clear
 # or
 php artisan config:clear
 ```
 8. Run the migrations: After the config and migration have been published and configured, you can create the tables for this package by running:
 ```php
 php artisan migrate
 ```
 9. Add the necessary trait to your User model:
 ```php
 // The User model requires this trait
 use HasRoles;
 ```
 Consult the Basic Usage section of the docs to get started using the features of this package.

 # Basic Usage
 First, add the Spatie\Permission\Traits\HasRoles trait to your User model(s):

```php
use Illuminate\Foundation\Auth\User as Authenticatable;
use Spatie\Permission\Traits\HasRoles;

class User extends Authenticatable
{
    use HasRoles;

    // ...
}
```
This package allows for users to be associated with permissions and roles. Every role is associated with multiple permissions. A Role and a Permission are regular Eloquent models. They require a name and can be created like this:
```php
use Spatie\Permission\Models\Role;
use Spatie\Permission\Models\Permission;

$role = Role::create(['name' => 'writer']);
$permission = Permission::create(['name' => 'edit articles']);
```
A permission can be assigned to a role using 1 of these methods:
```php
$role->givePermissionTo($permission);
$permission->assignRole($role);
```
Multiple permissions can be synced to a role using 1 of these methods:
```php
$role->syncPermissions($permissions);
$permission->syncRoles($roles);
```
A permission can be removed from a role using 1 of these methods:
```php
$role->revokePermissionTo($permission);
$permission->removeRole($role);
```
If you're using multiple guards the guard_name attribute needs to be set as well. Read about it in the using multiple guards section of the readme.

The HasRoles trait adds Eloquent relationships to your models, which can be accessed directly or used as a base query:
```php
// get a list of all permissions directly assigned to the user
$permissionNames = $user->getPermissionNames(); // collection of name strings
$permissions = $user->permissions; // collection of permission objects

// get all permissions for the user, either directly, or from roles, or from both
$permissions = $user->getDirectPermissions();
$permissions = $user->getPermissionsViaRoles();
$permissions = $user->getAllPermissions();

// get the names of the user's roles
$roles = $user->getRoleNames(); // Returns a collection
```
The HasRoles trait also adds a role scope to your models to scope the query to certain roles or permissions:
```php
$users = User::role('writer')->get(); // Returns only users with the role 'writer'
```
The role scope can accept a string, a \Spatie\Permission\Models\Role object or an \Illuminate\Support\Collection object.

The same trait also adds a scope to only get users that have a certain permission.
```php
$users = User::permission('edit articles')->get(); // Returns only users with the permission 'edit articles' (inherited or directly)
```
The scope can accept a string, a \Spatie\Permission\Models\Permission object or an \Illuminate\Support\Collection object.
# ELOQUENT
Since Role and Permission models are extended from Eloquent models, basic Eloquent calls can be used as well:
```php
$all_users_with_all_their_roles = User::with('roles')->get();
$all_users_with_all_direct_permissions = User::with('permissions')->get();
$all_roles_in_database = Role::all()->pluck('name');
$users_without_any_roles = User::doesntHave('roles')->get();
$all_roles_except_a_and_b = Role::whereNotIn('name', ['role A', 'role B'])->get();
```
# Direct Permissions
A permission can be given to any user:
```php
$user->givePermissionTo('edit articles');

// You can also give multiple permission at once
$user->givePermissionTo('edit articles', 'delete articles');

// You may also pass an array
$user->givePermissionTo(['edit articles', 'delete articles']);
```
A permission can be revoked from a user:
```php
$user->revokePermissionTo('edit articles');
```
Or revoke & add new permissions in one go:
```php
$user->syncPermissions(['edit articles', 'delete articles']);
```
You can check if a user has a permission:
```php
$user->hasPermissionTo('edit articles');
```
Or you may pass an integer representing the permission id
```php
$user->hasPermissionTo('1');
$user->hasPermissionTo(Permission::find(1)->id);
$user->hasPermissionTo($somePermission->id);
```
You can check if a user has Any of an array of permissions:
```php
$user->hasAnyPermission(['edit articles', 'publish articles', 'unpublish articles']);
```
...or if a user has All of an array of permissions:
```php
$user->hasAllPermissions(['edit articles', 'publish articles', 'unpublish articles']);
```
You may also pass integers to lookup by permission id
```php
$user->hasAnyPermission(['edit articles', 1, 5]);
```
Saved permissions will be registered with the Illuminate\Auth\Access\Gate class for the default guard. So you can check if a user has a permission with Laravel's default can function:
```php
$user->can('edit articles');
```
# Using Permissions via Roles
## Assigning Roles
A role can be assigned to any user:
```php
$user->assignRole('writer');

// You can also assign multiple roles at once
$user->assignRole('writer', 'admin');
// or as an array
$user->assignRole(['writer', 'admin']);
```
A role can be removed from a user:
```php
$user->removeRole('writer');
```
Roles can also be synced:
```php
// All current roles will be removed from the user and replaced by the array given
$user->syncRoles(['writer', 'admin']);
```
## Checking Roles
You can determine if a user has a certain role:
```php
$user->hasRole('writer');

// or at least one role from an array of roles:
$user->hasRole(['editor', 'moderator']);
```
You can also determine if a user has any of a given list of roles:
```php
$user->hasAnyRole(['writer', 'reader']);
// or
$user->hasAnyRole('writer', 'reader');
```
You can also determine if a user has all of a given list of roles:
```php
$user->hasAllRoles(Role::all());
```
You can also determine if a user has exactly all of a given list of roles:
```php
$user->hasExactRoles(Role::all());
```
The assignRole, hasRole, hasAnyRole, hasAllRoles, hasExactRoles and removeRole functions can accept a string, a \Spatie\Permission\Models\Role object or an \Illuminate\Support\Collection object.
## Assigning Permissions to Roles
A permission can be given to a role:
```php
$role->givePermissionTo('edit articles');
```
You can determine if a role has a certain permission:
```php
$role->hasPermissionTo('edit articles');
```
A permission can be revoked from a role:
```php
$role->revokePermissionTo('edit articles');
```
Or revoke & add new permissions in one go:
```php
$role->syncPermissions(['edit articles', 'delete articles']);
```
The givePermissionTo and revokePermissionTo functions can accept a string or a Spatie\Permission\Models\Permission object.

### NOTE: Permissions are inherited from roles automatically.

## WHAT PERMISSIONS DOES A ROLE HAVE?
The permissions property on any given role returns a collection with all the related permission objects. This collection can respond to usual Eloquent Collection operations, such as count, sort, etc.

```php
// get collection
$role->permissions;

// return only the permission names:
$role->permissions->pluck('name');

// count the number of permissions assigned to a role
count($role->permissions);
// or
$role->permissions->count();
```
## Assigning Direct Permissions To A User
Additionally, individual permissions can be assigned to the user too. For instance:
```php
$role = Role::findByName('writer');
$role->givePermissionTo('edit articles');

$user->assignRole('writer');

$user->givePermissionTo('delete articles');
```
In the above example, a role is given permission to edit articles and this role is assigned to a user. Now the user can edit articles and additionally delete articles. The permission of 'delete articles' is the user's direct permission because it is assigned directly to them. When we call $user->hasDirectPermission('delete articles') it returns true, but false for $user->hasDirectPermission('edit articles').

This method is useful if one builds a form for setting permissions for roles and users in an application and wants to restrict or change inherited permissions of roles of the user, i.e. allowing to change only direct permissions of the user.

You can check if the user has a Specific or All or Any of a set of permissions directly assigned:
```php
// Check if the user has Direct permission
$user->hasDirectPermission('edit articles')

// Check if the user has All direct permissions
$user->hasAllDirectPermissions(['edit articles', 'delete articles']);

// Check if the user has Any permission directly
$user->hasAnyDirectPermission(['create articles', 'delete articles']);
```
By following the previous example, when we call $user->hasAllDirectPermissions(['edit articles', 'delete articles']) it returns false, because the user does not have edit articles as a direct permission. When we call $user->hasAnyDirectPermission('edit articles'), it returns true because the user has one of the provided permissions.

You can examine all of these permissions:
```php
// Direct permissions
$user->getDirectPermissions() // Or $user->permissions;

// Permissions inherited from the user's roles
$user->getPermissionsViaRoles();

// All permissions which apply on the user (inherited and direct)
$user->getAllPermissions();
```
All these responses are collections of Spatie\Permission\Models\Permission objects.

If we follow the previous example, the first response will be a collection with the delete article permission and the second will be a collection with the edit article permission and the third will contain both.

### NOTE ABOUT USING PERMISSION NAMES IN POLICIES
When calling authorize() for a policy method, if you have a permission named the same as one of those policy methods, your permission "name" will take precedence and not fire the policy. For this reason it may be wise to avoid naming your permissions the same as the methods in your policy. While you can define your own method names, you can read more about the defaults Laravel offers in Laravel's documentation at Writing Policies.

# Roles vs Permissions
It is generally best to code your app around testing against permissions only. (ie: when testing whether to grant access to something, in most cases it's wisest to check against a permission, not a role). That way you can always use the native Laravel @can and can() directives everywhere in your app.

Roles can still be used to group permissions for easy assignment to a user/model, and you can still use the role-based helper methods if truly necessary. But most app-related logic can usually be best controlled using the can methods, which allows Laravel's Gate layer to do all the heavy lifting. Sometimes certain groups of route rules may make best sense to group them around a role, but still, whenever possible, there is less overhead used if you can check against a specific permission instead.

eg: users have roles, and roles have permissions, and your app always checks for permissions, not roles.
