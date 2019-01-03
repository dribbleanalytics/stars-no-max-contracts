# METHODOLOGY: Visualizing how much the top 10 NBA players would earn without max contracts

If the .ipynb titled "python-master-stars-no-max-contracts" does not open, try opening the "python-master-stars-no-max-contracts-NO-OUTPUTS" file. The original has all the outputs saved, making it a large file which GitHub sometimes can't open.

[Link to blog post.](https://dribbleanalytics.blogspot.com/2019/01/stars-no-max-contracts.html)

## Data collection

Basketball Reference collects yearly contract data for every NBA player currently under contract ([link](https://www.basketball-reference.com/contracts/players.html)). Using this data, I matched each player's 2018-19 salary to their 2017-18 [averages](https://www.basketball-reference.com/leagues/NBA_2018_per_game.html) and [advanced](https://basketball-reference.com/leagues/NBA_2018_advanced.html) stats.

## Data cleaning

After collecting all the data, I removed all players currently on first-round rookie contracts were removed. This is because their value is not decided by an open market, and rookie contracts are among the highest value contracts in the NBA (as stars like Ben Simmons get paid under $10 million per year).

For Gordon Hayward and Kawhi Leonard, I used 2016-17 data instead of 2017-18 data to adjust their 2018-19 salary because both players had major injuries last year. These injuries made their stats last year much different from their baseline. Though Hayward is unlikely to return to 2016-17 form, the contract was signed before his injury, so he is still being paid under the assumption of that performance.

Players on veteran minimums and MLEs were **not** removed. I considered removing them because these contracts create the same arbitrary barrier that max contracts create. However, I decided against it, as those players can often earn more than the veteran minimum or MLE.

## The top 10

I used [ESPN's 2018-19 top 10 NBA player rankings](http://www.espn.com/nba/story/_/id/24668720/nbarank-2018-19-1-10-best-players-season) as my top 10 players. The players were:

1. LeBron James
2. Stephen Curry
3. James Harden
4. Kevin Durant
5. Giannis Antetokounmpo (tied for 4th with KD)
6. Anthony Davis
7. Russell Westbrook
8. Kawhi Leonard
9. Joel Embiid
10. Damian Lillard

After limiting the dataset as mentioned above, I isolated these 10 players into their own dataset. This way, they can't influence the non-star dataset, and we can adjust their salary according to the entire non-star dataset.

All the regressions showing how each stat correlates to salary were done using the non-star dataset.

## Adjusting the salary

To see how much the top 10 players would make, I first adjusted their salary per metric to that of the non-star dataset. For example, I divided the sum of the non-star salary by the sum of the non-star VORP to find a $/VORP rate. I then multiplied this rate by each of the top 10 players' VORP to adjust their salary, thereby predicting how much they'd earn without max contracts.

In the case of PPG and WS, I used the median instead of the average. This is because for other metrics, there are both positive and negative values, and the statistic shows a big difference between the top players. However, all PPG is positive, and most players in the dataset had a positive WS (because of the exclusion of rookies). Therefore, to adjust the star salary to PPG and WS, I used the median in 3 different ways:

1. Sum of salary / sum of distance from median WS or PPG. In this case, if the median PPG was 10, then a player scoring 5 PPG and a player scoring 15 PPG would positively affect the average equally. This creates a sort of upper estimate.
2. Sum of salary / sum of (player WS or PPG - median WS or PPG) >=0. In this case, if the median PPG was 10, then a player scoring 5 PPG has a 0 in the sum; all values below the median were made 0. The player scoring 15 PPG has a 5 in the sum, as it did before. This creates less of an upper estimate.
3. Sum of salary / sum of (player WS or PPG - median WS or PPG). In this case, if the median PPG was 10, then a player scoring 5 PPG has a -5 in the sum. The player scoring 15 PPG still has a 5 in the sum. This truly adjusts the salary per PPG or WS to the median.
