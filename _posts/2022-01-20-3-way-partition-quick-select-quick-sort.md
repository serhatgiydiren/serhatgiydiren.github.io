---
title: 3-way Partitioning & Quick Select & Quick Sort & Find Kth Largest Element
published: true
redirect_to: https://serhatgiydiren.com/3-way-partition-quick-select-quick-sort/
---

```cpp
struct bound
{
 int lt;
 int gt;
};

bound partition_3way(vector < int > &arr, int lo, int hi)
{
 int pivot=arr[lo],i=lo;
 while(i<=hi)
 {
  if (arr[i]<pivot) swap(arr[i++],arr[lo++]);
  else if (arr[i]>pivot) swap(arr[i],arr[hi--]);
  else i++;
 }
 return {lo,hi};
}

int quick_select(vector < int > &nums, const int &k)
{
 random_shuffle(nums.begin(),nums.end());
 int lo=0,hi=int(nums.size())-1;
 while(lo<=hi)
 {
  bound b=partition_3way(nums,lo,hi);
  if (k>b.gt) lo=b.gt+1;
  else if (k<b.lt) hi=b.lt-1;
  else return nums[b.lt];
 }
 return -1;
}

void quick_sort(vector < int > &nums, const int &lo, const int &hi)
{
 if (lo>=hi) return;
 bound b=partition_3way(nums,lo,hi);
 quick_sort(nums,lo,b.lt-1);
 quick_sort(nums,b.gt+1,hi);
}

int findKthLargest(vector < int > &nums, const int &k)
{
 return quick_select(nums,int(nums.size())-k);
}
```
