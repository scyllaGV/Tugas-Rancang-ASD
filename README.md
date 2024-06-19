# Tugas-Rancang-ASD



#include <iostream>
#include <vector>
#include <fstream>
#include <algorithm>
#include <string>
#include <iomanip>
#include <thread>
#include <chrono>

#ifdef _WIN32
#include <windows.h>
#else
#include <unistd.h>
#endif

using namespace std;

struct Book {
    int id;
    string title;
    string author;
};

vector<Book> library;

// Function Prototypes
void menu();
void addBook();
void viewBooks();
void searchBooks();
void deleteBook();
void updateBook();
void sortBooks();
void kritiksaran();
void saveToFile();
void loadFromFile();
void clearScreen();
void delayPrint(string, int);
void setTextColorBlue();
void resetTextColor();
void beepOnError();

// Main Function
int main() {
    loadFromFile();
    while (true) {
        menu();
    }
    return 0;
}

void clearScreen() {
#ifdef _WIN32
    system("cls");
#else
    system("clear");
#endif
}

void delayPrint(string message, int millisPerChar) {
    for (char c : message) {
        cout << c << flush;
        this_thread::sleep_for(chrono::milliseconds(millisPerChar));
    }
}

void setTextColorBlue() {
#ifdef _WIN32
    SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), FOREGROUND_BLUE | FOREGROUND_INTENSITY);
#else
    cout << "\033[1;34m";
#endif
}

void resetTextColor() {
#ifdef _WIN32
    SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), 15);
#else
    cout << "\033[0m";
#endif
}

void beepOnError() {
#ifdef _WIN32
    Beep(750, 300);
#else
    cout << "\a";
#endif
}

void menu() {
    clearScreen();
    setTextColorBlue();
    cout << "=====================\n";
    cout << "      PERPUSTAKAAN      \n";
    cout << "=====================\n";
    cout << "1. Tambah Buku\n";
    cout << "2. Lihat Buku\n";
    cout << "3. Cari Buku\n";
    cout << "4. Hapus Buku\n";
    cout << "5. Update Buku\n";
    cout << "6. Sortir Buku\n";
    cout << "7. Kritik saran\n";
    cout << "8. Simpan dan Keluar\n";
    cout << "Pilihan: ";
    resetTextColor();

    int choice;
    cin >> choice;
    cin.ignore();

    switch (choice) {
        case 1:
            addBook();
            break;
        case 2:
            viewBooks();
            break;
        case 3:
            searchBooks();
            break;
        case 4:
            deleteBook();
            break;
        case 5:
            updateBook();
            break;
        case 6:
            sortBooks();
            break;
        case 7:
            kritiksaran();
            break;
        case 8:
            saveToFile();
            exit(0);
        default:
            beepOnError();
            setTextColorBlue();
            cout << "Pilihan tidak valid!" << endl;
            resetTextColor();
            this_thread::sleep_for(chrono::seconds(1));
    }
}

void addBook() {
    clearScreen();
    Book newBook;
    setTextColorBlue();
    cout << "ID Buku: ";
    resetTextColor();
    cin >> newBook.id;
    cin.ignore();
    setTextColorBlue();
    cout << "Judul Buku: ";
    resetTextColor();
    getline(cin, newBook.title);
    setTextColorBlue();
    cout << "Penulis Buku: ";
    resetTextColor();
    getline(cin, newBook.author);
    library.push_back(newBook);
    setTextColorBlue();
    cout << "Buku berhasil ditambahkan!" << endl;
    resetTextColor();
    this_thread::sleep_for(chrono::seconds(1));
}

void viewBooks() {
    clearScreen();
    setTextColorBlue();
    if (library.empty()) {
        cout << "Tidak ada buku dalam perpustakaan." << endl;
        beepOnError();
    } else {
        cout << left << setw(5) << "ID" << setw(30) << "Judul" << setw(20) << "Penulis" << endl;
        cout << "--------------------------------------------------------" << endl;
        for (const auto& book : library) {
            cout << left << setw(5) << book.id << setw(30) << book.title << setw(20) << book.author << endl;
        }
    }
    resetTextColor();
    cout << "Tekan Enter untuk kembali ke menu...";
    cin.get();
}

