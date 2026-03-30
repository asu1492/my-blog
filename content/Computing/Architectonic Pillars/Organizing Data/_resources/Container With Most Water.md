```java

public int maxAreaBruteForce(int[] height) {
    int n = height.length;
    int maxArea = 0;
    
    //edge case
    if (n < 2) return 0;

    for (int i = 0; i < n - 1; i++) {
        for (int j = i + 1; j < n; j++) {
            int width = j - i;
            int h = Math.min(height[i], height[j]);
            int area = width * h;

            if (area > maxArea) {
                maxArea = area;
            }
        }
    }

    return maxArea;
}

- Time complexity: O(n²)
- Space complexity: O(1)
```



```java
 public int maxArea(int[] height) {
	int n = height.length;
	int l = 0;
	int r = n-1;
	int max_area = 0;
  
	while(l < r){
		int w = r - l ;
		int h = Math.min(height[l], height[r]);
		int area = w * h;
		max_area = Math.max(max_area, area);

		if(height[l] < height[r]){
			l++;
		}
		else {
			r--;
		}
}
return max_area;
}


- Time complexity: O(n)
- Space complexity: O(1)
```