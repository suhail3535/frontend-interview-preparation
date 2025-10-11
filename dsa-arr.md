

# 🚀 JavaScript Array Interview Questions & Solutions

A collection of commonly asked **JavaScript interview problems** with optimal solutions, explanations, and time/space complexity.

---

## 📚 Table of Contents

1. [Reverse an Array](#reverse-an-array)

   * Approach 1: Using `.map()`
   * Approach 2: Two Pointer (In-place)
   * Approach 3: Reverse Loop
2. [Find Second Maximum in Array](#find-second-maximum-in-array)
3. [Remove Duplicates from Array](#remove-duplicates-from-array)

---

## ✅ Reverse an Array

### ✅ Approach 1: Using `.map()` (Immutable)

```javascript
let arr = [1, 2, 3, 4, 5];
let reversed = arr.map((_, i, a) => a[a.length - 1 - i]);
console.log(reversed); // [5, 4, 3, 2, 1]
```

| Detail                  | Value |
| ----------------------- | ----- |
| Time Complexity         | O(n)  |
| Space Complexity        | O(n)  |
| Mutates Original Array? | ❌ No  |

---

### ✅ Approach 2: Two-Pointer (In-Place Swap)

```javascript
let arr = [1, 2, 3, 4, 5];
let start = 0, end = arr.length - 1;

while (start < end) {
  [arr[start], arr[end]] = [arr[end], arr[start]];
  start++;
  end--;
}
console.log(arr); // [5, 4, 3, 2, 1]
```

| Detail                  | Value |
| ----------------------- | ----- |
| Time Complexity         | O(n)  |
| Space Complexity        | O(1)  |
| Mutates Original Array? | ✅ Yes |

---

### ✅ Approach 3: Reverse Loop

```javascript
let arr = [1, 2, 3, 4, 5];
let reversed = [];

for (let i = arr.length - 1; i >= 0; i--) {
  reversed.push(arr[i]);
}
console.log(reversed); // [5, 4, 3, 2, 1]
```

| Detail                  | Value |
| ----------------------- | ----- |
| Time Complexity         | O(n)  |
| Space Complexity        | O(n)  |
| Mutates Original Array? | ❌ No  |

---

## ✅ Find Second Maximum in Array

```javascript
let arr = [12, 35, 1, 10, 34, 1];
let max = -Infinity;
let secondMax = -Infinity;

for (let num of arr) {
  if (num > max) {
    secondMax = max;
    max = num;
  } else if (num > secondMax && num < max) {
    secondMax = num;
  }
}
console.log("Second Max:", secondMax);
```

| Detail           | Value |
| ---------------- | ----- |
| Time Complexity  | O(n)  |
| Space Complexity | O(1)  |

---

## ✅ Remove Duplicates from Array

### Approach 1: Using Object (Hash Map)

```javascript
function unique(arr) {
  const seen = {};
  const result = [];

  arr.forEach(item => {
    if (!seen[item]) {
      seen[item] = true;
      result.push(item);
    }
  });

  return result;
}

console.log(unique([1, 2, 2, 3, 4, 4, 5]));
```

| Detail           | Value |
| ---------------- | ----- |
| Time Complexity  | O(n)  |
| Space Complexity | O(n)  |

---

### Approach 2: Using `.filter()`

```javascript
let arr = [1, 2, 2, 3, 4, 4, 5];
let uniqueArr = arr.filter((item, index) => arr.indexOf(item) === index);
console.log(uniqueArr);
```

| Detail           | Value |
| ---------------- | ----- |
| Time Complexity  | O(n²) |
| Space Complexity | O(n)  |

---


