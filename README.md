# 2025 IMC Prosperity Challenge

I've always had interest in financial markets, but this was my first time learning to algo trade. I saw an advertisement for IMC Prosperity 3 on LinkedIn, so I immediately seized the opportunity.

I want to start off by giving credit where credit is due. I heavily relied on these resources for testing:
- https://github.com/jmerle/imc-prosperity-3-submitter
- https://github.com/jmerle/imc-prosperity-3-visualizer
- https://github.com/jmerle/imc-prosperity-3-backtester

Another resource I used was this:
- https://github.com/pe049395/IMC-Prosperity-2024

Because I am a completely new to algo trading, I decided to reverse engineer what successful algo traders have done in the past. My code is an extension/modification of the code shared above. 

# Phases

## Tutorial
<details>
  <summary>
    <span style="font-size:15px">Click here for round details</span>
    <hr/>
  </summary>
  
    In the tutorial round there are two tradable goods: `Rainforest Resin` and `Kelp`. While the value of the `Rainforest Resin` has been stable throughout the history of the archipelago, the value of `Kelp` has been going up and down over time. 

    Position limits for the newly introduced products:

    - `RAINFOREST_RESIN`: 50
    - `KELP`: 50

    ⚠️ All algorithms uploaded in the tutorial round will be processed and generate results instantly, so you can experiment with different programs and strategies.
</details>

I didn't really test much during this phase. Most of my time was spent reading the Notion wiki. I submitted a file or two to get used to the file upload feature and was researching general market making strategies.

## Round 1
<details>
  <summary>
    <span style="font-size:15px">Click here for round details</span>
    <hr/>
  </summary>

    ## Algorithm challenge

    The first three tradable products are introduced: : `Rainforest Resin` , `Kelp`, and `Squid Ink`. The value of the `Rainforest Resin` has been stable throughout the history of the archipelago, the value of `Kelp` has been going up and down over time, and the value of `Squid Ink` can also swing a bit, but some say there is a pattern to be discovered in its prize progression. All algorithms uploaded in the tutorial round will be processed and generate results instantly, so you can experiment with different programs and strategies.

    Position limits for the newly introduced products:

    - `RAINFOREST_RESIN`: 50
    - `KELP`: 50
    - `SQUID_INK`: 50

    ### Hint

    Squid Ink can be a very volatile product with price having large swings. Making a two-sided market or carrying position can be risky for such an instrument. However, with large swings comes large reversion. Squid Ink prices show more tendency to revert short term swings in price.

    A metric to keep track of the size of deviation/swing from recent average could help in trading profitable positions.

    ## Manual challenge

    You get the chance to do a series of trades in some foreign island currencies. The first trade is a conversion of your SeaShells into a foreign currency, the last trade is a conversion from a foreign currency into SeaShells. Everything in between is up to you. Give some thought to what series of trades you would like to do, as there might be an opportunity to walk away with more shells than you arrived with.
</details>





I only had a few hours to submit my code for this one because of a busy schedule with school and work. I didn't discover the submitter, visualizer, or backtester resources mentioned above, so I just had to manually run and check on IMC's website which was really tedious. The manual challenge was straightforward and fun. I modeled the the forex table as a graph traversal problem where each node is a currency and each directed edge represents a trade with a multiplicative weight (conversion rate). I then ran DFS with a limit of 5 because we were looking for a cycle with 5 edges and found the optimal solution. Code shown below.

```
def dfs(graph, current, depth, product, path, max_result):
    if depth == 5:
        if current == "shell":
            if product > max_result["max"]:
                max_result["max"] = product
                max_result["path"] = path[:]
        return

    for neighbor, rate in graph[current].items():
        path.append(neighbor)
        dfs(graph, neighbor, depth + 1, product * rate, path, max_result)
        path.pop()


graph = {
    "snow":    {"snow": 1,    "pizza": 1.45, "nugget": 0.52, "shell": 0.72},
    "pizza":   {"snow": 0.7,  "pizza": 1,    "nugget": 0.31, "shell": 0.48},
    "nugget":  {"snow": 1.95, "pizza": 3.1,  "nugget": 1,    "shell": 1.49},
    "shell":   {"snow": 1.34, "pizza": 1.98, "nugget": 0.64, "shell": 1},
}

max_result = {"max": 0, "path": []}
dfs(graph, "shell", 0, 1.0, ["shell"], max_result)

print("Best product:", max_result["max"])
print("Best path:", " -> ".join(max_result["path"]))
```

