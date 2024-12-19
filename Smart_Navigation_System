///////////////////////////////////////////////////////////////////////////////////////
// Project Name : Campus Navigation System
/////////////////////////////////////////////////////////////////////////////////////

#include <iostream>
#include <map>
#include <queue>
#include <climits>
#include <list>
#include <algorithm>
#include <string>

using namespace std;
struct Edge {                                                        // Structure to represent a weighted edge in a linked list
    string destination;
    int weight;
    Edge* next;

    Edge(string dest, int w) : destination(dest), weight(w), next(nullptr) {
	
	
	}
};

class LinkedList {                                                      // Linked List class for managing edges of each building
private:
    Edge* head;

public:
    LinkedList() : head(nullptr) {
	
	}

    void addEdge(string dest, int weight) {
        Edge* newEdge = new Edge(dest, weight);
        if (!head) {
            head = newEdge;
        } else {
            Edge* temp = head;
            while (temp->next)
                temp = temp->next;
            temp->next = newEdge;
        }
    }

    void displayEdges() {
        Edge* temp = head;
        while (temp) {
            cout << "(" << temp->destination << ", " << temp->weight << " meters) ";
            temp = temp->next;
        }
    }

    void deleteEdges() {
        while (head) {
            Edge* temp = head;
            head = head->next;
            delete temp;
        }
    }

    Edge* getHead() { 
	return head;
	 }

    void removeConnection(string dest) {
        Edge* current = head;
        Edge* prev = nullptr;

        while (current) {
            if (current->destination == dest) {
                if (prev)
                    prev->next = current->next;
                else
                    head = current->next;
                delete current;
                return;
            }
            prev = current;
            current = current->next;
        }
    }
};

class CampusGraph {                                    // Campus Graph class
private:
    map<string, LinkedList> adjList;
    list<string> buildings;

public:
    CampusGraph() {
        initializePresetMap();
    }

    void initializePresetMap() {
        addConnection("Hostel-1", "Academic Block", 500);
        addConnection("Hostel-1", "Central Library", 700);
        addConnection("Hostel-2", "Academic Block", 300);
        addConnection("Hostel-2", "Central Library", 500);
        addConnection("Academic Block", "Cafe", 200);
        addConnection("Cafe", "Sports Complex", 400);
        addConnection("Sports Complex", "Central Library", 600);
        addConnection("Hostel-3", "Sports Complex", 800);
        addConnection("Hostel-3", "Hostel-4", 400);
        addConnection("Hostel-4", "Academic Block", 900);
        addConnection("Faculty Block", "Academic Block", 300);
        addConnection("Faculty Block", "Central Library", 400);
    }

    void addConnection(string u, string v, int w) {
        adjList[u].addEdge(v, w);
        adjList[v].addEdge(u, w);

        if (find(buildings.begin(), buildings.end(), u) == buildings.end())
            buildings.push_back(u);
        if (find(buildings.begin(), buildings.end(), v) == buildings.end())
            buildings.push_back(v);
    }

    void displayMap() {                               //Dispaly Map
        cout << "\nCampus Map (Building Connections):\n";
        for (auto& node : adjList) {
            cout << node.first << " -> ";
            node.second.displayEdges();
            cout << endl;
        }
    }

    void displaySortedBuildings() {                    //Sorted Building
        buildings.sort();
        cout << "\nList of Buildings (Sorted Alphabetically):\n";
        for (auto& building : buildings) {
            cout << building << endl;
        }
    }

    void searchBuilding(string name) {                                                 //Search Building
        if (find(buildings.begin(), buildings.end(), name) != buildings.end()) {
            cout << "Building '" << name << "' exists in the campus.\n";
        } else {
            cout << "Building '" << name << "' not found!\n";
        }
    }
 
    void isolateBuilding(string name) {                                               //Isolate Building
        if (adjList.find(name) == adjList.end()) {
            cout << "Building '" << name << "' does not exist!\n";
            return;
        }

        for (auto& node : adjList) {
            node.second.removeConnection(name);
        }

        adjList[name].deleteEdges();
        cout << "Building '" << name << "' has been isolated (connections removed).\n";
    }

