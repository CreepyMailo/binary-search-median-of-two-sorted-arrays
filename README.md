# binary-search-median-of-two-sorted-arrays

# Intuition
Можно обойтись без полного объединения и сортировки. Так как массивы отсортированы, мы можем использовать бинарный поиск для нахождения "правильного разреза" массивов. Этот разрез делит оба массива так, что слева находятся все элементы меньше медианы, а справа больше или равные.

# Approach
Всегда выбираем для бинарного поиска более короткий массив (чтобы уменьшить количество итераций). Разбиваем оба массива на левую и правую части.

Проверяем условие:
максимальный элемент слева в первом массиве меньше или равен минимальному элементу справа во втором, и наоборот.

Если условие выполнено, значит нашли правильное разделение и можем вычислить медиану. Если нет, то сдвигаем границы бинарного поиска.

# Complexity
- Временная сложность: O(log(min(m+n)))

- Пространственная сложность: O(1)
Так как мы используем только несколько переменных для индексов.

# Code
```golang []
import (
	"math"
)

func findMedianSortedArrays(nums1 []int, nums2 []int) float64 {
	if len(nums1) > len(nums2) {
		return findMedianSortedArrays(nums2, nums1)
	}

	m, n := len(nums1), len(nums2)
	low, high := 0, m

	for low <= high {
		partitionX := (low + high) / 2
		partitionY := (m + n + 1) / 2 - partitionX

		maxLeftX := math.Inf(-1)
		if partitionX > 0 {
			maxLeftX = float64(nums1[partitionX-1])
		}

		minRightX := math.Inf(1)
		if partitionX < m {
			minRightX = float64(nums1[partitionX])
		}

		maxLeftY := math.Inf(-1)
		if partitionY > 0 {
			maxLeftY = float64(nums2[partitionY-1])
		}

		minRightY := math.Inf(1)
		if partitionY < n {
			minRightY = float64(nums2[partitionY])
		}

		if maxLeftX <= minRightY && maxLeftY <= minRightX {
			if (m+n)%2 == 0 {
				return (math.Max(maxLeftX, maxLeftY) + math.Min(minRightX, minRightY)) / 2
			}
			return math.Max(maxLeftX, maxLeftY)
		} else if maxLeftX > minRightY {
			high = partitionX - 1
		} else {
			low = partitionX + 1
		}
	}
	return 0.0
}

```