My final answer was SeaShells --> Snowballs --> Silicon Nuggets --> Pizza --> Snowballs --> SeaShells

### After Round 1 Ranking
- Overall: #197
- Manual: #1409
- Algorithmic: #210
- Country (USA): #59
- Shells: 90,596

## Round 2

<details>
  <summary>
    <span style="font-size:15px">Click here for round details</span>
    <hr/>
  </summary>

    ## Algorithm challenge

    In this second round, you’ll find that everybody on the archipelago loves to picnic. Therefore, in addition to the products from round one, two Picnic Baskets are now available as a tradable good. 

    `PICNIC_BASKET1` contains three products: 

    1. Six (6) `CROISSANTS`
    2. Three (3) `JAMS`
    3. One (1) `DJEMBE`

    `PICNIC_BASKET2` contains just two products: 

    1. Four (4) `CROISSANTS`
    2. Two (2) `JAMS`

    Aside from the Picnic Baskets, you can now also trade the three products individually on the island exchange. 

    Position limits for the newly introduced products:

    - `CROISSANTS`: 250
    - `JAM`: 350
    - `DJEMBE`: 60
    - `PICNIC_BASKET1`: 60
    - `PICNIC_BASKET2`: 100

    ## Manual challenge

    Some shipping containers with valuables inside washed ashore. You get to choose a maximum of two containers to open and receive the valuable contents from. The first container you open is free of charge, but for the second one you will have to pay some SeaShells. Keep in mind that you are not the only one choosing containers and making a claim on its contents. You will have to split the spoils with all others that choose the same container. So, choose carefully. 

    Here's a breakdown of how your profit from a container will be computed:
    Every container has its **treasure multiplier** (up to 90) and number of **inhabitants** (up to 10) that will be choosing that particular container. The container’s total treasure is the product of the **base treasure** (10 000, same for all containers) and the container’s specific treasure multiplier. However, the resulting amount is then divided by the sum of the inhabitants that choose the same container and the percentage of opening this specific container of the total number of times a container has been opened (by all players). 

    For example, if **5 inhabitants** choose a container, and **this container was chosen** **10% of the total number of times a container has been opened** (by all players), the prize you get from that container will be divided by 15. After the division, **costs for opening a container** apply (if there are any), and profit is what remains.

</details>

I finally discovered the visualizer and backtester tools. I opened up the visualizer and was trying to analyze each timestamp but it was a bit unclear to me what was going on. The anonymized market trades and own_trades fields seemed inconsistent with the profit/loss and positions fields but maybe I'm missing something. Either way, the tool was useful to quickly test if any new logic would improve long term profits or not. The manual trading for this phase was interesting. I tried to model the situation with the following Python code:

```
choices_1 = [(10, 1), (80, 6), (37, 3), (90, 10), (31, 2), (17, 1), (20, 2), (73, 4), (50, 4), (89, 8)]
choices_2 = [(10, 1), (80, 6), (37, 3), (90, 10), (31, 2), (17, 1), (20, 2), (73, 4), (50, 4), (89, 8)]

output = {}

# Fixed percentage of total openings (10%)
fixed_percentage = 10

# If going for 2 choices
for choice_1 in choices_1:
    for choice_2 in choices_2:
        # Ensure we're not picking the same container twice
        if choice_1 != choice_2:
            # Calculate treasure from choice 1 with fixed percentage
            treasure_1 = (10000 * choice_1[0]) / (choice_1[1] + fixed_percentage)
            # Calculate treasure from choice 2 with fixed percentage
            treasure_2 = (10000 * choice_2[0]) / (choice_2[1] + fixed_percentage)
            # Total treasure after subtracting the 50,000 cost for second container
            y = treasure_1 + treasure_2 - 50000
            output[y] = f"{choice_1[0]}x {choice_1[1]} inhab and {choice_2[0]}x {choice_2[1]} inhab"


# If going for 1 choice
for choice_1 in choices_1:
    # Calculate treasure from choice 1 with fixed percentage
    treasure_1 = (10000 * choice_1[0]) / (choice_1[1] + fixed_percentage)
    output[treasure_1] = f"{choice_1[0]}x {choice_1[1]} inhab"

for key in sorted(output.keys(), reverse=True):
    print(f"Key: {key}, Value: {output[key]}")
```

My final answer was one choice: 73x multiplier with 4 inhabitants