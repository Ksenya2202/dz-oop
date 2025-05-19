#Задание 1
class Student:
    def __init__(self, name, surname, gender):
        self.name = name
        self.surname = surname
        self.gender = gender
        self.finished_courses = []
        self.courses_in_progress = []
        self.grades = {}

   #Задание 2
    def rate_lecturer(self, lecturer, course, grade):
        if isinstance(lecturer, Lecturer) and course in self.courses_in_progress and course in lecturer.courses_attached:
            if 0 <= grade <= 10:
                lecturer.grades.setdefault(course, []).append(grade)
            else:
                return "Ошибка: оценка от 0 до 10"
        else:
            return "Ошибка при выставлении оценки"

    def get_average_grade(self):
        all_grades = [g for grades in self.grades.values() for g in grades]
        return round(sum(all_grades) / len(all_grades), 1) if all_grades else 0

    def __str__(self):
        avg_grade = self.get_average_grade()
        courses_in_progress = ', '.join(self.courses_in_progress)
        finished_courses = ', '.join(self.finished_courses)
        return (f"Имя: {self.name}\n"
                f"Фамилия: {self.surname}\n"
                f"Средняя оценка за домашние задания: {avg_grade}\n"
                f"Курсы: {courses_in_progress}\n"
                f"Завершённые курсы: {finished_courses}")

    def __lt__(self, other):
        if not isinstance(other, Student):
            raise TypeError("Нельзя сравнивать")
        return self.get_average_grade() < other.get_average_grade()


class Mentor:
    def __init__(self, name, surname):
        self.name = name
        self.surname = surname
        self.courses_attached = []


class Lecturer(Mentor):
    def __init__(self, name, surname):
        super().__init__(name, surname)
        self.grades = {}

    def get_average_grade(self):
        all_grades = [g for grades in self.grades.values() for g in grades]
        return round(sum(all_grades) / len(all_grades), 1) if all_grades else 0

   # Задание 3
    def __str__(self):
        avg_grade = self.get_average_grade()
        return (f"Имя: {self.name}\n"
                f"Фамилия: {self.surname}\n"
                f"Средняя оценка за лекции: {avg_grade}")

    def __lt__(self, other):
        if not isinstance(other, Lecturer):
            raise TypeError("Нельзя сравнивать")
        return self.get_average_grade() < other.get_average_grade()


class Reviewer(Mentor):
    def rate_hw(self, student, course, grade):
        if isinstance(student, Student) and course in self.courses_attached and course in student.courses_in_progress:
            if course in student.grades:
                student.grades[course].append(grade)
            else:
                student.grades[course] = [grade]
        else:
            return 'Ошибка'

    def __str__(self):
        return f"Имя: {self.name}\nФамилия: {self.surname}"


# Студенты
student1 = Student('Андрей', 'Андреев', 'мальчик')
student1.courses_in_progress += ['Python', 'Git']
student1.finished_courses += ['Введение в программирование']

student2 = Student('Алена', 'Сидорова', 'девочка')
student2.courses_in_progress += ['Python']
student2.finished_courses += ['Введение в програмирование']

# Проверяющие
reviewer1 = Reviewer('Иван', 'Иванов')
reviewer1.courses_attached += ['Python', 'Git']

reviewer2 = Reviewer('Александр', 'Александров')
reviewer2.courses_attached += ['Python']

# Лекторы
lecturer1 = Lecturer('Петр', 'Петров')
lecturer1.courses_attached += ['Python', 'Git']

lecturer2 = Lecturer('Петр', 'Сидоров')
lecturer2.courses_attached += ['Python']

# Проверяющие ставят оценки
reviewer1.rate_hw(student1, 'Python', 9.9)
reviewer1.rate_hw(student1, 'Git', 9.9)
reviewer1.rate_hw(student2, 'Python', 9.9)

reviewer2.rate_hw(student2, 'Python', 9.9)

# Студенты ставят оценки
student1.rate_lecturer(lecturer1, 'Python', 9.9)
student1.rate_lecturer(lecturer1, 'Git', 9.9)
student2.rate_lecturer(lecturer1, 'Python', 9.9)
student2.rate_lecturer(lecturer2, 'Python', 9.9)

print("Студенты:")
print(student1)
print()
print(student2)
print("\nПроверяющие:")
print(reviewer1)
print()
print(reviewer2)
print("\nЛекторы:")
print(lecturer1)
print()
print(lecturer2)

# Сравнение объектов
print("\nСравнение:")
print(f"student1 < student2: {student1 < student2}")
print(f"lecturer1 > lecturer2: {lecturer1 > lecturer2}")

 #Подсчет средних оценок

def average_student_grade_by_course(students, course):
    all_grades = []
    for student in students:
        if course in student.grades:
            all_grades.extend(student.grades[course])
    return round(sum(all_grades) / len(all_grades), 2) if all_grades else 0


def average_lecturer_grade_by_course(lecturers, course):
    all_grades = []
    for lecturer in lecturers:
        if course in lecturer.grades:
            all_grades.extend(lecturer.grades[course])
    return round(sum(all_grades) / len(all_grades), 2) if all_grades else 0


# Задание 4
print("\nСредние оценки:")
print(f"Средняя оценка студентов по курсу 'Python': {average_student_grade_by_course([student1, student2], 'Python')}")
print(f"Средняя оценка лекторов по курсу 'Python': {average_lecturer_grade_by_course([lecturer1, lecturer2], 'Python')}")