void searchBooks() {
    clearScreen();
    setTextColorBlue();
    cout << "Masukkan judul buku yang ingin dicari: ";
    resetTextColor();
    string searchTitle;
    getline(cin, searchTitle);
    transform(searchTitle.begin(), searchTitle.end(), searchTitle.begin(), ::tolower);
    bool found = false;
    setTextColorBlue();
    for (const auto& book : library) {
        string title = book.title;
        transform(title.begin(), title.end(), title.begin(), ::tolower);
        if (title.find(searchTitle) != string::npos) {
            cout << left << setw(5) << "ID" << setw(30) << "Judul" << setw(20) << "Penulis" << endl;
            cout << left << setw(5) << book.id << setw(30) << book.title << setw(20) << book.author << endl;
            found = true;
        }
    }
    if (!found) {
        cout << "Buku tidak ditemukan." << endl;
        beepOnError();
    }
    resetTextColor();
    cout << "Tekan Enter untuk kembali ke menu...";
    cin.get();
}

void deleteBook() {
    clearScreen();
    setTextColorBlue();
    cout << "Masukkan ID buku yang ingin dihapus: ";
    resetTextColor();
    int id;
    cin >> id;
    cin.ignore();
    auto it = remove_if(library.begin(), library.end(), [id](Book& book) { return book.id == id; });
    if (it != library.end()) {
        library.erase(it, library.end());
        setTextColorBlue();
        cout << "Buku berhasil dihapus!" << endl;
    } else {
        beepOnError();
        setTextColorBlue();
        cout << "Buku dengan ID tersebut tidak ditemukan." << endl;
        beepOnError();
    }
    resetTextColor();
    this_thread::sleep_for(chrono::seconds(1));
}

void updateBook() {
    clearScreen();
    setTextColorBlue();
    cout << "Masukkan ID buku yang ingin diupdate: ";
    resetTextColor();
    int id;
    cin >> id;
    cin.ignore();
    for (auto& book : library) {
        if (book.id == id) {
            setTextColorBlue();
            cout << "Judul baru: ";
            resetTextColor();
            getline(cin, book.title);
            setTextColorBlue();
            cout << "Penulis baru: ";
            resetTextColor();
            getline(cin, book.author);
            setTextColorBlue();
            cout << "Buku berhasil diupdate!" << endl;
            resetTextColor();
            this_thread::sleep_for(chrono::seconds(1));
            return;
        }
    }
    beepOnError();
    setTextColorBlue();
    cout << "Buku dengan ID tersebut tidak ditemukan." << endl;
    beepOnError();
    resetTextColor();
    this_thread::sleep_for(chrono::seconds(1));
}

void sortBooks() {
    clearScreen();
    setTextColorBlue();
    cout << "Sortir berdasarkan:\n1. ID\n2. Judul\n3. Penulis\nPilihan: ";
    resetTextColor();
    int choice;
    cin >> choice;
    cin.ignore();
    switch (choice) {
        case 1:
            sort(library.begin(), library.end(), [](Book& a, Book& b) { return a.id < b.id; });
            break;
        case 2:
            sort(library.begin(), library.end(), [](Book& a, Book& b) { return a.title < b.title; });
            break;
        case 3:
            sort(library.begin(), library.end(), [](Book& a, Book& b) { return a.author < b.author; });
            break;
        default:
            beepOnError();
            setTextColorBlue();
            cout << "Pilihan tidak valid!" << endl;
            beepOnError();
            resetTextColor();
            this_thread::sleep_for(chrono::seconds(1));
            return;
    }
    setTextColorBlue();
    cout << "Buku berhasil disortir!" << endl;
    resetTextColor();
    this_thread::sleep_for(chrono::seconds(1));
}

void kritiksaran() {
    clearScreen();
    setTextColorBlue();
    delayPrint("kritik!", 50);
    cout << endl;
    delayPrint("saran!", 50);
    cout << endl;
    resetTextColor();
    cout << "Tekan Enter untuk kembali ke menu...";
    cin.get();
}

void saveToFile() {
    ofstream file("library.txt");
    for (const auto& book : library) {
        file << book.id << "," << book.title << "," << book.author << endl;
    }
    file.close();
}

void loadFromFile() {
    ifstream file("library.txt");
    if (!file.is_open()) return;
    string line;
    while (getline(file, line)) {
        Book book;
        size_t pos1 = line.find(',');
        size_t pos2 = line.find(',', pos1 + 1);
        book.id = stoi(line.substr(0, pos1));
        book.title = line.substr(pos1 + 1, pos2 - pos1 - 1);
        book.author = line.substr(pos2 + 1);
        library.push_back(book);
    }
    file.close();
}
