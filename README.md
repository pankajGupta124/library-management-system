#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_TITLE 100
#define MAX_AUTHOR 100

// Structure for a Book
typedef struct Book {
    int id;
    char title[MAX_TITLE];
    char author[MAX_AUTHOR];
    struct Book* next;
} Book;

// Global pointer to the head of the linked list
Book* head = NULL;
int nextID = 1001;

// Function declarations
void addBook();
void displayBooks();
void searchBook();
void deleteBook();
void exitProgram();
Book* createBook(char title[], char author[]);
void clearMemory();

int main() {
    int choice;

    while (1) {
        printf("\n====== Library Management System ======\n");
        printf("1. Add Book\n");
        printf("2. Display All Books\n");
        printf("3. Search Book\n");
        printf("4. Delete Book\n");
        printf("5. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);
        getchar();  // Clear newline from input buffer

        switch (choice) {
            case 1: addBook(); break;
            case 2: displayBooks(); break;
            case 3: searchBook(); break;
            case 4: deleteBook(); break;
            case 5: exitProgram(); break;
            default: printf("Invalid choice. Please try again.\n");
        }
    }

    return 0;
}

// Function to create a new Book node
Book* createBook(char title[], char author[]) {
    Book* newBook = (Book*)malloc(sizeof(Book));
    newBook->id = nextID++;
    strcpy(newBook->title, title);
    strcpy(newBook->author, author);
    newBook->next = NULL;
    return newBook;
}

// Function to add a new book
void addBook() {
    char title[MAX_TITLE], author[MAX_AUTHOR];

    printf("Enter Book Title: ");
    fgets(title, MAX_TITLE, stdin);
    title[strcspn(title, "\n")] = '\0';  // Remove newline

    printf("Enter Author Name: ");
    fgets(author, MAX_AUTHOR, stdin);
    author[strcspn(author, "\n")] = '\0';  // Remove newline

    Book* newBook = createBook(title, author);

    // Insert at the beginning
    newBook->next = head;
    head = newBook;

    printf("Book added successfully! ID: %d\n", newBook->id);
}

// Function to display all books
void displayBooks() {
    if (head == NULL) {
        printf("No books available.\n");
        return;
    }

    Book* temp = head;
    printf("\nList of Books:\n");
    while (temp != NULL) {
        printf("ID: %d | Title: %s | Author: %s\n", temp->id, temp->title, temp->author);
        temp = temp->next;
    }
}

// Function to search for a book by ID or title
void searchBook() {
    if (head == NULL) {
        printf("No books available to search.\n");
        return;
    }

    char keyword[MAX_TITLE];
    printf("Enter Book ID or Title to search: ");
    fgets(keyword, MAX_TITLE, stdin);
    keyword[strcspn(keyword, "\n")] = '\0';

    Book* temp = head;
    int found = 0;

    while (temp != NULL) {
        char idStr[10];
        sprintf(idStr, "%d", temp->id);
        if (strcmp(idStr, keyword) == 0 || strcasecmp(temp->title, keyword) == 0) {
            printf("\nBook Found:\nID: %d\nTitle: %s\nAuthor: %s\n", temp->id, temp->title, temp->author);
            found = 1;
            break;
        }
        temp = temp->next;
    }

    if (!found)
        printf("Book not found.\n");
}

// Function to delete a book by ID
void deleteBook() {
    if (head == NULL) {
        printf("No books to delete.\n");
        return;
    }

    int delID;
    printf("Enter Book ID to delete: ");
    scanf("%d", &delID);
    getchar();  // Clear input buffer

    Book *temp = head, *prev = NULL;

    while (temp != NULL && temp->id != delID) {
        prev = temp;
        temp = temp->next;
    }

    if (temp == NULL) {
        printf("Book ID not found.\n");
        return;
    }

    if (prev == NULL) {
        head = temp->next;  // Deleting head
    } else {
        prev->next = temp->next;
    }

    free(temp);
    printf("Book deleted successfully.\n");
}

// Function to exit and clear memory
void exitProgram() {
    clearMemory();
    printf("Exiting program. Memory cleared. Goodbye!\n");
    exit(0);
}

// Function to free all dynamically allocated memory
void clearMemory() {
    Book* temp;
    while (head != NULL) {
        temp = head;
        head = head->next;
        free(temp);
    }
}
