# March-Madness-Prediction
This code looks to test data over the course of previous years of march madness to determine how well each team will do in the post season. 
There are 5 different equations that were tested, with the basic variables and the important variables performing the best.
All of these equations will be posted to see what coparison is needed to be made. 

## Introduction 
This project seeks to determine which regular-season factors can predict team success for Division I Men’s basketball teams—specifically related to NCAA March Madness tournament outcomes.

## Objectives 
The goals and objectives for the project include:
- Determine the effect of regular season statistics on postseason outcomes
- Determine patterns of upsets in March Madness. Is it truly random, or is there a pattern to underdogs winning?
- Compare regular season outcomes with postseason outcomes. Do they have the same trends?
- Develop a model to predict team outcomes for new seasons, regular and postseason
- Compare tournament outcomes across different seasons

## Requirements
R 4.2.3

## Data
Download the data sets by Andrew Sundberg, which is updated yearly. <br>
If the files do not work, use the following link: https://www.kaggle.com/datasets/andrewsundberg/college-basketball-dataset?select=cbb13.csv <br>
Note: You will have to create a seperate data for only playoff teams with numeric values for each round if you do this.

The original dataset contains 23 variables on over 300 Division I Men’s basketball teams. It contains data for the years 2013-2023, and it is updated each year after the tournament. 

## Download Steps 
1. Check to make sure you have the right version of R installed
2. Navigate to the "Code" folder to download and run the "Set-up" file
3. Download and run the "Example for EDA" if interested in looking at trends in the data
4. Download and run "Formulas for Postseaosn" file and "Formulas for Wins" file, depending on desired response variable
5. Compare model results with official March Madness results (located in the "Data" folder)

## Variables and Code
model-The main data, looked at wins <br>
model1-The postseason data, looked at March Madness succeess

To run the code, you must first run the code in the Set-Up file in R. Run ALL of it for it to function properly. Make sure you have both the R code and the data you have downloaded in the same file. If you do not, the code will not work and it will return an error. Run it in the order it is set up in the file, or it will not function properly.

The following Code was used to find the first formulas:
```
model = lm(W ~ G+ADJDE+ADJOE+BARTHAG+EFG_O+EFG_D+TOR+TORD+ORB+DRB+FTR+
             FTRD+THREEP_O+THREEP_D+TWOP_O+TWOP_D+ADJ_T+WAB, data = data)
model1 = lm(POSTSEASON ~ SEED+W+ADJDE+ADJOE+BARTHAG+EFG_O+EFG_D+TOR+TORD+ORB+DRB+FTR+
             FTRD+THREEP_O+THREEP_D+TWOP_O+TWOP_D+ADJ_T+WAB+CONF, data = data1)
set.seed(365)
test_id <- sample(1:nrow(data), size=round(0.20*nrow(data)))
Test <- data1[test_id,]
Train <- data1[-test_id,]
forest= randomForest(POSTSEASON ~ SEED+W+ADJDE+ADJOE+BARTHAG+EFG_O+EFG_D+TOR+TORD+ORB+DRB+FTR+
             FTRD+THREEP_O+THREEP_D+TWOP_O+TWOP_D+ADJ_T+CONF, na.action = na.omit, data = data1, ntree=201, mtry=3)
importance(forest) %>% as.data.frame() %>% 
  rownames_to_column() %>% 
  arrange(desc(IncNodePurity))

model.backward=step(model1, direction="backward")
summary(model1)
```

datatest0-4: The datasets relating to the formulas regarding March Madness and post season success. <br>
datatest5: The dataset relating to the formula regarding seeding in March Madness. <br>
datapre0-4: The datasets relating to predicting regular season success. <br>

Example of how both would work.

```{r}
datatest=data1 %>% filter(YEAR == 2015) %>% mutate(guess=76.3730+1.4671*ADJDE-1.5632*ADJOE)
model4 = lm(POSTSEASON ~ ADJDE+ADJOE, data = datatest)
```

```{r}
datapre=data %>% filter(YEAR == 2021) %>% mutate(guess=5.21235-0.38954*ADJDE+0.49404*ADJOE)
pre = lm(W ~ ADJDE+ADJOE, data = datapre)
```
Note: You will have to changed the year for both of these every time you want to try a different year for different results.

It does not matter which way you do it from here. You can choose to look at wins or March Madness success, and do any of the following formulas at any way you choose. Just make sure you label the variables right, because they have only a one character differnece when doing so and change the year to look at different results.


