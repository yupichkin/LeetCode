# Course Schedule
https://leetcode.com/problems/course-schedule/

```C++ 
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        int i = 0;
        int colors[numCourses] = { }; // All entries set to zero;
        vector<vector<int>> graf(numCourses);
        buildGraf(numCourses, prerequisites, graf);
        
        while (i < numCourses) {
            if (!checkCourse(i, graf, colors))
                return false;
            i++;
        }
        return true;
    }
private:
    void buildGraf(int numCourses, vector<vector<int>>& prerequisites, vector<vector<int>>& graf) {
        int i = 0;
        int size = prerequisites.size();
        while (i < size) {
            graf[prerequisites[i][0]].push_back(prerequisites[i][1]);
            i++;
        }
    }
    bool checkCourse(int numCourse, vector<vector<int>>& graf, int* colors) {
        if (colors[numCourse] == 2) //point is black
            return true;
        if (colors[numCourse] == 1) //point is gray
            return false;
        colors[numCourse] = 1; //point was white and now gray
        for(auto prevCourse: graf[numCourse]) {
            if (!checkCourse(prevCourse, graf, colors))
                return false;
        }
        colors[numCourse] = 2;
        return true;
    }
 ```

 # Course Schedule II

https://leetcode.com/problems/course-schedule-ii/
```C++ 
private:
vector<int> path;
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> graf(numCourses);
        if (!canFinish(numCourses, prerequisites, graf))
            return {}; //return empry array;
        return path;
    }
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites, vector<vector<int>>& graf) {
        int i = 0;
        int colors[numCourses] = { }; // All entries set to zero; (set all colors to white)
        buildGraf(numCourses, prerequisites, graf);
        
        while (i < numCourses) {
            if (!checkCourse(i, graf, colors))
                return false;
            i++;
        }
        return true;
    }
    void buildGraf(int numCourses, vector<vector<int>>& prerequisites, vector<vector<int>>& graf) {
        int i = 0;
        while (i < prerequisites.size()) {
            graf[prerequisites[i][0]].push_back(prerequisites[i][1]);
            i++;
        }
    }
    bool checkCourse(int numCourse, vector<vector<int>>& graf, int* colors) {
        if (colors[numCourse] == 2) //point is black
            return true;
        if (colors[numCourse] == 1) //point is gray
            return false;
        colors[numCourse] = 1; //point was white and now gray
        for(auto prevCourse: graf[numCourse]) {
            if (!checkCourse(prevCourse, graf, colors))
                return false;
        }
        colors[numCourse] = 2;
        path.push_back(numCourse);
        return true;
    }
 ```
 
 # Alien Dictionary

https://www.lintcode.com/problem/alien-dictionary/description
```C++ 
class Solution {
 private:
  vector<int> alphabet;
  int colors[26] = {0};
  bool meetedWord[26] = {};

 public:
  string alienOrder(vector<string> &words) {
    string alienAlphabet;
    int j;
    vector<vector<int>> graph(26);
    for (auto word : words)
      for (auto letter : word) meetedWord[letter - 'a'] = true;

    for (int i = 0; i < words.size() - 1; i++) {
      j = 0;
      while (j < min(words[i].length(), words[i + 1].length()) &&
             words[i][j] == words[i + 1][j])
        j++;
      graph[words[i + 1][j] - 'a'].push_back(words[i][j] - 'a');
    }
    for (int i = 0; i < 26; i++) {
      if (meetedWord[i]) {
        if (!checkLetter(i, graph)) return {};
      }
    }
    alienAlphabet.resize(alphabet.size());
    for (int i = 0; i < alphabet.size(); i++) {
      alienAlphabet[i] = 'a' + (char)alphabet[i];
    }
    return alienAlphabet;
  }
  bool checkLetter(int letter, vector<vector<int>> graph) {
    if (colors[letter] == 2)  // point is black
      return true;
    if (colors[letter] == 1)  // point is gray
      return false;
    colors[letter] = 1;  // point was white and now gray
    if (checkNeighbours(letter, graph)) {
      colors[letter] = 2;
      alphabet.push_back(letter);
      return true;
    }
    return false;
  }
  bool checkNeighbours(int letter, vector<vector<int>> graph) {
    for (int i = 0; i != graph[letter].size(); i++) {
      if (!checkLetter(graph[letter][i], graph)) return false;
    }
    return true;
  }
};
 
 ```
