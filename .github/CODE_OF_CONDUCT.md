php artisan make:model Student -m

// database/migrations/xxxx_xx_xx_create_students_table.php

public function up()
{
    Schema::create('students', function (Blueprint $table) {
        $table->id();
        $table->string('name');
        $table->integer('age');
        $table->string('address');
        $table->string('course');
        $table->string('subject');
        $table->timestamps();
    });
}

php artisan migrate
php artisan make:controller StudentController

// app/Http/Controllers/StudentController.php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Models\Student;

class StudentController extends Controller
{
    public function index()
    {
        return Student::all();
    }

    public function show($id)
    {
        return Student::findOrFail($id);
    }

    public function store(Request $request)
    {
        $student = Student::create($request->all());
        return response()->json($student, 201);
    }

    public function update(Request $request, $id)
    {
        $student = Student::findOrFail($id);
        $student->update($request->all());
        return response()->json($student, 200);
    }

    public function destroy($id)
    {
        Student::findOrFail($id)->delete();
        return response()->json(null, 204);
    }
}
// routes/api.php

use App\Http\Controllers\StudentController;

Route::resource('students', StudentController::class);


