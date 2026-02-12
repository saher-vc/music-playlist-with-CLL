# music-playlist-with-CLL
#c++ #DSA #CLL its a playlist with circular linked list where user can add the song, delete by searching name, search the by singer, play previous and next song according to his wish and dsiplay it as he wants
#include <iostream>
#include <string>
using namespace std;

class Node {
public:
    int song_id;
    string name;
    string singer;
    float duration;   // in minutes
    string genre;

    Node* next;
    Node* prev;

    Node(int id, string n, string s, float d, string g) {
        song_id = id;
        name = n;
        singer = s;
        duration = d;
        genre = g;
        next = prev = NULL;
    }
};

class Playlist {
private:
    Node* head;
    Node* tail;
    Node* current;
    int size;

public:
    Playlist() {
        head = tail = current = NULL;
        size = 0;
    }

    // ================= INSERT =================

    void addAtStart(Node* newNode) {
        if (head == NULL) {
            head = tail = current = newNode;
            newNode->next = newNode->prev = newNode;
        }
        else {
            newNode->next = head;
            newNode->prev = tail;

            head->prev = newNode;
            tail->next = newNode;

            head = newNode;
        }
        size++;
        cout << "Song added at start.\n";
    }

    void addAtEnd(Node* newNode) {
        if (head == NULL) {
            head = tail = current = newNode;
            newNode->next = newNode->prev = newNode;
        }
        else {
            newNode->next = head;
            newNode->prev = tail;

            tail->next = newNode;
            head->prev = newNode;

            tail = newNode;
        }
        size++;
        cout << "Song added at end.\n";
    }

    // ================= DELETE =================

    void deleteFromStart() {
        if (head == NULL) {
            cout << "Playlist is empty.\n";
            return;
        }

        if (head == tail) {
            delete head;
            head = tail = current = NULL;
        }
        else {
            Node* temp = head;
            head = head->next;

            head->prev = tail;
            tail->next = head;

            if (current == temp)
                current = head;

            delete temp;
        }

        size--;
        cout << "Deleted from start.\n";
    }

    void deleteFromEnd() {
        if (tail == NULL) {
            cout << "Playlist is empty.\n";
            return;
        }

        if (head == tail) {
            delete tail;
            head = tail = current = NULL;
        }
        else {
            Node* temp = tail;
            tail = tail->prev;

            tail->next = head;
            head->prev = tail;

            if (current == temp)
                current = head;

            delete temp;
        }

        size--;
        cout << "Deleted from end.\n";
    }

    void deleteByID(int id) {
        if (head == NULL) {
            cout << "Playlist is empty.\n";
            return;
        }

        Node* temp = head;

        do {
            if (temp->song_id == id) {

                if (head == tail) {
                    delete temp;
                    head = tail = current = NULL;
                }
                else {
                    temp->prev->next = temp->next;
                    temp->next->prev = temp->prev;

                    if (temp == head)
                        head = temp->next;

                    if (temp == tail)
                        tail = temp->prev;

                    if (temp == current)
                        current = temp->next;

                    delete temp;
                }

                size--;
                cout << "Song deleted successfully.\n";
                return;
            }

            temp = temp->next;

        } while (temp != head);

        cout << "Song not found.\n";
    }

    // ================= PLAY =================

    void playCurrent() {
        if (current == NULL) {
            cout << "Playlist is empty.\n";
            return;
        }
        cout << "Now Playing: " << current->name << endl;
    }

    void playNext() {
        if (current == NULL) return;

        current = current->next;
        cout << "Now Playing: " << current->name << endl;
    }

    void playPrevious() {
        if (current == NULL) return;

        current = current->prev;
        cout << "Now Playing: " << current->name << endl;
    }

    // ================= DISPLAY =================

    void display() {
        if (head == NULL) {
            cout << "Playlist is empty.\n";
            return;
        }

        Node* temp = head;

        cout << "\n--- Playlist ---\n";
        do {
            cout << temp->song_id << " | "
                 << temp->name << " | "
                 << temp->singer << " | "
                 << temp->duration << " min | "
                 << temp->genre << endl;

            temp = temp->next;

        } while (temp != head);
    }

    // ================= FILTER =================

    void longerThan3Min() {
        if (head == NULL) return;

        Node* temp = head;
        cout << "\nSongs longer than 3 minutes:\n";

        do {
            if (temp->duration > 3.0) {
                cout << temp->name << " (" << temp->duration << " min)\n";
            }
            temp = temp->next;
        } while (temp != head);
    }
};

// ================= MAIN =================

int main() {
    Playlist music;
    int choice;

    do {
        cout << "\n1. Add at Start\n";
        cout << "2. Add at End\n";
        cout << "3. Delete from Start\n";
        cout << "4. Delete from End\n";
        cout << "5. Delete by Song ID\n";
        cout << "6. Play Current\n";
        cout << "7. Play Next\n";
        cout << "8. Play Previous\n";
        cout << "9. Display Playlist\n";
        cout << "10. Songs Longer Than 3 Minutes\n";
        cout << "11. Exit\n";
        cout << "Enter choice: ";
        cin >> choice;

        if (choice == 1 || choice == 2) {
            int id;
            string name, singer, genre;
            float duration;

            cout << "Enter Song ID: ";
            cin >> id;
            cout << "Enter Song Name: ";
            cin >> name;
            cout << "Enter Singer: ";
            cin >> singer;
            cout << "Enter Duration (minutes): ";
            cin >> duration;
            cout << "Enter Genre: ";
            cin >> genre;

            Node* newNode = new Node(id, name, singer, duration, genre);

            if (choice == 1)
                music.addAtStart(newNode);
            else
                music.addAtEnd(newNode);
        }
        else if (choice == 3)
            music.deleteFromStart();
        else if (choice == 4)
            music.deleteFromEnd();
        else if (choice == 5) {
            int id;
            cout << "Enter Song ID to delete: ";
            cin >> id;
            music.deleteByID(id);
        }
        else if (choice == 6)
            music.playCurrent();
        else if (choice == 7)
            music.playNext();
        else if (choice == 8)
            music.playPrevious();
        else if (choice == 9)
            music.display();
        else if (choice == 10)
            music.longerThan3Min();

    } while (choice != 11);

    return 0;
}
