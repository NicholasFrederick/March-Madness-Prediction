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
