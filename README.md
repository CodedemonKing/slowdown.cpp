#include <algorithm>
#include <fstream>
#include <iostream>
#include <queue>
#include <vector>
using namespace std;
int N;
priority_queue<int, vector<int>, greater<int>> TimeEvents;
priority_queue<int, vector<int>, greater<int>> DistanceEvents;
double currentDistance, currentTime;
int speedValue;
int main(){
  // freopen("slowdown.in", "r", stdin);
  // freopen("slowdown.out", "w", stdout);
  
  cin >> N;
  for (int i = 0; i < N; i++) {
    char input1;
    int input2;
    cin >> input1 >> input2;
    if (input1 == 'T') {
      TimeEvents.push(input2);
    }
    else {
      DistanceEvents.push(input2);
    }
  }
  DistanceEvents.push(1000);
  currentTime = currentDistance = 0.0;
  speedValue = 1;
  while (!TimeEvents.empty() || !DistanceEvents.empty()) {
    bool isNextTime = false;
    if (DistanceEvents.empty()){
      isNextTime = true;
    }
    else if (!DistanceEvents.empty() && !TimeEvents.empty()) {
      if (TimeEvents.top() < (currentTime + (DistanceEvents.top() - currentDistance) * speedValue)){
        isNextTime = true;
      }
    }
    if (isNextTime) { 
      currentDistance += (TimeEvents.top() - currentTime) / (speedValue + 0.0);
      currentTime = TimeEvents.top();
      TimeEvents.pop();
    }
    else {
      currentTime += (DistanceEvents.top() - currentDistance) * speedValue;
      currentDistance = DistanceEvents.top();
      DistanceEvents.pop();
    }
    speedValue++;
  }
  
  int currentTime2 = (int)currentTime;
  double fraction = currentTime - currentTime2;
  if (fraction < 0.5) {
    cout << currentTime << "\n";
  }
  else {
    cout << currentTime + 1 << endl;
  }
  return 0;
}
