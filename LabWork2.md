Нижче наведено лістинг 1.1 - Ітеративний алгоритм сортування злиттям
```csharp
using System;

class Program
{
    static (int comparisons, int assignments) Merge(int[] a, int left, int mid, int right)
    {
        int comparisons = 0;
        int assignments = 0;

        int n1 = mid - left;
        int n2 = right - mid;

        int[] L = new int[n1];
        int[] R = new int[n2];

        for (int i = 0; i < n1; i++)
        {
            L[i] = a[left + i];
            assignments++;
        }

        for (int i = 0; i < n2; i++)
        {
            R[i] = a[mid + i];
            assignments++;
        }

        int it1 = 0;
        int it2 = 0;
        int k = left;

        assignments += 3;

        while (it1 < n1 && it2 < n2)
        {
            comparisons++;

            if (L[it1] < R[it2])
            {
                a[k] = L[it1];
                it1++;
                assignments++;
            }
            else
            {
                a[k] = R[it2];
                it2++;
                assignments++;
            }

            k++;
            assignments++;
        }

        while (it1 < n1)
        {
            a[k] = L[it1];
            it1++;
            k++;
            assignments++;
        }

        while (it2 < n2)
        {
            a[k] = R[it2];
            it2++;
            k++;
            assignments++;
        }

        return (comparisons, assignments);
    }

    static (int[] array, int comparisons, int assignments) MergeSortIterative(int[] a)
    {
        int n = a.Length;
        int comparisons = 0;
        int assignments = 0;

        int i = 1;

        while (i < n)
        {
            int j = 0;

            while (j < n - i)
            {
                int left = j;
                int mid = j + i;
                int right = Math.Min(j + 2 * i, n);

                var result = Merge(a, left, mid, right);

                comparisons += result.comparisons;
                assignments += result.assignments;

                j += 2 * i;
            }

            i *= 2;
        }

        return (a, comparisons, assignments);
    }

    static void Main()
    {
        int[] myList = { 80, 27, 37, 36, 91, 53, 86, 66, 98 };

        Console.WriteLine("Оригінальний список: " + string.Join(", ", myList));

        var result = MergeSortIterative((int[])myList.Clone());

        Console.WriteLine("Відсортований список: " + string.Join(", ", result.array));
        Console.WriteLine($"Кількість порівнянь: {result.comparisons}");
        Console.WriteLine($"Кількість присвоювань: {result.assignments}");
    }
}
```
Результат роботи алгоритму
```
Оригінальний список: 80, 27, 37, 36, 91, 53, 86, 66, 98
Відсортований список: 27, 36, 37, 53, 66, 80, 86, 91, 98
Кількість порівнянь: 24
Кількість присвоювань: 114
```

