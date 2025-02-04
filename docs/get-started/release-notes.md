# Release Notes

[[toc]]

## PowerGrid Version 4

### Dependencies

PowerGrid was born with the intention of always keeping as close as possible to the latest laravel update, so we updated the minimum versions of php, tailwind and livewire for greater support and durability.

[Read more](upgrade-guide?id=dependency-upgrades)

---

### Independent Export Engine

[Openspout](https://github.com/openspout/openspout) was previously installed as a dependency, now you must manually install it in `composer.json` and adjust which version you are using in PowerGrid settings.

[Read more](../get-started/upgrade-guide.html#independent-export-engine)

---

### Deprecated Batch Export properties

For better organization, we have moved the queues properties inside the Exportable Facade.

[Read more](../get-started/upgrade-guide.html#deprecated-queue-properties)

---

### Support TomSelect and SlimSelect

Add support for [Slim Select](https://slimselectjs.com/) and [Tom Select](https://tom-select.js.org/) by default, instead of using the multi select component that came by default in version 3. 
) and Tom Select by default instead of using the multi select component that came by default in version 3. 
This allows for further customization and greater support.

[Read more](../table/column-filters.html#multiselect)

---

### Changed `Columns::makeFilters` to Filter Facade

Instead of calling the method to create a custom filter inside a column, we should use the Filters Facade.

```php
<!-- 🚫 Before -->
Column::makeInputSelect() 
Column::makeInputMultiSelect()
Column::makeInputDatePicker()
Column::makeInputEnumSelect()
Column::makeInputRange()
Column::makeInputText()
Column::makeBooleanFilter()

<!-- ✅ After -->
Filter::multiSelect()
Filter::datepicker()
Filter::select()
Filter::number()
Filter::inputText()
Filter::boolean()
```

[Read more](../get-started/upgrade-guide.html#filters)

---

### Multi Sorting

PowerGrid v4 allows you to choose multiple columns to sort by.

To enable multi-sorting, you must set the property `$multiSort` to `true` in your PowerGrid table class.

```php
use PowerComponents\LivewirePowerGrid\Traits\WithExport;

final class YourPowerGridTable extends PowerGridComponent
{
     public bool $multiSort = true;
}
```

Multi-sorting behaves like chaining several `->orderBy(...)->orderBy(...)` [Laravel Eloquent](https://laravel.com/docs/9.x/eloquent) methods.

---

### Dynamic Filter

PowerGrid Filters are internal components, if you want to use an external component you can utilize Dynamic Filters.

A practical example is when you are using external components (such as [wireui](https://livewire-wireui.com/)) throughout your system and want to apply them in PowerGrid too.

Example:

```php
public function filters(): array
{
    return [
        Filter::dynamic('in_stock', 'in_stock')
            ->component('select') // <x-select ...attributes/>
            ->attributes([
                'class'           => 'min-w-[170px]',
                'async-data'      => route('categories.index'),
                'option-label'    => 'name',
                'multiselect'     => false,
                'option-value'    => 'id',
                'placeholder'     => 'Test',
                'wire:model.lazy' => 'filters.select.in_stock'
            ]),
    ];
}
```

![Output](/_media/examples/dynamic-select.png)

[Read more](../table/column-filters.html#filter-dynamic)

---

### Row Index

Sometimes we need to display the row index instead of the Model `id`.

To activate this functionality, simply chain the `index()` method to your column.

Example:

```php{5}
public function columns(): array
{
    return [
        Column::make('Index', '')
           ->index(),
}
```

![Output](/_media/examples/row-index.png)

---

### Tailwind Theme class

PowerGrid uses the slate color by default, you might want to change that, just insert the PowerGrid preset in the `tailwind.config.js` file

[Read more](../get-started/upgrade-guide.html#include-powergrid-presets-in-your-tailwind-config-js)

---

### Header Without Loading

If you don't want to display PowerGrid's default **loading** icon when some request is made to the server, just
call `withoutLoading()` on Header Facade. This is useful when you already have a layout to show the progress of internal calls. 

![Output](/_media/examples/without-loading.png)

---

### Custom SearchBox Theme

You can change the classes and styles of the input, icon search and icon close.

---

### Table tdBodyEmpty

You can use tdBodyEmpty to change the row style when the table is empty.

[Read more](../get-started/upgrade-guide.html#table-tdbodyempty)

---

### Filter Multi Select Async

If you don't want to load the multi-select data immediately when the table loads, you can use this feature to make your table appear faster.

PowerGrid uses TomSelect, you can read more about it [here](../get-started/release-notes.html#support-tomselect-and-slimselect).

[Read more](../table/column-filters.html#filter-multiselectasync)

---

### Defer Loading

The table will be fully loaded after the data is completely ready.
Behind the scenes [wire:init](https://laravel-livewire.com/docs/2.x/defer-loading#introduction) is used

![Output](/_media/examples/defer-loading-example.png)

[Read more](../table/component-settings.html#defer-loading)

---

### Search with whereHasMorph

Now you can search your table if your data source has morphic relationship [whereHasMorph](https://laravel.com/docs/9.x/eloquent-relationships#querying-morph-to-relationships)

---

### BulkAction store

Using the Alpine store, we can track how many items have been selected.
This is useful to count how many items we will export or carry out a mass action.

[Read more](../table/bulk-actions.html#show-number-of-selected-items)

---
