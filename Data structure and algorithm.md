## Sort

```
// 比较相邻元素，每轮将一个最大元素冒泡到最后
// 时间：最坏n^2（逆序），最优n(顺序，因为有flag存在），平均n^2
// 空间：辅助空间O(1)
void bubbleSort(vector<int>& vec)
{
    int length = vec.size();
    
    for(int i=length;i>0;i--)
    {
        bool flag = false;
        for (int j=1;j<i;j++)
        {
            if(vec[j-1]>vec[j])
            {
                flag = true;
                swap(vec[j-1], vec[j]);
            }
        }
        if(!flag)
        {
            break;
        }
    }
}

// 每次从未排序序列中选择最小的，和未排序序列第一个元素进行交换。
// 时间：最坏n^2，最优n^2，平均n^2
// 空间：辅助空间O(1)
void selectSort(vector<int>& vec)
{
    int length = vec.size();
    for(int i=0;i<length;i++)
    {
        int minIndex = i;
        for(int j=i+1;j<length;j++)
        {
            if(vec[j]<vec[minIndex])
            {
                minIndex = j;
            }
        }
        if (minIndex != i)
        {
            swap(vec[i], vec[minIndex]);
        }
    }
}

// 依次将元素插入到有序序列(初始为vec[0])的合适位置
// 时间：最坏n^2，最优n^2，平均n^2
// 空间：辅助空间O(1)
void insertSort(vector<int>& vec)
{
    int length = vec.size();
    for(int i=1;i<length;i++)
    {
        int x = vec[i];
        int j = i;
        for(;j>0;j--)
        {
            if (x < vec[j-1]) {
                vec[j] = vec[j-1];
            } else {
                break;
            }
        }
        vec[j] = x;
    }
}

void mergeArray(vector<int>&vec, int l, int m, int r)
{
    vector<int> temp; // 可以重复使用
    int i = l;
    int j = m+1;
    while(i<=m && j<=r)
    {
        if(vec[i]<vec[j])
        {
            temp.push_back(vec[i++]);
        }
        else
        {
            temp.push_back(vec[j++]);
        }
    }
    while(i<=m)
    {
        temp.push_back(vec[i++]);
    }
    while(j<=r)
    {
        temp.push_back(vec[j++]);
    }
    
    for(int k=0;k<temp.size();k++)
    {
        vec[l+k] = temp[k];
    }
}

// 递归排序左右两个子区间，然后进行合并
// 时间：最坏nlogn，最优nlogn，平均nlogn
// 空间：辅助空间O(n)
void mergeSort(vector<int>& vec, int l, int r)
{
    if(l<r)
    {
        int m = (l+r) / 2;
        mergeSort(vec, l, m);
        mergeSort(vec, m+1, r);
        mergeArray(vec, l, m, r);
    }
}

// 选择基准数，保证左边的数都比基准小，右边的都比基准大，然后递归对左右区间重复此操作
// 时间：最坏n^2（逆序），最优nlogn，平均nlogn
// 空间：辅助空间O(logn) （递归深度logn, 每次常数空间）
void quickSort(vector<int>& vec, int l, int r)
{
    if (l < r)
    {
        int i = l;
        int j = r;
        int x = vec[l]; // 也可以选择其他位置，和vec[l]交换即可
        while(i < j)
        {
            while (i < j && vec[j] >= x)
            {
                j--;
            }
            while (i < j && vec[i] <= x)
            {
                i++;
            }
            if (i < j)
            {
                swap(vec[i], vec[j]);
            }
        }
        swap(vec[l], vec[i]); // 将基准数（vec[l]）归位, vec[i]一定小于等于vec[l]
        
        quickSort(vec, l, i-1);
        quickSort(vec, i+1, r);
    }
}

void maxHeapify(vector<int>& vec, int i, int n)
{
    int temp = vec[i];
    int j = 2 * i + 1;
    while(j < n)
    {
        if(j+1 < n && vec[j+1] > vec[j])
        {
            j++;
        }
        if(temp >= vec[j])
        {
            break;
        }
        
        vec[i] = vec[j];
        i = j;
        j = 2 * j + 1;
    }
    vec[i] = temp;
}

// 首先堆化数组（最大堆），然后依次将最大元素（vec[0]）和无序序列最后一个元素交换，同时堆化无序序列。
// 时间：最坏nlogn，最优nlogn，平均nlogn
// 空间：辅助空间O(1)
void heapSort(vector<int>& vec)
{
    int length = vec.size();
    for(int i=length / 2 - 1;i>=0;i--)
    {
        maxHeapify(vec, i, length);
    }
    
    for(int i=length-1;i>=0;i--)
    {
        swap(vec[i], vec[0]);
        maxHeapify(vec, 0, i);
    }
}
```