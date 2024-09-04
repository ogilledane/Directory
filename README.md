#include <iostream>
#include <direct.h>
#include <string>
using namespace std;

void ListFile();
void Directory();
void ChangeDir();

int main() {
    int choices;
    while (true) {
        cout << "      Main Menu:     \n";
        cout << "--------------------------------\n";
        cout << "1. To Display List of Files\n";
        cout << "2. To Create New Directory\n";
        cout << "3. To Change the Working Directory\n";
        cout << "4. Exit\n";
        cout << "Enter Number: ";
        cin >> choices;

        if(choices==1) {
            ListFile();
    }else if (choices==2){
    	Directory();
	}else if (choices==3){
		ChangeDir();
	}else if (choices==4){
		return 0;
	}else {
		cout << "INVALID NUMBER. Please try again.\n";
	}
    }
      return 0;  
    }

void ListFile() {
    int choices;
    cout << "     LIST FILE DETAIL: \n";
    cout << "--------------------------------------\n";
    cout << "1. List All Files\n";
    cout << "2. List of Extension Files\n";
    cout << "3. List of Name Wise\n";
    cout << "Enter Number: ";
    cin >> choices;

    switch (choices) {
        case 1:
            cout << "List of all Files\n";
            system("dir");
            break;
        case 2: {
            string ext;
            cout << "Enter file extension: ";
            cin >> ext;
            system(("dir *." + ext).c_str());
            break;
        }
        case 3: {
            string pattern;
            cout << "Enter file name pattern: ";
            cin >> pattern;
            system(("dir " + pattern).c_str());
            break;
        }
        default:
            cout << "INVALID NUMBER. Please try again.\n";
    }
}

void Directory() {
    string dir;
    cout << "Directory name: ";
    cin >> dir;

    if (_mkdir(dir.c_str()) == 0) {
        cout << "Directory created.\n";
    } else {
        cout << "Error directory. It may already exist or be invalid.\n";
    }
}

void ChangeDir() {
    int command;
    cout << "  CHANGE DIRECTORY MENU: \n";
    cout << "1. Move one step back.\n";
    cout << "2. Move to the root directory.\n";
    cout << "3. Move to a specific directory provided by the user.\n";
    cout << "Enter your choice: ";
    cin >> command;

    switch (command) {
        case 1:
            if (_chdir("..") == 0) {
                cout << "Moved to parent directory.\n";
            } else {
                cout << "Error moving to parent directory.\n";
            }
            break;
        case 2:
            if (_chdir("\\") == 0) {
                cout << "Moved to root directory.\n";
            } else {
                cout << "Error moving to root directory.\n";
            }
            break;
        case 3: {
            string dir;
            cout << "Directory name: ";
            cin >> dir;
            if (_chdir(dir.c_str()) == 0) {
                cout << "Directory changed successfully.\n";
            } else {
                cout << "Error changing directory. It may not exist.\n";
            }
            break;
        }
        default:
            cout << "SORRY INVALID, Try again.\n";
    }
}
