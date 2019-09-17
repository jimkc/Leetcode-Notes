## Question
Suppose we abstract our file system by a string in the following manner:<br>

The string "dir\n\tsubdir1\n\tsubdir2\n\t\tfile.ext" represents:
<pre>
dir
    subdir1
    subdir2
        file.ext
</pre>

The directory dir contains an empty sub-directory subdir1 and a sub-directory subdir2 containing a file file.ext.<br>

The string "dir\n\tsubdir1\n\t\tfile1.ext\n\t\tsubsubdir1\n\tsubdir2\n\t\tsubsubdir2\n\t\t\tfile2.ext" represents:<br>
<pre>
dir
    subdir1
        file1.ext
        subsubdir1
    subdir2
        subsubdir2
            file2.ext
</pre>

The directory dir contains two sub-directories subdir1 and subdir2. subdir1 contains a file file1.ext and an empty second-level sub-directory subsubdir1. subdir2 contains a second-level sub-directory subsubdir2 containing a file file2.ext.<br>

We are interested in finding the longest (number of characters) absolute path to a file within our file system. For example, in the second example above, the longest absolute path is "dir/subdir2/subsubdir2/file2.ext", and its length is 32 (not including the double quotes).<br>

Given a string representing the file system in the above format, return the length of the longest absolute path to file in the abstracted file system. If there is no file in the system, return 0.<br>

Note:<br>
The name of a file contains at least a . and an extension.<br>
The name of a directory or sub-directory will not contain a ..<br>
Time complexity required: O(n) where n is the size of the input string.<br>

Notice that a/aa/aaa/file1.txt is not the longest file path, if there is another path aaaaaaaaaaaaaaaaaaaaa/sth.png.<br>

## Thinking
Use dictionary to remember each depth len, the code walks through each file as a dfs,
so if the file is found the previous depth record can be covered.

## Coding
Time: O(n);<br>
Space: O(1)
```python3
class Solution:
    def lengthLongestPath(self, input):
        maxlen = 0
        pathlen = {0: 0} # Record the max length in depth
        
        for line in input.splitlines():
            name = line.lstrip('\t')
            depth = len(line) - len(name) # See how many difference of tabs they have before and after lstrip
            
            if '.' in name:
                maxlen = max(maxlen, pathlen[depth] + len(name)) # The previous depth len add the len(name)
            else:
                pathlen[depth + 1] = pathlen[depth] + len(name) + 1 # name belongs to depth + 1, add 1 for '/'
        return maxlen
                        
```
