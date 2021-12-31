# Number of Islands II
## https://leetcode.com/problems/number-of-islands-ii

You are given an empty 2D binary grid grid of size m x n. The grid represents a map where 0's represent water and 1's represent land. Initially, all the cells of grid are water cells (i.e., all the cells are 0's).

We may perform an add land operation which turns the water at position into a land. You are given an array positions where positions[i] = [ri, ci] is the position (ri, ci) at which we should operate the ith operation.

Return an array of integers answer where answer[i] is the number of islands after turning the cell (ri, ci) into a land.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

![Number of islands II](example.JPG?raw=true)


# Implementation : Union Find
```java
class Solution {
    int islands = 0;
    
    public List<Integer> numIslands2(int m, int n, int[][] positions) {
        List<Integer> result = new ArrayList<>();
        
        int[] parents = new int[m*n];
        Arrays.fill(parents, -1);
        int[] rank = new int[m*n];
        
        int[][] directions = {{0,1}, {0,-1}, {-1, 0}, {1,0}};
        
        for(int[] position : positions) {
            int cellNumber = position[0] * n + position[1];
            if(parents[cellNumber] != -1) {
                result.add(islands);
                continue;
            }
            islands++;
            parents[cellNumber] = cellNumber;
            rank[cellNumber] = 1;
            for(int[] direction : directions) {
                int x = position[0] + direction[0];
                int y = position[1] + direction[1];
                int neighborCellNumber = x * n + y;
                
                if(x >= 0 && x < m && y >= 0 && y < n && parents[neighborCellNumber] != -1) {
                    union(cellNumber, neighborCellNumber, parents, rank);
                }
            }
            result.add(islands);
        }
        return result;
    }
    
    private int find(int cellNumber, int[] parents) {
        if(parents[cellNumber] == cellNumber)
            return cellNumber;
        parents[cellNumber] = find(parents[cellNumber], parents);
        return parents[cellNumber];
    }
    
    private void union(int cellNumber1, int cellNumber2, int[] parents, int[] rank) {
        int leader1 = find(cellNumber1, parents);
        int leader2 = find(cellNumber2, parents);
        if(leader1 != leader2) {
            if(rank[leader1] > rank[leader2]) {
                parents[leader2] = leader1;
            } else if(rank[leader2] > rank[leader1]) {
                parents[leader1] = leader2;
            } else {
                parents[leader2] = leader1;
                rank[leader1]++;
            } 
            islands--;
        }
    }
}
```

# References :
https://www.youtube.com/watch?v=PcYCkgYu6uc
