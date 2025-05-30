//-----------------Header section---------------------------
//--------------Name: Mahnoor-------------------------------
//--------------Roll no: 04072413004------------------------
//--------------Semester: 2---------------------------------
//----------Smart University Management System--------------
#include<iostream>
using namespace std;
//----Virtual base class (Person)----
class Person {
    protected:
        string name;
        int age;
    public:
        Person(string n = "", int a = 0) {
            name = n;
            age = a;
        }
};
//-----now User which is inherited from Person virtually----
class User: virtual public Person{
	protected:
		int id;
		static int totalusers;
	public:
		User(string n="", int a=0, int i=0): Person(n, a){
			name=n;
			age=a;
			id=i;
			totalusers++;
		}
		static int gettotaluser(){
			return totalusers;
		}
		virtual void displayinfo() const=0;
		virtual void calculateperformance() const=0;
};
int User:: totalusers=0;
class Student;
//----------Class Course-------
class Course{
	int coursecode;
	string coursetitle;
	public:
		Course(string title="", int code=0){
			coursetitle=title;
			coursecode=code;	
		}
		void displaycourse() const{
			cout<<"---Course info---"<<endl;
			cout<<"Course code: "<<coursecode<<endl;
			cout<<"Course title: "<<coursetitle<<endl;
		}
		bool operator == (const Course& other) const{
			return(coursecode== other.coursecode && coursetitle==other.coursetitle);
		}
		friend ostream& operator<<(ostream& out, const Course& c) {
        out << "[Course Code: " << c.coursecode << ", Title: " << c.coursetitle << "]";
        return out;
    	}
		// declaration of friend function
		friend void assignCourse(Student&, Course&);
};
class Student: public User{
	private:
		float gpa;
		int enrolledcoursecode;
		string enrolledcoursetitle;
	public:
		Student(string n = "", int a = 0, int i = 0, float g = 0.0, int enco = 0, string enti = "") 
        : User(n, a, i){
			gpa=g;
			enrolledcoursecode=enco;
			enrolledcoursetitle=enti;
		}
		void displayinfo() const override{
			cout<<"Name: "<<name<<endl;
			cout<<"Age: "<<age<<endl;
			cout<<"Student id: "<<id<<endl;
			cout<<"GPA: "<<gpa<<endl;
		}
		void calculateperformance() const override{
			cout<<"Student performance: "<<endl;
			if(gpa>=3.5)cout<<"Excellent performance."<<endl;
			else if(gpa<=3.4 && gpa>=2.5)cout<<"Excellent performance."<<endl;
			else cout<<"Needs improvement."<<endl;
		}
		friend void assignCourse(Student&, Course&);
};
class Professor: public User{
	private:
		float salary;
	public:
		Professor(string n = "", int a = 0, int i = 0, float sal = 0.0) 
        : User(n, a, i){
			salary=sal;
		}
		void displayinfo() const override{
			cout<<"Name: "<<name<<endl;
			cout<<"Age: "<<age<<endl;
			cout<<"Professor id: "<<id<<endl;
			cout<<"Salary: "<<salary<<endl;
		}
		void calculateperformance() const override{
			cout<<"Professor performance: "<<endl;
			if(salary>=100000)cout<<"Senior level."<<endl;
			else cout<<"Junior level."<<endl;
		}
};
void assignCourse(Student& s, Course& c){
	s.enrolledcoursecode=c.coursecode;
	s.enrolledcoursetitle=c.coursetitle;
	cout<<"Courses assigned successfully. "<<endl;
}
template<class T>
class Calculator{
	private:
		T* arr;
		int size;
	public:
		Calculator(T* a, int s){
			arr=a;
			size=s;
		}
		T sumRecursive(int index=0){
			if(index==size){
				return 0;
			}
			return arr[index]+ sumRecursive(index+1);
		}
		T operator / (int divisor){
			return sumRecursive()/divisor;
		}
		void displayGpas(){
			cout<<"GPAs: ";
			for(int i=0; i<size; i++){
				cout<<arr[i]<<" ";
			}
			cout<<endl;
		}
};
int sumOfDigits(int x){
	if(x==0)
	return 0;
	return(x%10)+sumOfDigits(x/10);
}
float power(float base, int exp){
	if(exp==0){
		return 1;
	}
	float half= power(base, exp/2);
	if(exp%2==0){
		return half*half;
	}
	else{
		return base*half*half;
	}
	
}
struct Professorscore{
	string name;
	float score;
};
template<class T>
void sortrecursive(T arr[], int n){
	if(n==1)
	return;
	for(int i=0; i<n-1; i++){
		if(arr[i].score< arr[i+1].score){
			swap(arr[i], arr[i+1]);
		}
	}
	sortrecursive(arr, n-1);
}
//---func to display ranking----
void displayranking(Professorscore profs[], int size){
	sortrecursive(profs, size);
	for(int i = 0; i < size; i++) {
        cout << i + 1 << ". " << profs[i].name << " --- Score: " << profs[i].score << endl;
    }
}
int main(){
	 Student student;
    Professor professor;
    Course course;
    bool studentCreated = false;
	bool professorCreated = false;
	bool courseAssigned = false;
	int choice;
    do {
        cout << "Welcome to Smart University Management System"<<endl;
        cout << "--------------------------------------------"<<endl;
        cout << "1. Create Student"<<endl;
        cout << "2. Create Professor"<<endl;
        cout << "3. Assign Course"<<endl;
        cout << "4. Calculate GPA"<<endl;
        cout << "5. Show User Info"<<endl;
        cout << "6. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch(choice) {
            case 1: {
                string name;
                int age, id;
                float gpa;
                cout << "Enter student name: ";
                cin.ignore();
                getline(cin, name);
                cout << "Enter age: ";
                cin >> age;
                cout << "Enter ID: ";
                cin >> id;
                cout << "Enter GPA: ";
                cin >> gpa;
                student = Student(name, age, id, gpa);
                studentCreated = true;
                cout << "Student created successfully."<<endl;
                break;
            }
            case 2: {
                string name;
                int age, id;
                float salary;
                cout << "Enter professor name: ";
                cin.ignore();
                getline(cin, name);
                cout << "Enter age: ";
                cin >> age;
                cout << "Enter ID: ";
                cin >> id;
                cout << "Enter Salary: ";
                cin >> salary;
                professor = Professor(name, age, id, salary);
                professorCreated = true;
                cout << "Professor created successfully."<<endl;
                break;
            }
            case 3: {
                if (!studentCreated) {
                    cout << "Please create a student first."<<endl;
                    break;
                }
                string title;
                int code;
                cout << "Enter course title: ";
                cin.ignore();
                getline(cin, title);
                cout << "Enter course code: ";
                cin >> code;
                course = Course(title, code);
                assignCourse(student, course);
                courseAssigned = true;
                break;
            }
            case 4: {
                if (!studentCreated) {
                    cout << "Create student first to calculate GPA performance."<<endl;
                    break;
                }
                student.calculateperformance();
                break;
            }
            case 5: {
                cout << "--- User Info ---"<<endl;
                if (studentCreated) {
                    cout << "------Student Info:------"<<endl;
                    student.displayinfo();
                }
                if (professorCreated) {
                    cout << "------Professor Info:------"<<endl;
                    professor.displayinfo();
                    professor.calculateperformance();
                }
                break;
            }
            case 6: {
                cout << "Exiting... Goodbye!"<<endl;
                break;
            }
            default:
                cout << "Invalid choice! Please try again."<<endl;
        }

    } while (choice != 6);
	return 0;
}
