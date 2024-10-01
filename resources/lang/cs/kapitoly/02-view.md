# View

## 1. **Vytvoření View**
- Views se nachází v adresáři **`resources/views`**.
- Používá se šablonovací systém Blade a proto má příponu **`.blade.php`**.

## 2. **Vrácení View z kontroleru**
Použij metodu **`view()`** pro vrácení šablony v kontroleru:

```php
return view('welcome');
```
Vrací soubor **`resources/views/welcome.blade.php`**.

## 3. **Odeslání dat do View**
Data můžeš poslat do view jako druhý argument metody `view()`:

```php
return view('greeting', ['name' => 'John Doe']);
```

Pro zobrazení v šabloně

```php
<!-- greeting.blade.php -->
Hello, {{ $name }}!
```

## 4. **Blade syntaxe pro zobrazení proměnných**

```php
{{ $variable }}
```
Bezpečně zobrazuje hodnotu proměnné

Pro zobrazení bez úniku:

```php
{!! $htmlVariable !!}
```

## 5. **Blade direktivy**

### @if, @elseif, @else, @endif
```php
@if ($user->isAdmin())
    Welcome, Admin!
@elseif ($user->isEditor())
    Welcome, Editor!
@else
    Welcome, User!
@endif
```

### @isset, @empty
```php
@isset($variable)
    The variable is set.
@endisset

@empty($variable)
    The variable is empty.
@endempty
```

### @unless (inverze if)
```php
@unless ($user->isAdmin())
    You are not an admin.
@endunless
```

### @switch, @case, @default
```php
@switch($role)
    @case('admin')
        Welcome, Admin!
        @break

    @case('editor')
        Welcome, Editor!
        @break

    @default
        Welcome, User!
@endswitch
```

## 6. **Cykly v Blade**

### @for
```php
@for ($i = 0; $i < 10; $i++)
    This is iteration {{ $i }}
@endfor
```

### @foreach
```php
@foreach ($users as $user)
    <p>This is user {{ $user->name }}</p>
@endforeach
```

### @forelse
```php
@forelse ($users as $user)
    <p>This is user {{ $user->name }}</p>
@empty
    <p>No users found.</p>
@endforelse
```

### @while
```php
@while (true)
    <p>This loop will run forever.</p>
@endwhile
```

## 7. **Zahrnutí šablon**

### @include
Vloží jinou šablonu:
```php
@include('header')
```

Můžeš poslat data jako argumenty:
```php
@include('header', ['title' => 'Welcome'])
```

### @extends
Používá se pro rozšíření layoutu:

```php
@extends('layouts.master')
```

### @section, @yield
Používá se pro vymezení oblastí, které lze v layoutu přepisovat.

```php
<!-- layout.blade.php -->
<html>
<head>
    <title>@yield('title')</title>
</head>
<body>
    @yield('content')
</body>
</html>
```

```php
<!-- child.blade.php -->
@extends('layouts.master')

@section('title', 'Page Title')

@section('content')
    <p>This is the content of the page.</p>
@endsection
```

### @push, @stack
Používá se pro přidávání částí kódu do specifických sekcí.

```php
@push('scripts')
    <script src="/example.js"></script>
@endpush

<!-- V layoutu -->
@stack('scripts')
```

## 8. **Podmíněné vložení souborů**

### @includeWhen
```php
@includeWhen($boolean, 'view.name', ['some' => 'data'])
```

### @includeIf
Vloží soubor pouze, pokud existuje:
```php
@includeIf('view.name', ['some' => 'data'])
```

## 9. **Komentáře v Blade**

Komentář v Blade, který nebude viditelný v HTML výstupu:
```php
{{-- This is a Blade comment --}}
```

## 10. **Výchozí hodnota pro proměnnou**

Pokud proměnná není nastavena, můžeš použít výchozí hodnotu:

```php
{{ $name ?? 'Default Name' }}
```

## 11. **Cykly a manipulace s daty**

##### $loop proměnná
Když používáš **@foreach**, Laravel poskytuje proměnnou `$loop` s informacemi o cyklu.

```php
@foreach ($items as $item)
    @if ($loop->first)
        This is the first iteration.
    @endif

    @if ($loop->last)
        This is the last iteration.
    @endif
@endforeach
```

## 12. **Asset Management**
Pro správu CSS a JavaScriptových souborů:

```php
<link href="{{ asset('css/app.css') }}" rel="stylesheet">
<script src="{{ asset('js/app.js') }}"></script>
```

## 13. **Vytváření komponent**

Komponenty Blade umožňují vytvářet znovu použitelné části stránky.

### Vytvoření komponenty
Vytvoříš komponentu souborem v adresáři **resources/views/components**:
```php
<!-- resources/views/components/alert.blade.php -->
<div class="alert alert-{{ $type }}">
    {{ $slot }}
</div>
```

### Použití komponenty
```php
<x-alert type="warning">
    This is a warning alert.
</x-alert>
```