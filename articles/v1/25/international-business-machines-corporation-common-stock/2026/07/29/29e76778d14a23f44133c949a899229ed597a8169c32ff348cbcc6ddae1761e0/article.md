---
schema_version: "1.0.0"
document_id: "29e76778d14a23f44133c949a899229ed597a8169c32ff348cbcc6ddae1761e0"
company_key: "international-business-machines-corporation-common-stock"
company: "International Business Machines Corporation"
source_id: "international-business-machines-corporation-common-stock-news-import-da8ebfcd6a45"
canonical_url: "https://research.ibm.com/blog/ponder-this-august-2026"
published_at: "2026-07-31T22:00:00+00:00"
first_seen_at: "2026-07-31T18:39:28.472942+00:00"
fetched_at: "2026-07-31T22:00:00.811652+00:00"
content_hash: "sha256:203cc8209b0dfdd09873d7c404ed881a73efed9c24a1f14a4a470b1ed5d53604"
---

# Ponder This Challenge - August 2026 - The Wheel of Buttons

## The Wheel of Buttons


This puzzle was suggested by Sanandan Swaminathan - thanks!


A game show is arranged as follows: The main attraction is a wheel that has NN


N


buttons equidistant from one another, such that the wheel looks perfectly symmetric.


Each button has two possible states: "on" and "off". There's no indicator to the status of a button, and each press toggles between those two states ("on" -> "off" -> "on" and so forth). At the beginning of the game, the buttons have random states with the one condition that not all buttons are "on".


On each turn, the wheel spins randomly without the contestant looking; the buttons are marked with 1,2,…1,2,\\ldots


1


,


2


,


…


beginning at the top-left button and going clockwise. The contestant picks a subset of the buttons and pushes them. After the contestant is done, two things can happen:


1. If all the buttons are "on", the contestant wins; this is made clear with balloons and confetti.
2. If not all the buttons are "on", a new round begins. Again, the wheel is spun without the contestant seeing it.


The contestant's goal is to find a strategy that guarantees winning in the least number of rounds, even if the game cheats and the spins of the wheel are not random.


The choices of the contestant can be described by a sequence of natural numbers. Rounds are separated by 0, and each round contains the numbers of the buttons pressed by the contestant.


For example, the following is an optimal solution for the case N=2N=2


N


=


2


:


If both buttons were "off" at the beginning, the game is won after the first round. Otherwise, after the second round both buttons have the same state, so after the third round, the contestant has certainly won the game.


Summing up the numbers in the solution, we arrive at the value 77


7


. If, in the middle round, we would have used 2 instead of 1, we'd reach the value 88


8


. We are interested in the minimum value that can be obtained from an optimal solution: We call this the "optimal solution sum" for NN


N


.


**Your goal** : Find the optimal solution sum for N=8N=8


N


=


8


.


**A bonus** "*" will be given for finding the optimal solution sum for N=64N=64


N


=


64


.
