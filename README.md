# movie-recommender-system
The goal is to predict score for User U1 for Movie M3.

## Complete Hybrid Recommendation System Example

### The Data

#### User Ratings

|    | M1 | M2 | M3 |
|:--:|:--:|:--:|:--:|    
| U1 | 5  | 4  | ?  |
| U2 | 4  | 5  | 3  |
| U3 | 2  | 1  | 5  |

#### Movie Features

| Movie | Genre | Actor | 
|:--:|:--:|:--:| 
| M1 | Action | Actor_A |
| M2 | Action| Actor_B |
| M3 | Drama| Actor_A |

#### Popularity (Bookings)

M1: 500 bookings <br>
M2: 800 bookings <br>
M3: 300 bookings <br>

### Method 1: Content-Based Filtering

**Simple Idea:** "You liked Action movies before, will you like this Drama movie?"

#### Step 1: What did U1 watch?

* M1 → rated 5 ⭐⭐⭐⭐⭐
* M2 → rated 4 ⭐⭐⭐⭐

#### Step 2: How similar is M3 to these movies? <br>
#### M3 vs M1:

* M1: Action, Actor_A <br>
* M3: Drama, Actor_A <br>

* Genre match? Action ≠ Drama → NO (0 points) <br>
* Actor match? Actor_A = Actor_A → YES (1 point) <br>

#### Similarity = 1/2 = 0.5

#### M3 vs M2:

* M2: Action, Actor_B <br>
* M3: Drama, Actor_A <br>

* Genre match? Action ≠ Drama → NO (0 points) <br>
* Actor match? Actor_B = Actor_A → NO (0 points) <br>

#### Similarity = 0/2 = 0.0

#### Step 3: Calculate Content Based Score

<pre>
CB_Score = (0.5 * 5 + 0.0 * 4) / (5 + 4) <br><br><br>
         = (2.5 + 0) / 9

</pre>      










