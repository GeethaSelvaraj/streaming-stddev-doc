# streaming-stddev-doc

````markdown
# 📊 Incremental Standard Deviation Calculation for Streaming Data

### ✍️ Author: [Geetha Selvaraj]
### 📅 Date: [2025-05-05]

---

## 📌 Abstract

In high-volume data processing scenarios, calculating statistical metrics like **standard deviation** across dynamic or streaming datasets poses both performance and memory challenges. While existing methods like **Welford’s algorithm** offer efficient ways to update mean and variance incrementally, they often abstract away intermediate steps.

This method presents a **custom-derived algorithm** that updates the standard deviation while tracking intermediate means — enabling better transparency and control. It is **numerically stable**, **scalable**, and **suitable for large-scale streaming or batch data**.

---

## 🔍 Problem Statement

**Given:**
- A dataset with:
  - Mean: `mean`
  - Std deviation: `ostd`
  - Count: `oldCount`
- A list of **new elements** to be added

**Goal:**
- Efficiently calculate the **updated standard deviation** without revisiting the original data.

---

## 🧠 Derivation

### 1. Mean Update

```js
newMean = (prevMean * count + e_i) / (count + 1)
````

Track intermediate means in an array `mArr`, where:

* `mArr[0] = mean`
* `mArr[i+1] = mean after processing i-th new element`

---

### 2. Variance Update

Let:

* `var = ostd²`
* `x_i` = i-th new element
* `\bar{x}_i`, `\bar{x}_{i+1}` = means before and after adding `x_i`

Update the sum of squares:

```math
dividend = (oldCount - 1) * ostd² + Σ (xᵢ - mArr[i+1]) * (xᵢ - mArr[i])
```

Then compute:

```math
newVariance = dividend / (oldCount + k - 1)

newStdDev = sqrt(newVariance)
```

---

## 🧑‍💻 JavaScript Implementation

```js
let mArr = [mean];
let count = oldCount;

newelements.forEach((e, i) => {
    const newMean = (mArr[i] * count + e) / (count + 1);
    count++;
    mArr.push(newMean);
});

let dividend = (oldCount - 1) * ostd * ostd;
let divisor = oldCount + newelements.length - 1;

for (let i = 0; i < newelements.length; i++) {
    const x = newelements[i];
    dividend += (x - mArr[i + 1]) * (x - mArr[i]);
}

const newStdDev = Math.sqrt(dividend / divisor);
```

---

## 📊 Comparison to Welford’s Algorithm

| Feature                     | Welford’s Method | This Custom Method            |
| --------------------------- | ---------------- | ----------------------------- |
| Tracks intermediate means   | ❌ No             | ✅ Yes                         |
| Memory-efficient            | ✅ Yes            | ⚠️ Slightly more memory usage |
| Numerically stable          | ✅ Yes            | ✅ Yes                         |
| Suited for large data       | ✅ Yes            | ✅ Yes                         |
| Designed for explainability | ❌ Abstracted     | ✅ Transparent & explainable   |

---

## ✅ Use Cases

* Real-time sensor data analysis
* Streaming data pipelines
* Massive dataset updates (e.g., 1M+ rows)
* Dashboards requiring accurate stats on the fly

---

## ✅ Conclusion

This approach provides a practical, stable, and explainable method for updating standard deviation in streaming scenarios. It’s well-suited for performance-critical analytics pipelines where full recomputation is not viable.

---

> Feel free to fork, improve, or integrate it into your projects. PRs and feedback welcome!

```
