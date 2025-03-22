import os

class Book:
    def __init__(self, title, author, isbn):
        self.title = title
        self.author = author
        self.isbn = isbn

    def __str__(self):
        return f"Title: {self.title}, Author: {self.author}, ISBN: {self.isbn}"

class Library:
    def __init__(self):
        self.books = []
        self.load_books()

    def load_books(self):
        """Load books from a file if exists"""
        if os.path.exists("library_data.txt"):
            with open("library_data.txt", "r") as file:
                for line in file:
                    title, author, isbn = line.strip().split(", ")
                    self.books.append(Book(title, author, isbn))

    def save_books(self):
        """Save the books to a file"""
        with open("library_data.txt", "w") as file:
            for book in self.books:
                file.write(f"{book.title}, {book.author}, {book.isbn}\n")

    def add_book(self, title, author, isbn):
        """Add a new book to the library"""
        new_book = Book(title, author, isbn)
        self.books.append(new_book)
        self.save_books()
        print("Book added successfully!")

    def remove_book(self, isbn):
        """Remove a book from the library by ISBN"""
        for book in self.books:
            if book.isbn == isbn:
                self.books.remove(book)
                self.save_books()
                print("Book removed successfully!")
                return
        print("Book with the given ISBN not found.")

    def list_books(self):
        """List all books in the library"""
        if not self.books:
            print("No books in the library.")
            return
        for book in self.books:
            print(book)

    def search_book(self, search_term):
        """Search for a book by title, author, or ISBN"""
        found_books = [book for book in self.books if search_term.lower() in book.title.lower() or search_term.lower() in book.author.lower() or search_term.lower() in book.isbn]
        if found_books:
            for book in found_books:
                print(book)
        else:
            print("No books found matching the search term.")

def main():
    library = Library()

    while True:
        print("\nLibrary Management System")
        print("1. Add a Book")
        print("2. Remove a Book")
        print("3. List all Books")
        print("4. Search for a Book")
        print("5. Exit")

        choice = input("Choose an option (1-5): ")

        if choice == '1':
            title = input("Enter book title: ")
            author = input("Enter book author: ")
            isbn = input("Enter book ISBN: ")
            library.add_book(title, author, isbn)
        
        elif choice == '2':
            isbn = input("Enter ISBN of the book to remove: ")
            library.remove_book(isbn)
        
        elif choice == '3':
            library.list_books()
        
        elif choice == '4':
            search_term = input("Enter search term (title, author, or ISBN): ")
            library.search_book(search_term)
        
        elif choice == '5':
            print("Exiting Library Management System.")
            break
        
        else:
            print("Invalid option. Please choose between 1-5.")

if __name__ == "__main__":
    main()
