---
title : "Maximum subarray sum"
author : ["Suraj Yadav"]
date : 2022-05-31
tags : ["solution"]
draft : false
layout: ../../layouts/RequestResponse.astro
---

## Maximum Subarray Sum {#maximum-subarray-sum}

**What is maximum subarray sum of `{}`?**

-   0

**What is maximum subarray sum of `{1}`?**

-   1

**What is maximum subarray sum of `{1, -2}`?**

-   1, because including -2 will make the sum negative

**What is the subarray sum of `{1}` in the considered array `{1, ...}`?**

-   The sum of subarray `{1}` is 1

**Assuming there is potential positive sum subarray head, should we include the subarray `{1}` in the array `{1, ...}`?**

-   if we include `{1}`, it will bring the subarray sum up by `1`
-   So yes

**Should we include the first two elements `{1, -2}` for subarray sum in the array `{1, -2, ...}`?**

-   The sum of the two elements is `-1`
-   Assuming there is a potential positive sum subarray ahead, including the elements will bring down the sum by `1`
-   So no

**What about `{4, -3}` in the array `{4, -3, ...}`?**

-   The sum of `{4, -3}` is `1`, so yes

**What about `{4, -3}` in the array `{4, -3, 1}`?**

-   If we do add the subarray sum `{4, -3}`, it will bring up the subarray sum of `{1}` by 1.
-   But clearly `4` is the maximum subarray sum
-   We need more clarity

**What about `{4, -3}` in the array `{4, -3, 10}`?**

-   If we do add the subarray sum `{4, -3}`, it will bring up the subarray sum of `{10}` by 1.
-   In this case, the subarray `{4, -3}` did help

**How do we overcome this ambiguity?**

-   By storing the subarray sum at every point in traversal

**What is the maximum subarray sum for the following array?**

    |       |   |    |   |
    |-------|---|----|---|
    | Array | 4 | -3 | 1 |

-   Starting with the first element, since its positive

    | Array         | 4 | -3 | 1 |
    |---------------|---|----|---|
    | Subarray Sums | 4 |    |   |

-   `4 + -3` = `1`. Since `1 > 0`, we do not reset the sum

    | Array         | 4 | -3 | 1 |
    |---------------|---|----|---|
    | Subarray Sums | 4 | 1  |   |

-   `1 + 1` = `2`.

    | Array         | 4 | -3 | 1 |
    |---------------|---|----|---|
    | Subarray Sums | 4 | 1  | 2 |

**We have calculated all the subarray sums. `Max(4, 1, 2) = 4`. Hence the maximum subarray sum for `{4, -3, 1}` is 4**

<!--listend-->

**What about the array**

|       |   |    |   |    |    |
|-------|---|----|---|----|----|
| Array | 4 | -5 | 3 | -2 | 17 |

-   Same for first element

    | Array         | 4 | -5 | 3 | -2 | 17 |
    |---------------|---|----|---|----|----|
    | Subarray Sums | 4 |    |   |    |    |

-   `4 + -5 = -1`. Since `-1 < 0`, we reset the subarray sum to 0.

    | Array         | 4 | -5 | 3 | -2 | 17 |
    |---------------|---|----|---|----|----|
    | Subarray Sums | 4 | -1 |   |    |    |

-   Since sum has been reset, it'll be `3` for `{3}`

    | Array         | 4 | -5 | 3 | -2 | 17 |
    |---------------|---|----|---|----|----|
    | Subarray Sums | 4 | -1 | 3 |    |    |

-   `3 + -2 = 1`, which can increase the sum of potential subarray ahead

    | Array         | 4 | -5 | 3 | -2 | 17 |
    |---------------|---|----|---|----|----|
    | Subarray Sums | 4 | -1 | 3 | 1  |    |

-   `1 + 17 = 18`

    | Array         | 4 | -5 | 3 | -2 | 17 |
    |---------------|---|----|---|----|----|
    | Subarray Sums | 4 | -1 | 3 | 1  | 18 |

**`Max(4 , -1 , 3 ,  1 , 18) = 18`, Hence `18` is the maximum subarray sum**

<!--listend-->

**Is this better than 0(n<sup>2</sup>)?**

-   Yes, because its linear. Thanks Kadane.
