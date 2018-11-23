#### Given an array of integers, write a program that efficiently finds the second largest element present in the array.

```
public class SecondLargestElement {

	public static void main(String[] args) {

		int arr[] = { 1, 1 };

		System.out.println(secondLargestNumber(arr));

	}
```
```
	private static int secondLargestNumber(int[] arr) {

		int first = Integer.MIN_VALUE, second = Integer.MIN_VALUE;

		for (int i = 0; i < arr.length; i++) {
			int value = arr[i];

			if (value > first) {
				second = first;
				first = arr[i];
			}
			if (value < first && value > second) {
				second = value;
			}
		}
		if (second == Integer.MIN_VALUE) {
			return -1;
		}
		return second;
	}

}
```
