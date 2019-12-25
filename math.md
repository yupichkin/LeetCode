# Mathematics
+ [Fibonachi numbers](#fibonacci-number)
  + [Recursively](#recursively)
  + [Recursively, but with using memory](#recursively, but with using memory)
  + [Iteratively](#iteratively)

# Fibonachi numbers

3 different algorithm for finding Fibonachi numbers

1) Recursively
```C++ 
  int fib(int N) {
        if (N < 2)
            return N;
        int numbers[N];
        return fib(N - 1) + fib(N - 2);
   }
 ```
 
 2) Recursively, but with using memory
 ```C++ 
 int fib(int N) {
        if (N < 2)
            return N;
        int fibNumbers[N + 1] = {0,1};
        int i = N - 1; 
        while (i) {
            fibNumbers[i + 1] = -1;
            i--;
        }
        return Fib(N, fibNumbers);
    }
    int Fib(int N, int* fibNumbers) {
        if (fibNumbers[N] < 0)
            fibNumbers[N] = Fib(N - 1, fibNumbers) + Fib(N - 2, fibNumbers);
        return fibNumbers[N];
    }
```

3) Itteratively
```C++ 
  int fib(int N) {
       if(N < 2)
           return N;
        int prev = 0, next = 1;
        N--;
        while (N) {
            next = next + prev; //set to "next" - next fibonachi number
            prev = next - prev; //set to "prev" - set previous number before new "next", but next number after "prev"
            N--;
        }
        return next;
    }
```