Нижче наведено лістинг 1.2 - Рекурсивний алгоритм сортування злиттям
```csharp
using System;

class Program
{
    static (int[] array, int comparisons, int assignments) Merge(
        int[] left, int[] right)
    {
        int[] merged = new int[left.Length + right.Length];

        int comparisons = 0;
        int assignments = 0;

        int i = 0;
        int j = 0;
        int k = 0;

        while (i < left.Length && j < right.Length)
        {
            comparisons++;

            if (left[i] <= right[j])
            {
                merged[k] = left[i];
                i++;
            }
            else
            {
                merged[k] = right[j];
                j++;
            }

            k++;
            assignments++;
        }

        while (i < left.Length)
        {
            merged[k] = left[i];
            i++;
            k++;
            assignments++;
        }

        while (j < right.Length)
        {
            merged[k] = right[j];
            j++;
            k++;
            assignments++;
        }

        return (merged, comparisons, assignments);
    }

    static (int[] array, int comparisons, int assignments, int recursiveCalls)
        MergeSortRecursive(int[] arr)
    {
        int comparisons = 0;
        int assignments = 0;
        int recursiveCalls = 1;

        if (arr.Length <= 1)
            return (arr, comparisons, assignments, recursiveCalls);

        int mid = arr.Length / 2;
        assignments++;

        int[] left = new int[mid];
        int[] right = new int[arr.Length - mid];

        Array.Copy(arr, 0, left, 0, mid);
        Array.Copy(arr, mid, right, 0, arr.Length - mid);

        var leftResult = MergeSortRecursive(left);
        var rightResult = MergeSortRecursive(right);

        comparisons += leftResult.comparisons + rightResult.comparisons;
        assignments += leftResult.assignments + rightResult.assignments;

        recursiveCalls += leftResult.recursiveCalls + rightResult.recursiveCalls;

        var mergeResult = Merge(leftResult.array, rightResult.array);

        comparisons += mergeResult.comparisons;
        assignments += mergeResult.assignments;

        return (
            mergeResult.array,
            comparisons,
            assignments,
            recursiveCalls
        );
    }

    static void Main()
    {
        int[] myList = { 80, 27, 37, 36, 91, 53, 86, 66, 98 };

        Console.WriteLine("Оригінальний список: " + string.Join(", ", myList));

        var result = MergeSortRecursive(myList);

        Console.WriteLine("Відсортований список: " + string.Join(", ", result.array));
        Console.WriteLine($"Загальна кількість порівнянь: {result.comparisons}");
        Console.WriteLine($"Загальна кількість присвоювань: {result.assignments}");
        Console.WriteLine($"Загальна кількість рекурсивних викликів: {result.recursiveCalls}");
    }
}
```
Результат роботи алгоритму
```
Оригінальний список: 80, 27, 37, 36, 91, 53, 86, 66, 98
Відсортований список: 27, 36, 37, 53, 66, 80, 86, 91, 98
Загальна кількість порівнянь: 19
Загальна кількість присвоювань: 37
Загальна кількість рекурсивних викликів: 17
```

Нижче наведено лістинг 1.3 - Алгоритм швидкого сортування
```csharp
using System;

class Program
{
    static (int q, int comparisons, int assignments) Partition(int[] a, int l, int r)
    {
        int comparisons = 0;
        int assignments = 0;

        int pivot = a[l];
        assignments++;

        int i = l - 1;
        int j = r + 1;
        assignments += 2;

        while (true)
        {
            i++;
            assignments++;

            while (a[i] < pivot)
            {
                comparisons++;
                i++;
                assignments++;
            }

            comparisons++;

            j--;
            assignments++;

            while (a[j] > pivot)
            {
                comparisons++;
                j--;
                assignments++;
            }

            comparisons++;

            comparisons++;

            if (i >= j)
                return (j, comparisons, assignments);

            int temp = a[i];
            a[i] = a[j];
            a[j] = temp;
            assignments += 3;
        }
    }

    static (int comparisons, int assignments, int recursiveCalls)
        QuickSort(int[] a, int l, int r)
    {
        int comparisons = 0;
        int assignments = 0;
        int recursiveCalls = 1;

        if (l < r)
        {
            var result = Partition(a, l, r);

            comparisons += result.comparisons;
            assignments += result.assignments;

            var left = QuickSort(a, l, result.q);
            var right = QuickSort(a, result.q + 1, r);

            comparisons += left.comparisons + right.comparisons;
            assignments += left.assignments + right.assignments;

            recursiveCalls += left.recursiveCalls + right.recursiveCalls;
        }

        return (comparisons, assignments, recursiveCalls);
    }

    static void Main()
    {
        int[] myList = { 80, 27, 37, 36, 91, 53, 86, 66, 98 };

        int[] originalList = (int[])myList.Clone();

        Console.WriteLine("Оригінальний список: " + string.Join(", ", originalList));

        var result = QuickSort(myList, 0, myList.Length - 1);

        Console.WriteLine("Відсортований список: " + string.Join(", ", myList));

        Console.WriteLine($"Загальна кількість порівнянь: {result.comparisons}");

        Console.WriteLine($"Загальна кількість присвоювань: {result.assignments}");

        Console.WriteLine($"Загальна кількість рекурсивних викликів: {result.recursiveCalls}");
    }
}
```

Результат роботи алгоритму
```
Оригінальний список: 80, 27, 37, 36, 91, 53, 86, 66, 98
Відсортований список: 27, 36, 37, 53, 66, 80, 86, 91, 98
Загальна кількість порівнянь: 58
Загальна кількість присвоювань: 86
Загальна кількість рекурсивних викликів: 17
```
