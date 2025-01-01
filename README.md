# taskTracker
Sample solution for the task-tracker challenge from roadmap.sh. Just a simple CLI based task Tracking program in C++.


#include <iostream>
#include <string>
#include <vector>
#include <fstream>
using namespace std;

struct Task{
  string description;
  string status;    //"To_do , In_progress , Done"
};


void AddTask(vector<Task>& tasks) {
  string description;
     cout << "Please enter the task description. ";
  cin.ignore();
  getline(cin, description);

     Task newTask;
     newTask.description = description;
     newTask.status = "To_do";
     tasks.push_back(newTask);
    cout << "Task added succesfully! " << endl;
}


void listTasks(const vector<Task>& tasks){
  if(tasks.empty()) {
     cout << "No tasks available. " << endl;
     return;
  }
  for (size_t i = 0; i < tasks.size(); ++i) {
    cout << i + 1 << ". " << tasks[i].description << " [" << tasks[i].status << "]" << endl;
  }
}

void markTaskDone(vector<Task>& tasks, int index){
  if(index <= 0 || index > tasks.size()){
    cout << "Invalid task number. " << endl;
    return;
  }
  tasks[index - 1].status = "Done. ";
     cout << "Task marked as Done! " << endl;
}

void loadTasks(vector<Task>& tasks, const string& filename){
  ifstream infile(filename);
  if (!infile){
    cout << "No previous task file found. Starting new. " << endl;
    return;
  }

  string description, status;
  while (getline(infile, description)){
    getline(infile, status);
    Task task;
    task.description = description;
    task.status = status;
    tasks.push_back(task);
  }
  infile.close();
}

void saveTasks(const vector<Task>& tasks, const string& filename){
  ofstream outfile(filename);
  for (const auto& task : tasks){
    outfile << task.description << endl;
    outfile << task.status << endl;
  }
  outfile.close();
}


int main(){
  vector<Task> tasks;
  const string filename = "tasks.txt";

  loadTasks(tasks, filename);
  string command;
  while (true){
    cout << "Enter prompt (add/list/done/exit): ";
    cin >> command;

     if (command == "add"){
      AddTask(tasks);
     } else if (command == "list"){
      listTasks(tasks);
     } else if (command == "done"){
      int index;
       cout << "Enter task number: ";
       cin >> index;
      markTaskDone(tasks, index);
     } else if (command == "exit"){
      saveTasks(tasks, filename);
      cout << "Tasks saved, exiting. " << endl;
      break;
     } else {
       cout << "Unknown prompt. Try again. " << endl;
     }
  }
  return 0;
}
