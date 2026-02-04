# movie-recommender-system

* [The Data](#the-data)

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
CB_Score = (0.5 * 5 + 0.0 * 4) / (5 + 4)
         = (2.5 + 0) / 9
         = 2.5 / 9
         = 0.278
</pre>      
<br>
Normalize to 0-1: Already in scale

#### Content-Based Score = 0.28 ✅ 

### Method 2: Collaborative Filtering

**Simple Idea:** "Find people similar to you, see what they rated M3"

#### Step 1: Find users similar to U1

We compare U1 with other users based on movies they both rated (M1 and M2).

#### U1 vs U2:

U1 ratings: (5,4)  (for M1, M2) <br>
U2 ratings: (4,5)  (for M1, M2) <br>

Both liked these movies! Similar taste! <br>

#### Calculate Cosine Similarity:

<pre>
Similarity = (U1·U2) / (|U1| × |U2|)
           = (5×4 + 4×5) / (√(5²+4²) × √(4²+5²))
           = (20 + 20) / (√41 × √41)
           = 40 / 41
           = 0.976 (Very similar! 97.6%)
</pre>

#### U1 vs U3:

U1 ratings: (5,4)  (for M1, M2) <br>
U3 ratings: (2,1)  (for M1, M2) <br>

Opposite taste! U1 loves what U3 hates! <br>

#### Calculate Cosine Similarity:

<pre>
Similarity = (5×2 + 4×1) / (√41 × √5)
           = 14 / 14.32
           = 0.977
</pre>

But wait! They have OPPOSITE preferences! <br>
Let's use Pearson correlation = -0.8 (negative) <br>

For this example, we'll ignore negative similarities. <br>
So Sim(U1, U3) = 0 (we don't use it) <br>

#### Step 2: Predict U1's rating using similar users

Only U2 is similar to U1 (similarity = 0.976) <br>
U2 rated M3 = 3 stars <br>

Predicted_Rating = Σ(Similarity × Rating) / Σ(Similarity)

<pre>
CF_score = (0.976 × 3) / 0.976
         = 2.928 / 0.976
         = 3.0
</pre>

Normalize to 0-1 scale: <br>
CF_score = 3.0 / 5 = 0.60

#### Collaborative Filtering Score = 0.60 ✅

### Method 3: Popularity-Based

**Simple Idea:** "What's trending? People usually like popular stuff!"

#### Calculate Popularity Score

M1: 500 bookings
M2: 800 bookings (highest) <br>
M3: 300 bookings (lowest) <br>

Normalize M3's popularity:
<pre>
Pop_score = (M3_bookings - Min_bookings) / (Max_bookings - Min_bookings)
          = (300 - 300) / (800 - 300)
          = 0 / 500
          = 0.0
</pre>

#### Popularity Score = 0.0** ✅ (M3 is least popular)

### Final Step: Combine All Three (Hybrid Score)

#### Decide on Weights

Let's use **business-driven weights** for BookMyShow:

w_CB = 0.30  (30% - content features) <br>
w_CF = 0.50  (50% - user behavior is most important) <br>
w_Pop = 0.20 (20% - trending factor) <br>

Total = 100% <br>

#### Calculate Final Hybrid Score

Hybrid_score = w_CB × CB_score + w_CF × CF_score + w_Pop × Pop_score

<pre>
Hybrid_score = 0.30 × 0.28 + 0.50 × 0.60 + 0.20 × 0.0
             = 0.084 + 0.300 + 0.0
             = 0.384
</pre>

#### Convert back to 5-star scale (optional):
Final_Rating = 0.384 × 5 = 1.92 stars

### Complete Summary

| Method | What it Says | Score | Weight | Contribution |
|:--:|:--:|:--:|:--:|:--:|
| Content-Based | "M3 is Drama, you like Action" ❌ | 0.28 | 30% | 0.084 |
| Collaborative | "Similar user gave it 3/5" ⚠️ | 0.60 | 50% | 0.300 |
| Popularity | "M3 is least popular" ❌ | 0.00 | 20% | 0.000 |
| FINAL HYBRID | "Probably won't like much" | 0.384 | 100% | 0.384 |





























