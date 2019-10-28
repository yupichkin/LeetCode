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
        if (checkNeighbours(numCourse, graf, colors)) { 
            colors[numCourse] = 2;
             return true;
        }
        return false;
    }
    bool checkNeighbours(int numCourse, vector<vector<int>>& graf, int* colors) {
        int i = 0;
        while (i < graf[numCourse].size()) {
            if (!checkCourse(graf[numCourse][i], graf, colors))
                return false;
            i++;
        }
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
        if (checkNeighbours(numCourse, graf, colors)) {
            colors[numCourse] = 2;
            path.push_back(numCourse);
            return true;
        }
        return false;
    }
    bool checkNeighbours(int numCourse, vector<vector<int>>& graf, int* colors) {
        int i = 0;
        while (i < graf[numCourse].size()) {
            if (!checkCourse(graf[numCourse][i], graf, colors))
                return false;
            i++;
        }
        return true;
    }
 ```
