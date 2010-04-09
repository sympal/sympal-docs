The Sympal Data Grid component allows you to easily generate data grids based on Doctrine Query objects. This chapter of the documentation will explain the component and show some code examples.

Before continuing let's explore a simple working example with screenshots so that you'll want to
continue reading! :)

Here is the example code executed to produce the screenshot:

    [php]
    $dataGrid = sfSympalDataGrid::create('ContentType', 't')
      ->addColumn('t.id')
      ->addColumn('t.name')
      ->addColumn('t.plugin_name');

    echo $dataGrid->render();

Now with a default Sympal database it would output the following data grid:

[asset:47]

## Create

You can create a `sfSympalDataGrid` instance by using the `create()` static method:

    [php]
    $dataGrid = sfSympalDataGrid::create('User', 'u');

### From Existing Query

You can of course always create the data grid from an existing `Doctrine_Query` instance:

    [php]
    $query = Doctrine_Core::getTable('User')
       ->createQuery('u');
    $dataGrid = sfSympalDataGrid::create($query);

### From Pager

Lastly, you can create a data grid from an instance of `sfDoctrinePager`:

    [php]
    $pager = new sfDoctrinePager('User');
    $dataGrid = sfSympalDataGrid::create($pager);

No matter how you create the data grid instance, afterwords you can retrieve the `Doctrine_Query` and `sfDoctrinePager` instance used:

    [php]
    $pager = $dataGrid->getPager();
    $query = $dataGrid->getQuery();

## Building

In this section we'll talk more about the API of `sfSympalDataGrid` and how you can use it to easily build data grids.

### Configuring Data Grid Columns

If you want to add and configure the columns of the data grid you can use the `addColumn()` and `configureColumn()` methods:

    [php]
    $dataGrid = sfSympalDataGrid::create('User', 'u')
      ->addColumn('u.username');

The above example would output a data grid with only one column that holds the username of each user.

> **NOTE**
> Please note that if you do not call `addColumn()` at least once then all columns
> for the root model (User) will be added for you.

If you want to configure an existing column and change some options you can do so by using the `configureColumn()` method:

    [php]
    $dataGrid->configureColumn('u.username', 'label=My Custom Label');

This is useful if you want to just change something about a column that was already added previously.

## Rendering

Rendering a data grid is as easy as calling the `render()` method:

    [php]
    $dataGrid = sfSympalDataGrid::create('User', 'u')
      ->where('u.is_super_admin = 1');

    echo $dataGrid->render();

## Customizing

Customizing a data grid is pretty simple. It allows you to override things a few different ways. In this section we'll document the different ways with a few examples.

### Data Grid Template

You can customize the template used to produce the data grid by specifying a custom rendering module to use:

    [php]
    $dataGrid->setRenderingModule('my_custom_module');

Inside that modules templates folder you just need to create a component or partial named `list` with a template `_list.php`. The default template that comes with Sympal looks like this:

    <table class="<?php echo $dataGrid->getClass() ?>" id="<?php echo $dataGrid->getId() ?>">
    <thead>
      <tr>
        <?php foreach ($dataGrid->getColumns() as $column): ?>
          <th><?php echo $dataGrid->getColumnSortLink($column) ?></th>
        <?php endforeach; ?>
      </tr>
    </thead>
    <tbody>
      <?php foreach ($dataGrid->getRows($hydrationMode) as $row): ?>
        <tr>
          <?php foreach ($row as $value): ?>
            <td><?php echo $value ?></td>
          <?php endforeach; ?>
        </tr>
      <?php endforeach; ?>
    </tbody>
    </table>

The default module used for the data grid is a module named `sympal_data_grid` and it comes bundled with `sfSympalPlugin`. If you wish, you can simply create your own module with the same name and start overriding the templates included with Sympal.

### Column Values

The rendering of a row column's values can be overridden two different ways. You can specify a custom method used to retrieve the value or use a Symfony partial or component to render the value.

#### Method

Here is an example where we use a custom method to render the value of a column:

    [php]
    $dataGrid = sfSympalDataGrid::create('User', 'u')
      ->addColumn('u.name', 'method=getFullName');

Now since `name` is not a real column on the `User` model we must define a method named `getFullName()` to get the value:

    [php]
    class User extends BaseUser
    {
       public function getFullName()
       {
          return $this->first_name.' '.$this->last_name;
       }
    }

#### Partial

You can also render the value using a Symfony partial or component:

    [php]
    $dataGrid = sfSympalDataGrid::create('User', 'u')
      ->addColumn('u.username', 'renderer=my_module/my_partial');

Now in `my_module/templates/_my_partial.php` you can add some code like the following:

    [php]
    echo $record->username; // do something else

## Sorting

All data grids are sortable by default but this can be configured of course.

You can disable sorting for the entire data grid:

    [php]
    $dataGrid->isSortable(false);

Or you can just disable the sorting for a single column:

    [php]
    $dataGrid->addColumn('u.username', 'is_sortable=false');