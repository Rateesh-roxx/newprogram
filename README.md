# newprogram
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct Student
{
    char usn[20];
    char name[50];
    int semester;
    char dept[50];
    struct Student *next;
};

struct Student *head = NULL;

/* Read student details */
void readStudent(struct Student *newNode)
{
    printf("Enter USN: ");
    scanf(" %19s", newNode->usn);

    printf("Enter Name: ");
    scanf(" %49[^\n]", newNode->name);

    printf("Enter Semester (1-8): ");
    scanf("%d", &newNode->semester);

    printf("Enter Department: ");
    scanf(" %49[^\n]", newNode->dept);

    newNode->next = NULL;
}


/* Create a new student node */
struct Student *createNode()
{
    struct Student *newNode;

    newNode = (struct Student *)malloc(sizeof(struct Student));

    if (newNode == NULL)
    {
        printf("Memory allocation failed\n");
        exit(1);
    }

    readStudent(newNode);

    return newNode;
}

/* Insert student at the end */
void createList()
{
    struct Student *newNode, *temp;
    int n, i;

    printf("Enter number of students: ");
    scanf("%d", &n);

    for (i = 1; i <= n; i++)
    {
        printf("\nEnter details of student %d\n", i);

        newNode = createNode();

        if (head == NULL)
        {
            head = newNode;
        }
        else
        {
            temp = head;

            while (temp->next != NULL)
                temp = temp->next;

            temp->next = newNode;
        }
    }
}

/* Display all student records */
void display()
{
    struct Student *temp = head;

    if (head == NULL)
    {
        printf("Student list is empty\n");
        return;
    }

    printf("\nUSN\t\tName\t\tSemester\tDepartment\n");

    while (temp != NULL)
    {
        printf("%s\t%s\t\t%d\t\t%s\n",
               temp->usn, temp->name,
               temp->semester, temp->dept);

        temp = temp->next;
    }
}

/* Display students of a given semester */
void displaySemester()
{
    struct Student *temp = head;
    int sem, found = 0;

    printf("Enter semester: ");
    scanf("%d", &sem);

    if (sem < 1 || sem > 8)
    {
        printf("Invalid semester\n");
        return;
    }

    while (temp != NULL)
    {
        if (temp->semester == sem)
        {
            printf("\nUSN: %s", temp->usn);
            printf("\nName: %s", temp->name);
            printf("\nSemester: %d", temp->semester);
            printf("\nDepartment: %s\n", temp->dept);

            found = 1;
        }

        temp = temp->next;
    }

    if (found == 0)
        printf("No students found in this semester\n");
}

/* Insert a student after a given name */
void insertAfterName()
{
    struct Student *temp = head;
    struct Student *newNode;
    char searchName[50];

    if (head == NULL)
    {
        printf("Student list is empty\n");
        return;
    }

    printf("Enter the name after which the new student is to be inserted: ");
    scanf(" %49[^\n]", searchName);

    while (temp != NULL && strcmp(temp->name, searchName) != 0)
        temp = temp->next;

    if (temp == NULL)
    {
        printf("Student not found\n");
        return;
    }

    printf("\nEnter new student details\n");
    newNode = createNode();

    newNode->next = temp->next;
    temp->next = newNode;

    printf("Student inserted successfully\n");
}

/* Delete a student based on USN or Name */
void deleteStudent()
{
    struct Student *temp = head;
    struct Student *prev = NULL;
    char key[50];
    int choice, found = 0;

    if (head == NULL)
    {
        printf("Student list is empty\n");
        return;
    }

    printf("\n1. Delete by USN");
    printf("\n2. Delete by Name");
    printf("\nEnter your choice: ");
    scanf("%d", &choice);

    if (choice == 1)
        printf("Enter USN: ");
    else if (choice == 2)
        printf("Enter Name: ");
    else
    {
        printf("Invalid choice\n");
        return;
    }

    scanf(" %49[^\n]", key);

    while (temp != NULL)
    {
        if ((choice == 1 && strcmp(temp->usn, key) == 0) ||
            (choice == 2 && strcmp(temp->name, key) == 0))
        {
            found = 1;
            break;
        }

        prev = temp;
        temp = temp->next;
    }

    if (found == 0)
    {
        printf("Student not found\n");
        return;
    }

    if (prev == NULL)
        head = temp->next;
    else
        prev->next = temp->next;

    free(temp);

    printf("Student record deleted successfully\n");
}

int main()
{
    int choice;

    while (1)
    {
        printf("\n\nSTUDENT RECORD MANAGEMENT");
        printf("\n1. Create Student List");
        printf("\n2. Display All Students");
        printf("\n3. Display Students by Semester");
        printf("\n4. Insert Student After a Given Name");
        printf("\n5. Delete Student");
        printf("\n6. Exit");

        printf("\nEnter your choice: ");
        scanf("%d", &choice);

        switch (choice)
        {
            case 1:
                createList();
                break;

            case 2:
                display();
                break;

            case 3:
                displaySemester();
                break;

            case 4:
                insertAfterName();
                break;

            case 5:
                deleteStudent();
                break;

            case 6:
                exit(0);

            default:
                printf("Invalid choice\n");
        }
    }

    return 0;
}
