### Link
https://www.1point3acres.com/bbs/thread-1135155-1-1.html

### Question
给定一个二维字符数组 `city`，其中 ' ' 代表可以通过的道路，'X' 代表障碍物，'D' 代表 DashMart 的位置。 同时给定一个二维整型数组 `locations`，包含多个坐标 `[row, col]`，代表需要查询的地点。

对于 `locations` 中的每一个坐标，找到距离它最近的 DashMart 的距离。 只能上下左右移动。 如果无法到达 DashMart，则返回 -1。

Input:

`city`: `char[][]`
`locations`: `int[][2]`


Output:

`answer`: `int[]`


返回一个列表，包含 `locations` 中每个坐标到最近 DashMart 的距离。

### Thinking

### Solution
```java
import java.util.LinkedList;
import java.util.Queue;

public class Solution {

    /**
     * 计算每个地点到最近的DashMart的距离。
     *
     * @param city      二维字符数组，代表城市地图。
     * @param locations 二维整型数组，包含需要查询的坐标。
     * @return 一个整型数组，包含每个坐标到最近DashMart的距离。
     */
    public int[] findNearestDashMart(char[][] city, int[][] locations) {
        if (city == null || city.length == 0 || city[0].length == 0) {
            return new int[0];
        }

        int rows = city.length;
        int cols = city[0].length;

        // 1. 初始化距离数组和队列（多源BFS）
        int[][] distances = new int[rows][cols];
        Queue<int[]> queue = new LinkedList<>();

        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (city[i][j] == 'D') {
                    // 将所有DashMart作为BFS的起始点
                    queue.offer(new int[]{i, j});
                    distances[i][j] = 0;
                } else {
                    // -1 表示尚未访问
                    distances[i][j] = -1;
                }
            }
        }

        // 定义移动方向：上, 下, 左, 右
        int[][] directions = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};

        // 2. 执行多源BFS
        while (!queue.isEmpty()) {
            int[] current = queue.poll();
            int r = current[0];
            int c = current[1];

            for (int[] dir : directions) {
                int newR = r + dir[0];
                int newC = c + dir[1];

                // 检查新坐标是否有效
                if (newR >= 0 && newR < rows && newC >= 0 && newC < cols
                        && city[newR][newC] != 'X' && distances[newR][newC] == -1) {
                    
                    // 更新距离并加入队列
                    distances[newR][newC] = distances[r][c] + 1;
                    queue.offer(new int[]{newR, newC});
                }
            }
        }

        // 3. 查询并生成结果
        int[] answer = new int[locations.length];
        for (int i = 0; i < locations.length; i++) {
            int locR = locations[i][0];
            int locC = locations[i][1];
            answer[i] = distances[locR][locC];
        }

        return answer;
    }

    // 主函数用于测试
    public static void main(String[] args) {
        Solution sol = new Solution();

        // 示例输入
        char[][] city = {
            {'X', ' ', ' ', 'D', 'X'},
            {' ', 'X', ' ', ' ', ' '},
            {' ', ' ', ' ', 'D', ' '},
            {' ', ' ', ' ', ' ', 'X'},
            {' ', ' ', 'X', ' ', ' '}
        };

        int[][] locations = {
            {0, 1}, // 应该为 2
            {1, 3}, // 应该为 1
            {4, 3}, // 应该为 3
            {0, 0}, // 是 'X'，但查询的是坐标本身，无法到达，应为 -1
            {4, 0}  // 应该为 4
        };

        int[] result = sol.findNearestDashMart(city, locations);

        // 打印结果
        System.out.print("Distances: [");
        for (int i = 0; i < result.length; i++) {
            System.out.print(result[i]);
            if (i < result.length - 1) {
                System.out.print(", ");
            }
        }
        System.out.println("]");
        // 预期输出: Distances: [2, 1, 3, -1, 4]
    }
}
```