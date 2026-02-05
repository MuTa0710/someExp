# Quicksort
## 快速排序的思想
快速排序的算法是从下标 left 到下标 right 的一段数组中，指定（可以是随机，也可以是 left 或者 right ）一个元素，把这段数组中比这个元素大的放在右边，比这个元素小的放在左边，然后把这个元素放到中间位置。
![quicksort](quicksort.png)

下面给出两种实现这个算法的方法，其实最重要的是partition这个函数的实现。

第一种：

    def partition(arr, low, high):
        # 1. 选取最右侧元素作为基准
        pivot = arr[high]
        
        # 2. i 指向比基准小的区域的最后一个元素
        # 初始时，该区域为空，所以 i 指向 low - 1
        i = low - 1
        
        # 3. 遍历从 low 到 high - 1 的元素
        for j in range(low, high):
            if arr[j] <= pivot:
                # 发现一个比 pivot 小的元素，扩充“较小值区”
                i += 1
                # 将该元素交换到 i 的位置
                arr[i], arr[j] = arr[j], arr[i]
        
        # 4. 最后将基准元素（arr[high]）交换到它该在的位置（i + 1）
        arr[i + 1], arr[high] = arr[high], arr[i + 1]
        
        # 返回基准元素的下标，用于下次递归
        return i + 1



第二种：

    def partition(self, nums,  left, right):
            i = randint(left, right)
            pivot = nums[i]
            nums[i], nums[left] = nums[left], nums[i]
            i, j = left + 1, right
            while True:
                while i <= j and nums[i] < pivot:
                    i += 1
                while i <= j and nums[j] > pivot:
                    j -= 1
                if i >= j:
                    
                    break
                nums[i], nums[j] = nums[j], nums[i]
                i += 1
                j -= 1
            nums[left], nums[j] = nums[j], nums[left]
            return j

然后直接递归左右分区即可：

    def quick_sort(arr, low, high):
        if low < high:
            # 获取分区后的基准位置
            pi = partition(arr, low, high)
            
            # 递归排序左半部分 (low 到 pi-1)
            quick_sort(arr, low, pi - 1)
            # 递归排序右半部分 (pi+1 到 high)
            quick_sort(arr, pi + 1, high)