## Formulas
Predicted Seed=2.45530+0.67538xADJDE-0.67537*ADJOE+12.36019xBARTHAG+0.19725xEFG_O+FTRDx0.05740-ADJ_Tx0.08455<br>
<br>
Predicted March Madness Success1=76.3730+1.4671xADJDE-1.5632xADJOE<br>(Related to only ADJOE and ADJDE) <br>
<br>
Predicted March Madness Success2=-10.4514+1.1880xSEED-1.8632xW+3.7798xADJDE-2.3714xADJOE+68.8196xBARTHAG+1.2750xTORD-1.3082xDRB+0.3124xFTR-1.4533xTWOP_D+2.248xWAB(Related to Important Variables)<br>
<br>
Predicted March Madness Success3=55.3479+0.3623xSEED-1.5262xW+WABx0.3918(Related to Basic Variables)<br>
<br>
Predicted March Madness Success4=205.0355-2.495xADJOE+1.8113xEFG_O-1.3013xTOR+ORBx0.1067+FTRx0.3631+0.5797xTHREEP_O-0.8719xTWOP_O+0.7859xADJ_T(Related to Offense)<br>
<br>
Predicted March Madness Success5=-138.3163+3.0282xADJDE+5.0483xEFG_D+1.54xTORD-DRBx1.4060+FTRDx0.3314-3.7031xTHREEP_D-4.884xTWOP_D(Related to Defense)<br>
<br>
Predicted Wins1=5.21235-0.38954xADJDE+0.49404xADJOE(Related to only ADJOE and ADJDE)<br>
<br>
Predicted Wins2=15.73771+0.59479xTHREEP_O-0.67821xTHREEP_D+0.71905xTWOP_O-0.69906xTWOP_D+0.13843xFTR-0.06921xFTRD(Related to Basic Variables) <br>
<br>
Predicted Wins3=-40.12261+0.199xADJOE+0.50192xEFG_O-0.74015xTOR+ORBx0.48263+0.12185xFTR+0.04667xTHREEP_O+0.33595xTWOP_O-ADJ_Tx0.17809(Related to Offense) <br>
<br>
Predicted Wins4=59.34784-ADJDEx0.16006-EFG_Dx1.16557+TORDx0.29898-DRBx0.19832-FTRDx0.11869+0.51144xTHREEP_D+TWOP_Dx0.48645 (Related to Defense) <br>
<br>
Predicted Wins5=1.373930+ADJDEx0.09432+BARTHAGx4.012412+EFG_Ox0.811507-EFG_Dx0.847656-TORx0.779007+TORDx0.786747+ORBx0.335041-DRBx0.292518+FTRx0.124129-FTRDx0.126843+ADJ_Tx0.055060(Related to Important Variables)

## How to Continue: Postseason
Once you have generated your dataset for the March Madness teams, you will noticed a guess variable at the end of the data. That is the variable that predicts what placement, or how far they will go, in the tournament. When two teams go against each other, you will choose the LOWER number of the two teams as the winner of the match. You will do this for all 63 games to complete the entire bracket. We have provided the actual March Madness Brackets, being Actual Results in powerpoint, for you to compare your bracket to the real thing. We typed the games we got wrong in red, so it is a good idea to make some marker to keep track of the games you had gotten incorrect.

## Results
ADJDE and ADJOE only:about 69% accurate <br>
<br>
Significant Variables:about  72% accurate <br>
<br>
Basic Variables:about 72% accurate <br>
<br>
Offense Only:about 64% accurate <br>
<br>
Defense Only:about 59% accurate <br>
<br>
Predicting wins varied heavily, though the most consistent and accurate was equation 2 regarding basic variables being 68-70% accurate over 10 years. <br>
Equation 1 ranged from 55-77%, being very inconsistent. <br>
Equation 3 was not as bad, but less accurate with 50% to 62%. <br>
Equation 4 had more wins than games played regularly, sometimes 10+ more, and was about 50% to 65% accurate. Also did not have enough wins for the great teams. <br>
Equation 5 had the opposite problem, having less wins than games played and sometimes going into the negatives, ranging from 64% to 70% accurate. This makes it the second most accurate and consistent of the equations. <br>

Brackets(both predicted and actual results) will be provided.

## Further Studies
Toutkoushian, E. (2011). Predicting March Madness: A statistical evaulation of the men’s NCAA basketball tournament (thesis). Ohio State University, Columbus. https://kb.osu.edu/handle/1811/488831 <br>
<br>
Bennett, N. (2018). Comparing Various Machine Learning Statistical Methods Using Variable Differentials to Predict College Basketball (thesis). University of Akron, Arkon. https://ideaexchange.uakron.edu/honors_research_projects/731/ <br>
<br>
Both supply other ways this project was approached and while not as accurate in some cases, give useful insights into other parts of the game of Basketball and supplies a huge help for our project.
