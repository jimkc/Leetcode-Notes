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