```
public class SortWaveArray {

	public static void main(String[] args) {

		SortWaveArray object = new SortWaveArray();
		int arr[] = { 10, 90, 49, 2, 1, 5, 23 };
		int arr1[] = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
		int arr2[] = { 9, 8, 7, 6, 5, 4, 3, 2, 1 };
		object.sort(arr, arr.length);
		object.sort(arr1, arr1.length);
		object.sort(arr2, arr2.length);
		object.print(arr);
		object.print(arr1);
		object.print(arr2);
	}
```
```
	public void sort(int arr[], int n) {
		for (int i = 0; i < n; i += 2) {
			if (i > 0 && arr[i - 1] > arr[i]) {
				swap(arr, i - 1, i);
			}
			if (i < n - 1 && arr[i] < arr[i + 1])
				swap(arr, i, i + 1);
		}
	}
```
```
	void swap(int arr[], int a, int b) {
		int temp = arr[a];
		arr[a] = arr[b];
		arr[b] = temp;
	}
```
```
	void print(int arr[]) {
		for (int i : arr)
			System.out.print(i + " ");
		System.out.println();
	}
}

```
