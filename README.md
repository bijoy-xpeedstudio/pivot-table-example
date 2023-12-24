
## How to use pivot table by using Laravel Eloquent

code sample with Employee with Department



## Usage/Examples
Employee Model

```php
<?php

namespace Modules\People\Models\Employee;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Modules\People\Models\Department\DepartmentModel;
use SyntheticRevisions\Trait\RevisionableTrait;
use SyntheticCore\Abstracts\AbstractModelBase;
use SyntheticComments\Trait\CommentTrait;
use SyntheticFilters\Traits\FilterTrait;


class EmployeeModel extends AbstractModelBase
{
    use HasFactory, CommentTrait, FilterTrait;
    protected $primaryKey = '_id';
    protected $fillable = [
        'name', 'departments'
    ];

    protected $collection = 'employees';
    protected $casts = [
        // 'department_id' => 'array',
    ];
    // public function department()
    // {
    //     return $this->belongsTo(DepartmentModel::class);
    // }
    public function EmployeeDeletr()
    {
        return $this->deleter(DepartmentModel::class);
    }

    public function departments()
    {
        return $this->belongsToMany(DepartmentModel::class, '_id');
    }
}
```
Department Model

```php
<?php

namespace Modules\People\Models\Department;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Modules\People\Models\Employee\EmployeeModel;
use SyntheticComments\Trait\CommentTrait;
use SyntheticCore\Abstracts\AbstractModelBase;
use SyntheticFilters\Traits\FilterTrait;
use SyntheticRevisions\Trait\RevisionableTrait;

class DepartmentModel extends AbstractModelBase
{
    use HasFactory, CommentTrait, FilterTrait;
    protected $primaryKey = '_id';
    protected $fillable = [
        'name'
    ];
    protected $collection = 'departments';

    public function employees()
    {
        return $this->hasMany(EmployeeModel::class, 'department_id', '_id');
    }
}

```
Use Examples

```php
// $data = EmployeeModel::where('name', 'bijoy')->first();
        // $employee = new EmployeeModel();
        // $employee->name = 'bijoy';
        // $employee->save();

        // $comment1 = new DepartmentModel(['name' => 'mysql']);
        // $comment1->save();
        // $comment2 = new DepartmentModel(['name' => 'laravel']);
        // $comment2->save();

        // $employee->departments()->saveMany([$comment1, $comment2]);

        $employee = EmployeeModel::where('name', 'bijoy')->first();
        $department = DepartmentModel::where('name', 'mysql')->first();

        $employee->departments()->save($department);
        return response()->json($employee);
```
Get Relaton Data

```php
$emp = EmployeeModel::where('name', 'bijoy')->first();
        return response()->json($emp->departments);
```