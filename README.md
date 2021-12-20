# Number of Islands II
## https://leetcode.com/problems/number-of-islands-ii


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
