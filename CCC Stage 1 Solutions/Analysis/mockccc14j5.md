If there is no time travelling option, this would be a standard BFS-on-a-grid question. 
However, after introducing the 3rd dimension of time, we simply have to add one more variable to our state. 
At first it may look like BFS on a cube, but this is an incorrect assumption, 
since we need not travel to adjacent times. At every state, we can either travel to the 4 adjacent cells on the grid, or 
travel to the other T−1 time settings. Each of these moves will add 1 to the total "distance" travelled in 3D. 
When we reach the 'B' square, we print our answer and stop the program. The maximum number of states to visit is O(T×R×C), 
which is no greater than 10×100×100 = 100 000, and should easily pass in time.

```cpp

#include <iostream>
#include <queue>
using namespace std;

struct state { int t, r, c, d; };

int R, C, T, sr, sc;
char map[10][101][101];
bool vis[10][101][101] = {0};

int main() {
  cin >> R >> C >> T;
  for (int t = 0; t < T; t++)
    for (int r = 0; r < R; r++)
      for (int c = 0; c < C; c++) {
        cin >> map[t][r][c];
        if (map[t][r][c] == 'A') {
          sr = r;
          sc = c;
        }
      }
  int r, c, t, d;
  queue<state> Q;
  Q.push((state){0, sr, sc, 0});

  while (!Q.empty()) {
    t = Q.front().t;
    r = Q.front().r;
    c = Q.front().c;
    d = Q.front().d;
    Q.pop();

    if (r < 0 || r >= R || c < 0 || c >= C)
      continue;
    if (map[t][r][c] == 'X' || vis[t][r][c])
      continue;
    if (map[t][r][c] == 'B') {
      cout << d << endl;
      return 0;
    }
    vis[t][r][c] = true;

    Q.push((state){t, r-1, c, d+1});
    Q.push((state){t, r+1, c, d+1});
    Q.push((state){t, r, c-1, d+1});
    Q.push((state){t, r, c+1, d+1});
    for (int tt = 0; tt < T; tt++) {
      if (tt != t)
        Q.push((state){tt, r, c, d+1});
    }
  }
  cout << -1 << endl;
  return 0;
}
```
