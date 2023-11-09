# March-Madness-Prediction
This code looks to test data over the course of previous years of march madness to determine how well each team will do in the post season. 
There are 5 different equations that were tested, with the basic variables and the important variables performing the best.
All of these equations will be posted to see what coparison is needed to be made. 

## Requirements
R 4.2.3

## Data
Download the data sets by Andrew Sundberg, which is updated yearly. <br>
If the files do not work, use the following link: https://www.kaggle.com/datasets/andrewsundberg/college-basketball-dataset?select=cbb13.csv <br>
Note: You will have to create a seperate data for only playoff teams with numeric values for each round if you do this.

## Variables and Code
model-The main data, looked at wins <br>
model1-The postseason data, looked at March Madness succeess

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

datatest0-5: The datasets relating to the formulas regarding March Madness and post season success. <br>
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


## Formulas
Predicted Seed=<br>
Predicted March Madness Success1=76.3730+1.4671*ADJDE-1.5632*ADJOE<br>(Related to only ADJOE and ADJDE)
Predicted March Madness Success2=-10.4514+1.1880*SEED-1.8632*W+3.7798*ADJDE-2.3714*ADJOE+68.8196*BARTHAG+1.2750*TORD-1.3082*DRB+0.3124*FTR-1.4533*TWOP_D+2.248*WAB(Related to Important Variables)<br>
Predicted March Madness Success3=55.3479+0.3623*SEED-1.5262*W+WAB*0.3918(Related to Basic Variables)<br>
Predicted March Madness Success4=205.0355-2.495*ADJOE+1.8113*EFG_O-1.3013*TOR+ORB*0.1067+FTR*0.3631+0.5797*THREEP_O-0.8719*TWOP_O+0.7859*ADJ_T(Related to Offense)<br>
Predicted March Madness Success5=-138.3163+3.0282*ADJDE+5.0483*EFG_D+1.54*TORD-DRB*1.4060+FTRD*0.3314-3.7031*THREEP_D-4.884*TWOP_D(Related to Defense)<br>
Predicted Win1s=5.21235-0.38954*ADJDE+0.49404*ADJOE(Related to only ADJOE and ADJDE)
Predicted Wins2=15.73771+0.59479*THREEP_O-0.67821*THREEP_D+0.71905*TWOP_O-0.69906*TWOP_D+0.13843*FTR-0.06921*FTRD(Related to Basic Variables) <br>
Predicted Wins3=-40.12261+0.199*ADJOE+0.50192*EFG_O-0.74015*TOR+ORB*0.48263+0.12185*FTR+0.04667*THREEP_O+0.33595*TWOP_O-ADJ_T*0.17809(Related to Offense) <br>
Predicted Wins4=88.63371-ADJDE*0.24796-EFG_D*1.17020+TORD*0.51659-DRB*0.37067-FTRD*0.0211+0.22497*THREEP_D+TWOP_D*0.26545 (Related to Defense) <br>
Predicted Wins5=1.373930+ADJDE*0.09432+BARTHAG*4.012412+EFG_O*0.811507-EFG_D*0.847656-TOR*0.779007+TORD*0.786747+ORB*0.335041-DRB*0.292518+FTR*0.124129-FTRD*0.126843+ADJ_T*0.055060(Related to Important Variables)
