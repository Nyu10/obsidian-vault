

Lockedin AI: A comprehensive interview co-pilot that integrates technical depth (DSA) with high-level system design [09:13]. 

Interview Sidekick: Focuses on communication skills by analyzing verbal delivery, tone, and confidence during mock interviews [08:24]. 

Final Round AI: Specifically designed for behavioral interview prep, helping users structure stories using the STAR method [07:23]. 

**Pramp** 

Interviewing.io

**Daily minimum (forever):**
- **30 min LeetCode**
- **30 min System Design**


January and February 
### Months 1-2: The "Pattern Flush" (Speed & Accuracy)

- **LeetCode:** Blast through the NeetCode 150. Since you've done 300, you should be doing **3–4 Mediums a day.** * **The Constraint:** Set a timer for 20 minutes. If it's not solved and bug-free, you failed the "Big Tech" bar. Record _why_ you were slow (e.g., "forgot how to use `PriorityQueue` comparator in Java").
    
- **System Design:** Finish Alex Xu Vol 1. Focus specifically on **Database Sharding** and **Consensus Algorithms (Paxos/Raft)**—Quants and High-Frequency Trading (HFT) firms care deeply about low-level consistency and latency.
### Months 3-4: The "Hard" Deep Dive

- **Target:** Top 50 "Hard" problems on LeetCode filtered by frequency.
### Months 5-6: The "Simulation" Phase
- **Mock Interviews:** You need to do 2 mocks a week.
- **System Design Vol 2:** Alex Xu’s second book covers more complex systems like **Distributed Message Queues** and **Google Maps.**



Competitive Programmer
**Resource:** Start doing **Codeforces** Div 2 contests once a week. It will hurt your ego, but it will make "Big Tech" interviews feel like child's play.


### Step 2: Pattern Sprints (This Is the Key)

Each day:
- Pick **ONE pattern**
- Do **3–5 problems** from that pattern
- **No more than 8 min per problem**

### Step 3: One-Line Pattern Anchors

For every problem, you write **one sentence**:
> “This is sliding window because we’re tracking a valid window under constraint X.”
Or

> “This is binary search on answer because feasibility is monotonic.”
This is **how competitive programmers think**.
Codeforces Div2 C/D (selectively)


### 🧠 Hard Protocol (15–30 min max)
- 10 min: Think
    
- 10 min: Read editorial
    
- 10 min: Re-derive _why_ it works
    
Write:
> “The trick is realizing X, which allows Y.”
If you can explain the trick, you’re winning


8-9 Devos
9-10 Leetcode
10-4 work 
4-5 system design
then whatever i have going on (gym life group)



anki cards 
**Front of Card (The "Trigger"):**

> [Problem Name] (e.g., "Sliding Window Maximum")
> 
> Constraints: $N = 10^5$, $k = 10^3$.
> 
> The Goal: Find max in every sliding window of size $k$.

**Back of Card (The "Blueprints"):**

- **Pattern:** Monotonic Queue (Deque).
    
- **The Trick:** "Maintain a deque of indices. Before adding a new element, pop all smaller elements from the back (they can never be the max). Pop from front if the index is out of the window $(i - k)$."
    
- **Complexity:** $O(N)$ time, $O(k)$ space.
    
- **Code Skeleton:** (Optional—just a 4-5 line snippet of the core logic).