    void BFS(string start) {                                      //Breath First Search (BFS)  Traversal
        if (adjList.find(start) == adjList.end()) {
            cout << "Starting building '" << start << "' does not exist!\n";
            return;
        }

        queue<string> q;
        map<string, bool> visited;

        cout << "\nBFS Traversal from " << start << ":\n";
        q.push(start);
        visited[start] = true;

        while (!q.empty()) {
            string current = q.front();
            q.pop();
            cout << current << " ";

            Edge* temp = adjList[current].getHead();
            while (temp) {
                if (!visited[temp->destination]) {
                    visited[temp->destination] = true;
                    q.push(temp->destination);
                }
                temp = temp->next;
            }
        }
        cout << endl;
    }

    void dijkstra(string start, string destination) {                //Dijkstra Algo
        if (adjList.find(start) == adjList.end() || adjList.find(destination) == adjList.end()) {
            cout << "One or both buildings do not exist!\n";
            return;
        }

        map<string, int> dist;
        map<string, string> parent;
        priority_queue<pair<int, string>, vector<pair<int, string>>, greater<pair<int, string>>> pq;

        for (auto& node : adjList)
            dist[node.first] = INT_MAX;

        dist[start] = 0;
        pq.push({0, start});

        while (!pq.empty()) {
            string current = pq.top().second;
            int currentDist = pq.top().first;
            pq.pop();

            Edge* temp = adjList[current].getHead();
            while (temp) {
                int newDist = currentDist + temp->weight;
                if (newDist < dist[temp->destination]) {
                    dist[temp->destination] = newDist;
                    parent[temp->destination] = current;
                    pq.push({newDist, temp->destination});
                }
                temp = temp->next;
            }
        }

        cout << "\nShortest path from " << start << " to " << destination << " is: "
             << dist[destination] << " meters\n";

        vector<string> path;
        string temp = destination;
        while (temp != start) {
            path.push_back(temp);
            temp = parent[temp];
        }
        path.push_back(start);

        cout << "Path: ";
        for (int i = path.size() - 1; i >= 0; i--) {
            cout << path[i];
            if (i != 0) cout << " -> ";
        }
        cout << endl;
    }
};
 
int main() {          //Main Fnction
	
	
    CampusGraph campus;
    cout << "Welcome to the Smart GIKI Campus Navigation System!\n";

    while (true) {
        cout << "\nMenu:\n";
        cout << "1. Add Connection\n2. Display Campus Map\n3. Search Building\n4. BFS Traversal\n";
        cout << "5. Find Shortest Path\n6. Display Sorted Buildings\n7. Isolate Building\n";
        cout << "8. Exit\n";
        cout << "Enter your choice: ";
        int choice;
        cin >> choice;
        cin.ignore(); // Clear input buffer for getline

        if (choice == 1) {
            string u, v;
            int w;
            cout << "Enter building 1: ";
            getline(cin, u);
            cout << "Enter building 2: ";
            getline(cin, v);
            cout << "Enter distance: ";
            cin >> w;
            campus.addConnection(u, v, w);
        } else if (choice == 2) {
            campus.displayMap();
        } else if (choice == 3) {
            string name;
            cout << "Enter building name: ";
            getline(cin, name);
            campus.searchBuilding(name);
        } else if (choice == 4) {
            string start;
            cout << "Enter starting building: ";
            getline(cin, start);
            campus.BFS(start);
        } else if (choice == 5) {
            string start, dest;
            cout << "Enter starting building: ";
            getline(cin, start);
            cout << "Enter destination: ";
            getline(cin, dest);
            campus.dijkstra(start, dest);
        } else if (choice == 6) {
            campus.displaySortedBuildings();
        } else if (choice == 7) {
            string name;
            cout << "Enter building to isolate: ";
            getline(cin, name);
            campus.isolateBuilding(name);
        } else if (choice == 8) {
            cout << "Exiting...\n";
            break;
        } else {
            cout << "Invalid choice! Try again.\n";
        }
    }

    return 0;                    //Termination
}
