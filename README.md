```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct {
    int id;
    char name[50];
    int age;
    char department[30];
    float salary;
} Employee;

void addEmployee() {
    FILE *fp = fopen("employee.txt", "a");
    Employee e;

    printf("Enter ID: ");
    scanf("%d", &e.id);
    printf("Enter Name: ");
    scanf(" %[^\n]", e.name);
    printf("Enter Age: ");
    scanf("%d", &e.age);
    printf("Enter Department: ");
    scanf(" %[^\n]", e.department);
    printf("Enter Salary: ");
    scanf("%f", &e.salary);

    fwrite(&e, sizeof(Employee), 1, fp);
    fclose(fp);
    printf("✅ Employee added successfully!\n");
}

void displayEmployees() {
    FILE *fp = fopen("employee.txt", "r");
    Employee e;

    printf("\n--- Employee Records ---\n");
    while (fread(&e, sizeof(Employee), 1, fp)) {
        printf("ID: %d\nName: %s\nAge: %d\nDepartment: %s\nSalary: %.2f\n\n",
               e.id, e.name, e.age, e.department, e.salary);
    }
    fclose(fp);
}

void searchEmployee() {
    FILE *fp = fopen("employee.txt", "r");
    Employee e;
    int id, found = 0;

    printf("Enter ID to search: ");
    scanf("%d", &id);

    while (fread(&e, sizeof(Employee), 1, fp)) {
        if (e.id == id) {
            printf("\n✅ Employee Found:\n");
            printf("ID: %d\nName: %s\nAge: %d\nDepartment: %s\nSalary: %.2f\n",
                   e.id, e.name, e.age, e.department, e.salary);
            found = 1;
            break;
        }
    }
    if (!found) {
        printf("❌ Employee with ID %d not found.\n", id);
    }
    fclose(fp);
}

void updateEmployee() {
    FILE *fp = fopen("employee.txt", "r+");
    Employee e;
    int id, found = 0;
    long pos;

    printf("Enter ID to update: ");
    scanf("%d", &id);

    while (fread(&e, sizeof(Employee), 1, fp)) {
        if (e.id == id) {
            pos = ftell(fp) - sizeof(Employee);
            printf("Enter new Name: ");
            scanf(" %[^\n]", e.name);
            printf("Enter new Age: ");
            scanf("%d", &e.age);
            printf("Enter new Department: ");
            scanf(" %[^\n]", e.department);
            printf("Enter new Salary: ");
            scanf("%f", &e.salary);

            fseek(fp, pos, SEEK_SET);
            fwrite(&e, sizeof(Employee), 1, fp);
            found = 1;
            printf("✅ Employee updated successfully!\n");
            break;
        }
    }
    if (!found)
        printf("❌ Employee with ID %d not found.\n", id);

    fclose(fp);
}

void deleteEmployee() {
    FILE *fp = fopen("employee.txt", "r");
    FILE *temp = fopen("temp.txt", "w");
    Employee e;
    int id, found = 0;

    printf("Enter ID to delete: ");
    scanf("%d", &id);

    while (fread(&e, sizeof(Employee), 1, fp)) {
        if (e.id != id) {
            fwrite(&e, sizeof(Employee), 1, temp);
        } else {
            found = 1;
        }
    }

    fclose(fp);
    fclose(temp);

    remove("employee.txt");
    rename("temp.txt", "employee.txt");

    if (found)
        printf("✅ Employee deleted successfully!\n");
    else
        printf("❌ Employee with ID %d not found.\n", id);
}

int main() {
    int choice;

    do {
        printf("\n===== Employee Management System =====\n");
        printf("1. Add Employee\n");
        printf("2. Display All Employees\n");
        printf("3. Search Employee\n");
        printf("4. Update Employee\n");
        printf("5. Delete Employee\n");
        printf("6. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1: addEmployee(); break;
            case 2: displayEmployees(); break;
            case 3: searchEmployee(); break;
            case 4: updateEmployee(); break;
            case 5: deleteEmployee(); break;
            case 6: printf("👋 Exiting...\n"); break;
            default: printf("❌ Invalid choice! Try again.\n");
        }
    } while (choice != 6);

    return 0;
}
```
