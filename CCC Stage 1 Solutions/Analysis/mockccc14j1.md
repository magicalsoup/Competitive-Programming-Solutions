This question requires simple math and an if/else if/else statement. 
First we see if the value of A is greater than R, in which case Bob overdoses on day 1. 
Then we see if half of A plus B is greater than R. If so, Bob overdoses on day 2. If neither is the case, 
then output that he never overdoses. The only thing to be careful with is when we divide A by 2, we must compare 
the real number result to R, not the result of integer division.

```cpp
#include <iostream>
using namespace std;

int A, B, R;

int main() {
  cin >> A >> B >> R;
  if (A > R) {
    cout << "Bob overdoses on day 1." << endl;
  } else if ((A*0.5 + B) > R) {
    cout << "Bob overdoses on day 2." << endl;
  } else {
    cout << "Bob never overdoses." << endl;
  }
  return 0;
}
```
