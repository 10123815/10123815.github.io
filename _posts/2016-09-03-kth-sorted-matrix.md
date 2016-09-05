---
layout:     post
title:      "Kth Smallest Element in a Sorted Matrix"
date:       2016-09-03
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:      
        - algorithm
        - leetcode
---

>Given a n x n matrix where each of the rows and columns are sorted in ascending order, 
find the kth smallest element in the matrix.

我用最大堆做的，堆的大小为k，堆顶保存当前最大的数字，即矩阵中第k小的数字     
建堆的时候，每push一个数字到堆的末尾，这个数字可能表较大，要上升到合适的位置（`up`）；    
之后遍历剩下的数字，如果某个数字比堆顶小，则替换堆顶，并且这个数字要在堆中下降到合适的位置（`down`）         

```cpp
class Solution {
public:
    void down(vector<int>& heap, int i, int k) {
        int li = i * 2 + 1;
        int ri = li + 1;
        if (li < k && ri < k) {
            int ci = li;
            int max = heap[ci];
            if (max < heap[ri]) {
                ci = ri;
                max = heap[ri];
            }
            if (heap[i] < max) {
                heap[ci] = heap[i];
                heap[i] = max;
                down(heap, ci, k);
            }
        } else if (li < k && ri >= k) {
            int ci = li;
            int max = heap[li];
            if (heap[i] < max) {
                heap[ci] = heap[i];
                heap[i] = max;
                down(heap, ci, k);
            }
        } else if (li >= k && ri < k) {
            int ci = ri;
            int max = heap[ri];
            if (heap[i] < max) {
                heap[ci] = heap[i];
                heap[i] = max;
                down(heap, ci, k);
            }
        }
    }
    
    void up(vector<int>& heap, int i) {
        int pi = (i - 1) / 2;
        if (pi >= 0 && heap[i] > heap[pi]) {
            int tmp = heap[i];
            heap[i] = heap[pi];
            heap[pi] = tmp;
            up(heap, pi);
        }
    }
    
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        size_t n = matrix.size();
        vector<int> heap;
        // Init the heap
        int kk = 0;
        while (kk < k) {
            int j = kk / n;
            int i = kk % n;
            heap.push_back(matrix[i][j]);
            up(heap, kk++);
        }
        // Start search
        while (kk < n * n) {
            int j = kk / n;
            int i = kk % n;
            if (matrix[i][j] < heap[0]) {
                heap[0] = matrix[i][j];
                down(heap, 0, k);
            }
            kk++;
        }
        return heap[0];
    }
};
```

如果用stl的priority_queue:    

```cpp
class Solution {
public:
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        priority_queue<int, vector<int>> q;
        for (int i = 0; i < matrix.size(); ++i) {
            for (int j = 0; j < matrix[i].size(); ++j) {
                q.emplace(matrix[i][j]);
                if (q.size() > k) q.pop();
            }
        }
        return q.top();
    }
};
```
