# 2353. Design a Food Rating System

Design a food rating system that can do the following:

- Modify the rating of a food item listed in the system.
- Return the highest-rated food item for a type of cuisine in the system.

Implement the `FoodRatings` class:

- `FoodRatings(String[] foods, String[] cuisines, int[] ratings)` Initializes the system. The food items are described by `foods`, `cuisines` and `ratings`, all of which have a length of `n`.

    - `foods[i]` is the name of the `ith` food,

    - `cuisines[i]` is the type of cuisine of the `ith` food, and

    - `ratings[i]` is the initial rating of the `ith` food.

- `void changeRating(String food, int newRating)` Changes the rating of the food item with the name `food`.

- `String highestRated(String cuisine)` Returns the name of the food item that has the highest rating for the given type of `cuisine`. If there is a tie, return the item with the lexicographically smaller name.

Note that a string `x` is lexicographically smaller than string `y` if `x` comes before y in dictionary order, that is, either `x` is a prefix of `y`, or if `i` is the first position such that `x[i] != y[i]`, then `x[i]` comes before `y[i]` in alphabetic order.

## Examples

**Example 1:**

**Input:** ["FoodRatings", "highestRated", "highestRated", "changeRating", "highestRated", "changeRating", "highestRated"]
[[["kimchi", "miso", "sushi", "moussaka", "ramen", "bulgogi"], ["korean", "japanese", "japanese", "greek", "japanese", "korean"], [9, 12, 8, 15, 14, 7]], ["korean"], ["japanese"], ["sushi", 16], ["japanese"], ["ramen", 16], ["japanese"]]
**Output:** [null, "kimchi", "ramen", null, "sushi", null, "ramen"]
**Explanation:**
FoodRatings foodRatings = new FoodRatings(["kimchi", "miso", "sushi", "moussaka", "ramen", "bulgogi"], ["korean", "japanese", "japanese", "greek", "japanese", "korean"], [9, 12, 8, 15, 14, 7]);
foodRatings.highestRated("korean"); // return "kimchi"
                                    // "kimchi" is the highest rated korean food with a rating of 9.
foodRatings.highestRated("japanese"); // return "ramen"
                                      // "ramen" is the highest rated japanese food with a rating of 14.
foodRatings.changeRating("sushi", 16); // "sushi" now has a rating of 16.
foodRatings.highestRated("japanese"); // return "sushi"
                                      // "sushi" is the highest rated japanese food with a rating of 16.
foodRatings.changeRating("ramen", 16); // "ramen" now has a rating of 16.
foodRatings.highestRated("japanese"); // return "ramen"
                                      // Both "sushi" and "ramen" have a rating of 16.
                                      // However, "ramen" is lexicographically smaller than "sushi".

## Constraints:

- `1 <= n <= 2 * 10^4`
- `n == foods.length == cuisines.length == ratings.length`
- `1 <= foods[i].length, cuisines[i].length <= 10`
- `foods[i]`, `cuisines[i]` consist of lowercase English letters.
- `1 <= ratings[i] <= 10^8`
- All the strings in `foods` are distinct.
- `food` will be the name of a food item in the system across all calls to `changeRating`.
- `cuisine` will be a type of cuisine of at least one food item in the system across all calls to `highestRated`.
- At most `2 * 10^4` calls in total will be made to `changeRating` and `highestRated`.

## Solution

```python
class AutoSortArray:

    def __init__(self):
        self.arr = []

    def binary_search(self, arr, val, start, end):
        
        # we need to distinguish whether we 
        # should insert before or after the 
        # left boundary. imagine [0] is the last 
        # step of the binary search and we need 
        # to decide where to insert -1
        if start == end:
            if arr[start] > val:
                return start
            else:
                return start+1

        # this occurs if we are moving 
        # beyond left's boundary meaning 
        # the left boundary is the least 
        # position to find a number greater than val
        if start > end:
            return start

        mid = (start+end)//2
        if arr[mid] < val:
            return self.binary_search(arr, val, mid+1, end)
        elif arr[mid] > val:
            return self.binary_search(arr, val, start, mid-1)
        else:
            return mid


    def insertion(self, val):
        if len(self.arr) > 0:
            i = len(self.arr)
            j = self.binary_search(self.arr, val, 0, i-1)
            self.arr = self.arr[:j] + [val] + self.arr[j:i] + self.arr[i+1:]
        else:
            self.arr.append(val)
        return self.arr


    def remove(self, val):
        if len(self.arr) > 0:
            i = len(self.arr)
            j = self.binary_search(self.arr, val, 0, i-1)
            #print("val:", val, "j:", j, "i:", i)
            if j < i:
                for k in range(j - 1,i):
                    #print(self.arr[k], k)
                    if self.arr[k] == val:
                        self.arr.pop(k)
                        break
            else:
                self.arr.pop(j-1)
        else:
            self.arr.pop(0)
        return self.arr


class FoodRatings:

    def __init__(self, foods: List[str], cuisines: List[str], ratings: List[int]):
  
        self.cuisine_map = {}
        self.food_map = {}
        self.food_cuisine_map = {}
 
        for food,rating,cuisine in zip(foods,ratings,cuisines):
            self.food_map[food] = rating
            self.food_cuisine_map[food] = cuisine
            self.cuisine_map.setdefault(cuisine,AutoSortArray()).insertion((-rating,food))
        
        #print(self.cuisine_map)
        
    def changeRating(self, food: str, newRating: int) -> None:
        self.cuisine_map[self.food_cuisine_map[food]].remove((-self.food_map[food],food))
        self.food_map[food] = newRating
        self.cuisine_map[self.food_cuisine_map[food]].insertion((-self.food_map[food],food))
        #print(self.cuisine_map[self.food_cuisine_map[food]].arr)

    def highestRated(self, cuisine: str) -> str:
        
        aux_food = self.cuisine_map[cuisine].arr[0][1]

        return aux_food

# Your FoodRatings object will be instantiated and called as such:
# obj = FoodRatings(foods, cuisines, ratings)
# obj.changeRating(food,newRating)
# param_2 = obj.highestRated(cuisine)
```