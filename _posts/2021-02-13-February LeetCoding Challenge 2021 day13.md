---
layout: post
title: Day13---Shortest Path in Binary Matrix
description: Java solution
image: assets/images/feb-challenge.png
---

# Day13---Shortest Path in Binary Matrix(medium)

## 题目

In an N by N square grid, each cell is either empty (0) or blocked (1).

A *clear path from top-left to bottom-right* has length `k` if and only if it is composed of cells `C_1, C_2, ..., C_k` such that:

- Adjacent cells `C_i` and `C_{i+1}` are connected 8-directionally (i.e., they are different and share an edge or corner)
- `C_1` is at location `(0, 0)` (i.e. has value `grid[0][0]`)
- `C_k` is at location `(N-1, N-1)` (i.e. has value `grid[N-1][N-1]`)
- If `C_i` is located at `(r, c)`, then `grid[r][c]` is empty (i.e. `grid[r][c] == 0`).

Return the length of the shortest such clear path from top-left to bottom-right. If such a path does not exist, return -1.

要点：

* 在此题中，与当前格子相连的8个格子（斜方向的也算）都可以被当作下一步的格子。
* 起点是（0，0），终点是（N-1，N-1）

## 分析

对于求Shortest Path的问题，常见思路是用BFS来做。因为相关题目处理起来稍微有些复杂，所以在此统一整理一下思路：

1. 首先我们需要有一个意识，就是**存在无法从起点到达终点的可能性**，这种可能性分两种情况：第一种是起点就不可走，第二种是有一圈闭合的“1”把起点的路给封死了。第一种意外的情况的解决思路其实是在后面，相当于是不能走visited数组值为1的位置，所以我们容易忽视掉这个问题。第二种情况比较好解决，当我们走遍所有能走到格子，此时队列为空，如果还没有走到终点直接返回就好了。所以从起点开始，首先要判断（0，0）处值是否为0，不是直接返回-1。
2. 然后我们需要把当前格子邻接的所有可走的格子入队列。这里有一个细节就是什么时候修改visited数组这个问题，为了避免把同样的格子重复入队列，应该入队的时候就修改visited数组。

## 代码

Java Solution：

```java
class Solution {
    public class Point{
        public int x;
        public int y;
        public int dist;
    }
    public int shortestPathBinaryMatrix(int[][] grid) {
        Queue<Point> queue = new LinkedList<Point>();
        int len = grid[0].length;
        int[][] visited = new int[len][len];
        if (grid[0][0] == 1){
            return -1;
        }
        Point start = new Point();
        start.x = 0;
        start.y = 0;
        start.dist = 1;
        int[] dir = {-1, 0, 1};
        queue.offer(start);
        visited[0][0] = 1;
        while(queue.isEmpty() == false){
            Point thisPoint = queue.poll();
            if(thisPoint.x == len - 1 && thisPoint.y == len - 1){
                return thisPoint.dist;
            }
            int x = thisPoint.x, y = thisPoint.y, dist = thisPoint.dist;
            for(int i: dir){
                for(int j: dir){
                    if ((i | j) == 0){
                        continue;
                    }
                    if ((x + i) < 0 || (x + i) == len){
                        continue;
                    }
                    if ((y + j) < 0 || (y + j) == len){
                        continue;
                    }
                    if (visited[x + i][y + j] == 1 || grid[x + i][y + j] == 1){
                        continue;
                    }
                    Point newPoint = new Point();
                    newPoint.x = x + i;
                    newPoint.y = y + j;
                    newPoint.dist = dist + 1;
                    queue.offer(newPoint);
                    visited[newPoint.x][newPoint.y] = 1;
                }
            }
        }
        return -1;
    }
}
```

## 优化

参考leetcode高赞解法：

```java
class Solution {
    public int shortestPathBinaryMatrix(int[][] grid) {
        if(grid[0][0] == 1) 
            return -1;
        int m = grid.length, n = grid[0].length;
        Queue<int[]> q = new LinkedList<>();  // 用int数组存坐标，不必定义一个class
        boolean[][] visited = new boolean[m][n];
		// 方向向量定义更细
        int[] dx = new int[]{1, 1, 0, -1, -1, -1, 0, 1};
        int[] dy = new int[]{0, 1, 1, 1, 0, -1, -1, -1};
        q.add(new int[]{0, 0});
        visited[0][0] = true;
        int level = 1;
        while(q.size() > 0) {
            for(int i = q.size(); i > 0; i--) {  // for循环中的q.size()固定，以此来达到分层次的作用
                int[] xy = q.poll();
                if(xy[0] == m - 1 && xy[1] == n - 1)
                    return level;
                for(int j = 0; j < 8; j++) { 
                    int nx = xy[0] + dx[j];
                    int ny = xy[1] + dy[j];
                    if(nx >= 0 && nx < m && ny >= 0 && ny < n && !visited[nx][ny] && grid[nx][ny] == 0) {
                        visited[nx][ny] = true;
                        q.add(new int[]{nx, ny});
                    }
                }
            }
            level++;
        }
        return -1;
    }
}
```

