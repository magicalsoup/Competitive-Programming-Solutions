This problem asks us to select a range of out N ranges that yields the largest intersection with another given range. 
It is best if we first convert the time to some standard representation. You may use time processing features 
if your programming language supports it, or you may simply convert the time to the number of minutes after midnight.

For each range, you can then calculate the intersection with Bob's shift, and take the shift with the largest intersection. 
Note that each of Alice's shifts may intersect (or not intersect) with Bob's shift in one of the following 6 ways:


```
Alice:  a1-------a2               Overlap: a2 - b1
Bob:        b1--------b2


Alice:           a1-------a2      Overlap: b2 - a1
Bob:        b1--------b2


Alice:  a1----------------a2      Overlap: b2 - b1
Bob:        b1--------b2


Alice:      a1--------a2          Overlap: a2 - a1
Bob:    b1----------------b2


Alice:  a1-----a2                 Overlap: 0
Bob:               b1-----b2


Alice:             a1-----a2      Overlap: 0
Bob:    b1-----b2

```
We calculate the overlap of Bob's shift with each of Alice's shifts, either by checking each of the cases above 
(as shown in the implementation below), or using the formula:

```
intersection = (a2 − a1) + (b2 − b1) − (max(a2,b2) − min(a1,b1))
```

(length of Alice's shift + length of Bob's shift - union of both shifts). 
Whenever we have an overlap that is strictly larger than the current best answer, we update the best answer. 
This ensures that we select the earliest best answer in the input. In the end, if we have not found an overlap, 
then we tell Alice to call in sick.

```cpp

#include <iostream>
#include <sstream>
using namespace std;

/**
 * converts a time (e.g. "08:30AM") to
 * the number of minutes after midnight
 */
int to_minutes(string s) {
  int H, M;
  char colon;
  string AMPM;
  stringstream ss;
  ss << s;
  ss >> H >> colon >> M >> AMPM;
  int minutes = H * 60 + M;
  if (AMPM[0] == 'P' && H != 12)
    minutes += 12 * 60;
  return minutes;
}

/**
 * calculates the intersection distance
 * of the ranges [a1, a2] and [b1, b2]
 */
int overlap(int a1, int a2, int b1, int b2) {
  if (a1 <= b1 && b1 <= a2 && a2 <= b2)
    return a2 - b1;
  if (b1 <= a1 && a1 <= b2 && b2 <= a2)
    return b2 - a1;
  if (a1 <= b1 && b2 <= a2) return b2 - b1;
  if (b1 <= a1 && a2 <= b2) return a2 - a1;
  return 0;
}

int N;
int T1, T2, L[100], H[100];
string lo, hi, inp[100], ans;

int main() {
  cin >> lo >> hi;
  T1 = to_minutes(lo);
  T2 = to_minutes(hi);
  cin >> N;
  for (int i = 0; i < N; i++) {
    cin >> lo >> hi;
    inp[i] = lo + " " + hi;
    L[i] = to_minutes(lo);
    H[i] = to_minutes(hi);
  }
  int max_overlap = 0;
  for (int i = 0; i < N; i++) {
    int ol = overlap(L[i], H[i], T1, T2);
    if (ol > max_overlap) {
      max_overlap = ol;
      ans = inp[i];
    }
  }
  if (max_overlap > 0) {
    cout << ans << endl;
  } else {
    cout << "Call in a sick day." << endl;
  }
  return 0;
}
```
