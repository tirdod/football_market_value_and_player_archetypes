# Football Player Market Value and Archetype Analysis

**Authors:** Tirdod Behbehani and Timothy Cassel

This project investigates two key questions:
- What statistical indicators best predict a player's market value?
- Can we use unsupervised learning to identify player archetypes?

## Objective

**Player Market Value:**
We aim to predict the market values of football players in the top five
European leagues from the 2019/20 through the 2022/23 seasons. 
We utilize various regression and penalization techniques such as Ordinary Least Squares,
LASSO, Ridge, Elastic Net, and Adaptive LASSO.

**Player Archetype Classification:**
We explore unsupervised learning techniques such as K-Means Clustering,
PCA, and Sparse PCA to assign players into different archetype buckets.

## Data
This project integrates two publicly available datasets from Kaggle, 
integrating FBref and Transfermarkt data:
- [Player Stats From Top European Football Leagues (FBref)](https://www.kaggle.com/datasets/beridzeg45/top-league-footballer-stats-2000-2023-seasons/data)  
- [Football Data from Transfermarkt](https://www.kaggle.com/datasets/davidcariboo/player-scores)

## Results

**Player Market Value**

Below is a summary of how our different models performed on our "Per 90s" dataset.

| Model               | RMSE | R² | BIC   |
|---------------------|------------|----------|-------------|
| OLS (log)           | 0.6462     | 0.7171   | 10473.238   |
| Ridge (log)         | 0.6602     | 0.7047   | -3532.034   |
| Lasso (log)         | 0.6462     | 0.7171   | -3797.360   |
| Elastic Net (log)   | 0.6462     | 0.7171   | -3797.441   |
| **Adaptive Lasso** (log) | **0.6460** | **0.7173** | **-3854.222** |

Below is a brief analysis of some of the notable coefficients. Covariate weight values are included within the parentheses.

- Age (1.37) and Age² (-1.88) have a concave relationship. Value rises with age up to a certain point, then declines. Upon further investigation, this "peak age" point is when a player is 27 years old.
- Playing time (0.14), goals (0.11), and xG (0.10) were strong positive predictors, emphasizing performance output. Defensive metrics like Tackles Won (-0.04) and Interceptions had weak or negative influence, reflecting market bias toward offensive output. This is further supported by being a Forward or Midfielder (both 0.11) positively influencing market value.
- Progressive Passes (0.12) and Successful Take-Ons (0.09) highlighted the value of creativity and ball progression.
- Player club (0.65) has a major impact on a player's market valuation. However, this is partly due to reputational and financial biases in Transfermarkt's valuations. When a young player from a small club moves to a big club, we observe the player's market value skyrocket, even in the absence of any performance change. This highlights the importance of incorporating both market value and on-pitch value in player analysis.

Intuitively, we see our results confirm how young, attacking players who receive suitable playing time are strong performance-based indicators of market value. 

While our models explain a sizable portion of variance (R² ≈ 0.71), there are many latent, unobserved variables such as agent influence, social media presence, player wages, or medical history that contribute the residual noise. Future research could improve predictive power by incorporating transfer history, wage data, or clustering position-specific player groups in the market value model.

**Player Archetype Classification**
While K-means clustering struggled, PCA and Sparse PCA showed promise in their ability to reduce dimensionality and separate players into archetypes. Sparse PCA was able to identify four easily interpretable principal components.

<table>
  <tr>
    <td valign="top" style="padding-right:2em; vertical-align:top;">
      <strong>Principal Component 1 (Playmaking)</strong>
      <table>
        <thead>
          <tr><th align="left">Variable</th><th align="right">Loading Score</th></tr>
        </thead>
        <tbody>
          <tr><td>key_passes</td><td align="right">0.6004</td></tr>
          <tr><td>progressive_carries</td><td align="right">0.5073</td></tr>
          <tr><td>successful_take_ons</td><td align="right">0.3998</td></tr>
          <tr><td>assist</td><td align="right">0.3371</td></tr>
          <tr><td>fouled</td><td align="right">0.2287</td></tr>
          <tr><td>progressive_passes</td><td align="right">0.1832</td></tr>
          <tr><td>expected_assist</td><td align="right">0.1480</td></tr>
          <tr><td>aerial_duels_won</td><td align="right">-0.0289</td></tr>
        </tbody>
      </table>
    </td>
    <td valign="top" style="padding-left:2em; vertical-align:top;">
      <strong>Principal Component 2 (Defending)</strong>
      <table>
        <thead>
          <tr><th align="left">Variable</th><th align="right">Loading Score</th></tr>
        </thead>
        <tbody>
          <tr><td>clearance</td><td align="right">-0.5350</td></tr>
          <tr><td>interception</td><td align="right">-0.4729</td></tr>
          <tr><td>blocks</td><td align="right">-0.3887</td></tr>
          <tr><td>tackles_won</td><td align="right">-0.3441</td></tr>
          <tr><td>playing_time_min</td><td align="right">-0.3412</td></tr>
          <tr><td>aerial_duels_won</td><td align="right">-0.2475</td></tr>
          <tr><td>error</td><td align="right">-0.1588</td></tr>
          <tr><td>progressive_passes</td><td align="right">-0.1332</td></tr>
        </tbody>
      </table>
    </td>
  </tr>
  <tr>
    <td valign="top" style="padding-right:2em; vertical-align:top;">
      <strong>Principal Component 3 (Goalscorers)</strong>
      <table>
        <thead>
          <tr><th align="left">Variable</th><th align="right">Loading Score</th></tr>
        </thead>
        <tbody>
          <tr><td>expected_goals</td><td align="right">0.7547</td></tr>
          <tr><td>standard_goals</td><td align="right">0.4849</td></tr>
          <tr><td>standard_pk</td><td align="right">0.4043</td></tr>
          <tr><td>is_Forward</td><td align="right">0.1367</td></tr>
          <tr><td>aerial_duels_won</td><td align="right">0.1100</td></tr>
          <tr><td>fouls_drawn</td><td align="right">0.0313</td></tr>
        </tbody>
      </table>
    </td>
    <td valign="top" style="padding-left:2em; vertical-align:top;">
      <strong>Principal Component 4 (Age)</strong>
      <table>
        <thead>
          <tr><th align="left">Variable</th><th align="right">Loading Score</th></tr>
        </thead>
        <tbody>
          <tr><td>age</td><td align="right">-0.7256</td></tr>
          <tr><td>age_squared</td><td align="right">-0.6881</td></tr>
        </tbody>
      </table>
    </td>
  </tr>
</table>

We can leverage the principal components and use the loading scores to derive player
archetypes. For instance:
- High PC1, High PC2, High PC3, High PC4: Creative, young forwards
- High PC1, Low PC2, Low PC3, High PC4: Creative, young midfielders
- Low PC1, Low PC2, Low PC3, Low PC4: Defensive veterans

As we can see, the interactions between the sparse principal components serve 
as a starting point for understanding a player’s role, style, and profile.

## Opportunities for Future Research
- Trying to predict a player’s market value by estimating the player’s on-field value,
then leveraging latent variable analysis to uncover hidden features which influence a player’s market value.
- Building separate models to predict market valuations for defenders, midfielders, and forwards,
leveraging insights from our Sparse PCA to guide parameter tuning.




