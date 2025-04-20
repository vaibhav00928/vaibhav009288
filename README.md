#include <iostream>
#include <fstream>
#include <vector>
#include <string>
#include <filesystem>

namespace fs = std::filesystem;

const std::string notesDir = "pdf_notes";
const std::string databaseFile = "notes_list.txt";

void loadNotes(std::vector<std::string>& notes) {
    std::ifstream infile(databaseFile);
    std::string line;
    while (getline(infile, line)) {
        notes.push_back(line);
    }
}

void saveNotes(const std::vector<std::string>& notes) {
    std::ofstream outfile(databaseFile);
    for (const auto& note : notes) {
        outfile << note << std::endl;
    }
}

void listNotes(const std::vector<std::string>& notes) {
    std::cout << "\nAvailable PDF Notes:\n";
    if (notes.empty()) {
        std::cout << "No notes available.\n";
    } else {
        for (size_t i = 0; i < notes.size(); ++i) {
            std::cout << i + 1 << ". " << notes[i] << std::endl;
        }
    }
}

void addNote(std::vector<std::string>& notes) {
    std::string pdfName;
    std::cout << "\nEnter the filename of the PDF to add (must be in 'pdf_notes' folder): ";
    std::cin >> pdfName;

    std::string filePath = notesDir + "/" + pdfName;
    if (fs::exists(filePath)) {
        notes.push_back(pdfName);
        saveNotes(notes);
        std::cout << "PDF added successfully.\n";
    } else {
        std::cout << "File does not exist in '" << notesDir << "' directory.\n";
    }
}

void createNotesFolder() {
    if (!fs::exists(notesDir)) {
        fs::create_directory(notesDir);
    }
}

int main() {
    createNotesFolder();

    std::vector<std::string> notes;
    loadNotes(notes);

    int choice;

    do {
        std::cout << "\n=== PDF Notes Manager ===\n";
        std::cout << "1. List all notes\n";
        std::cout << "2. Add a new note\n";
        std::cout << "3. Exit\n";
        std::cout << "Choose an option: ";
        std::cin >> choice;

        switch (choice) {
            case 1:
                listNotes(notes);
                break;
            case 2:
                addNote(notes);
                break;
            case 3:
                std::cout << "Exiting program...\n";
                break;
            default:
                std::cout << "Invalid option. Try again.\n";
        }

    } while (choice != 3);

    return 0;
}
