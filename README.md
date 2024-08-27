#include <iostream>
#include <filesystem>
#include <string>

using namespace std;

void showCurrentDirectory(const string& currentDir) {
    cout << "Current Directory: " << currentDir << filesystem::current_path().string() << endl;
}

void listAllFiles(const string& dirPath) {
    cout << "Listing all files in directory: " << endl;
    int fileCount = 0;
    cout << "\n";

    for (const auto& entry : filesystem::directory_iterator(dirPath)) {
        cout << entry.path().filename().string() << endl;
        fileCount++;
    }

    cout << "\nTotal Files: " << fileCount << "\n";
    cout << "Press Enter to continue...";
    cin.ignore();
}

void listFilesByExtension(const string& dirPath, const string& extension) {
    cout << "Listing all files with extension " << extension << " in directory: " << dirPath << endl;
    int fileCount = 0;

    for (const auto& entry : filesystem::directory_iterator(dirPath)) {
        if (entry.path().extension() == extension) {
            cout << entry.path().filename().string() << endl;
            fileCount++;
        }
    }

    cout << "\nTotal Files: " << fileCount << "\n";
    cout << "Press Enter to continue...";
    cin.ignore();
}

void listFilesByPattern(const string& dirPath, const string& pattern) {
    cout << "Listing all files matching pattern " << pattern << " in directory: " << dirPath << endl;
    int fileCount = 0;

    for (const auto& entry : filesystem::directory_iterator(dirPath)) {
        if (entry.path().filename().string().find(pattern) != string::npos) {
            cout << entry.path().filename().string() << endl;
            fileCount++;
        }
    }

    cout << "\nTotal Files: " << fileCount << "\n";
    cout << "Press Enter to continue...";
    cin.ignore();
}

void createDirectory(const string& dirPath) {
    if (filesystem::create_directory(dirPath)) {
        cout << "Directory created successfully: " << dirPath << endl;
    } else {
        cout << "Error: Could not create directory or it already exists.\n";
    }
    cout << "Press Enter to continue...";
    cin.ignore();
}

bool forwardDirectory(string& currentDir) {
    cout << "Enter directory path: ";
    string newDir;
    getline(cin, newDir);

    if (filesystem::exists(newDir) && filesystem::is_directory(newDir)) {
        currentDir = newDir;
        return true;
    } else {
        cout << "Invalid directory path.\n";
        return false;
    }
}

void changeDirectory(string& currentDir) {
    int choice;
    cout << "Change Directory Menu\n";
    cout << "-----------------------------\n";
    cout << "1. Step by Step backward\n";
    cout << "2. Go to the root directory\n";
    cout << "3. Forward directory\n";
    cout << "Enter choice: ";
    cin >> choice;
    cin.ignore();

    if (choice == 1) {
        currentDir = filesystem::path(currentDir).parent_path().string();
    } else if (choice == 2) {
        currentDir = "/";
    } else if (choice == 3) {
        forwardDirectory(currentDir);
    } else {
        cout << "Invalid choice.\n";
    }

    showCurrentDirectory(currentDir);
    cout << "Press Enter to continue...";
    cin.ignore();
}

void showMainMenu() {
    string currentDir = filesystem::current_path().string();
    int choice;

    do {
        cout << "Main Menu\n";
        cout << "-----------------------------\n";
        cout << "1. To display list of files\n";
        cout << "2. To Create a new directory\n";
        cout << "3. To Change the working directory\n";
        cout << "4. Exit\n";
        cout << "Enter choice: ";
        cin >> choice;
        cin.ignore();

        if (choice == 1) {
            int listChoice;
            cout << "List Files Menu\n";
            cout << "-----------------------------\n";
            cout << "1. List all files\n";
            cout << "2. List files by extension\n";
            cout << "3. List files by pattern\n";
            cout << "Enter choice: ";
            cin >> listChoice;
            cin.ignore();

            if (listChoice == 1) {
                listAllFiles(currentDir);
            } else if (listChoice == 2) {
                string extension;
                cout << "Enter file extension (e.g., .txt): ";
                cin >> extension;
                cin.ignore();
                listFilesByExtension(currentDir, extension);
            } else if (listChoice == 3) {
                string pattern;
                cout << "Enter file pattern (e.g., file): ";
                cin >> pattern;
                cin.ignore();
                listFilesByPattern(currentDir, pattern);
            } else {
                cout << "Invalid choice.\n";
            }

        } else if (choice == 2) {
            cout << "Enter the directory name to create: ";
            string dirName;
            getline(cin, dirName);
            createDirectory(currentDir + "/" + dirName);

        } else if (choice == 3) {
            changeDirectory(currentDir);

        } else if (choice == 4) {
            cout << "Exiting program.\n";

        } else {
            cout << "Invalid choice.\n";
        }

    } while (choice != 4);
}

int main() {
    showMainMenu();
    return 0;
}
