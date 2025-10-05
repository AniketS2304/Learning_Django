# 🧩 Django Models — Complete Reference Notes

## 📝 What is a Django Model?
A **Django model** is a Python class that defines your database structure. Each class maps to a database table, and each field maps to a column.

Django ORM (Object Relational Mapper) allows you to interact with the database using Python instead of SQL.

### Example:
```python
from django.db import models

class Student(models.Model):
    name = models.CharField(max_length=30)
    age = models.IntegerField()
    gender = models.CharField(max_length=5, default="Male")
    phone_no = models.CharField(max_length=50)
    student_bio = models.TextField()
    dob = models.DateField()
    student_reg_time = models.DateTimeField(auto_now_add=True)
    percentage = models.FloatField()
    student_email = models.EmailField(unique=True)
    file = models.FileField(upload_to="files/students/")
```
When you run:
```bash
python manage.py makemigrations
python manage.py migrate
```
Django creates the corresponding SQL table and columns automatically.

---

## 🧱 Field Types in Django Models

| Field Type | Description | Example |
|-------------|--------------|----------|
| CharField | Short text | `name = models.CharField(max_length=100)` |
| TextField | Long text | `bio = models.TextField()` |
| IntegerField | Whole numbers | `age = models.IntegerField()` |
| FloatField | Decimal values | `percentage = models.FloatField()` |
| DecimalField | Precise decimal (currency) | `price = models.DecimalField(max_digits=8, decimal_places=2)` |
| BooleanField | True/False | `is_active = models.BooleanField(default=True)` |
| DateField | Date only | `dob = models.DateField(auto_now_add=True)` |
| DateTimeField | Date + Time | `created_at = models.DateTimeField(auto_now_add=True)` |
| EmailField | Email validation | `email = models.EmailField(unique=True)` |
| SlugField | URL-friendly | `slug = models.SlugField(unique=True)` |
| URLField | Website URLs | `website = models.URLField()` |
| UUIDField | Unique identifier | `uuid = models.UUIDField(primary_key=True, default=uuid.uuid4, editable=False)` |
| JSONField | JSON data | `meta = models.JSONField(default=dict)` |

---

## 📂 File and Image Handling

### 🗂 FileField
```python
file = models.FileField(upload_to="files/students/")
```

### 🖼 ImageField
```python
profile_pic = models.ImageField(upload_to="images/students/")
```

Settings in `settings.py`:
```python
MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'
```

---

## ⚙️ Common Field Options

| Option | Description | Example |
|---------|--------------|----------|
| default | Default value | `gender = models.CharField(default='Male', max_length=10)` |
| null=True | Allows NULL | `bio = models.TextField(null=True)` |
| blank=True | Can be empty | `bio = models.TextField(blank=True)` |
| unique=True | Unique constraint | `email = models.EmailField(unique=True)` |
| choices | Restricted options | `status = models.CharField(max_length=10, choices=[('A', 'Active'), ('I', 'Inactive')])` |

---

## 🔗 Model Relationships

### One-to-One
```python
class Profile(models.Model):
    student = models.OneToOneField(Student, on_delete=models.CASCADE)
```

### One-to-Many
```python
class Course(models.Model):
    title = models.CharField(max_length=100)

class Enrollment(models.Model):
    student = models.ForeignKey(Student, on_delete=models.CASCADE)
    course = models.ForeignKey(Course, on_delete=models.CASCADE)
```

### Many-to-Many
```python
class Subject(models.Model):
    name = models.CharField(max_length=100)
    students = models.ManyToManyField(Student)
```

---

## ⚙️ Meta Class Configuration
```python
class Student(models.Model):
    name = models.CharField(max_length=100)
    email = models.EmailField()

    class Meta:
        db_table = "student_info"
        ordering = ['name']
        verbose_name = "Student Record"
        verbose_name_plural = "Student Records"
```

---

## 🧩 Model Methods
```python
class Student(models.Model):
    name = models.CharField(max_length=50)
    percentage = models.FloatField()

    def is_passed(self):
        return self.percentage >= 40

    def __str__(self):
        return f"{self.name} ({self.percentage}%)"
```

---

## 🧮 Django ORM — CRUD Operations

### Create
```python
Student.objects.create(name="Aniket", age=21, email="ani@example.com")
```

### Read
```python
Student.objects.all()
Student.objects.filter(age__gt=18)
```

### Update
```python
student = Student.objects.get(id=1)
student.name = "Ani"
student.save()
```

### Delete
```python
student = Student.objects.get(id=1)
student.delete()
```

---

## ⚡ Common ORM Query Examples

| Operation | Example |
|------------|----------|
| Get first record | `Student.objects.first()` |
| Get last record | `Student.objects.last()` |
| Count records | `Student.objects.count()` |
| Filter substring | `Student.objects.filter(name__icontains='ani')` |
| Order by field | `Student.objects.order_by('-age')` |
| Exclude condition | `Student.objects.exclude(age__lt=18)` |

---

## 🧠 Model Inheritance
```python
class Person(models.Model):
    name = models.CharField(max_length=50)
    dob = models.DateField()

class Student(Person):
    roll_no = models.IntegerField()
```

---

## ⚙️ Migrations
```bash
python manage.py makemigrations
python manage.py migrate
python manage.py showmigrations
```

---

## 🧰 Common Model Commands
| Command | Purpose |
|----------|----------|
| `python manage.py shell` | Open Django shell |
| `Student.objects.all()` | Fetch all records |
| `Student.objects.filter(age=21)` | Filter records |
| `Student.objects.values('name', 'email')` | Fetch specific fields |

---

## ✅ Summary
- Django Models = database tables represented as Python classes.
- ORM allows CRUD without SQL.
- Fields define structure; Meta defines behavior.
- Relationships connect data across tables.
- File/Image fields simplify media management.
- Migrations keep your database schema consistent.
