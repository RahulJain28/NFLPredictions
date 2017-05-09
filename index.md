# Predicting NFL Plays
<img src="images\Cover.jpg">

### TEAM MEMBERS
* Keyur Muzumdar
* Aditya Kharosekar
* Rahul Jain

### INTRODUCTION
An important role of coaches, in any sport, is predicting what the opposing team will do. Nobody can predict it perfectly, but the best coaches make good adjustments. Coaches and players watch film to try and understand certain tendencies, and our objective is to take a data-driven approach in finding these. We know the obvious, “if you’re down by 2 you’ll kick a field goal,” but understanding smaller details about coaches, players, and the game itself can yield a competitive edge. A popular example of analytics-driven strategy is in the recent popularity of three-point shooting in the NBA. Over the past ten years, NBA general managers have opted to add more three-point shooters as data supports higher productivity at a high field goal rate. This is seen in recently successful teams such as the Golden State Warriors and Cleveland Cavaliers--both try to surround their superstars with excellent perimeter shooters. Similarly, we feel there is a lot of work to be done in data-driven strategy in the NFL.

### DATA COLLECTION
We used a Kaggle dataset, Detailed NFL Play-by-Play Data 2015. This dataset contained 63 features on 46,129 plays in the 2015-16 NFL season. Examples of some features present in this dataset are : 
* Ydstogo - how many yards until the first down marker
* DefensiveTeam - initials of the team who is on defense for that particular possession
* SideOfField - initials of the team whose side of the field the ball is at the moment
* PlayType - A one-word keyword about the type of play (Pass, Run, Punt, Kickoff, etc…)


### EXPLORATORY DATA ANALYSIS

The 63 features present in the dataset could broadly be divided into two categories - 
* Situational - These provide information (metadata) about the play
   * Some examples - Quarter, SideOfField, Time
* Result - These provide information about the play itself
   * Some examples - PlayType, PassLocation, Tackler1, RunGap

As our project focused on predicting offensive plays, there were several types of plays which were not relevant to us, such as Two-Minute Warning or End of Quarter. We decided to focus on the following plays - Pass, Run, Punt, Kickoff, Onside Kick, Field Goal, and QB Kneel. After removing the other plays, we were left with 39090 plays.The distribution of these selected play types was as follows - 

<img src="images\PlayTypes.png">

### FEATURE ENGINEERING
A team’s offensive play-calls depend on where on the field the offense is. If a team is barely in field-goal range, it would think twice before calling a slow-developing pass play, as a sack would push them out of scoring range. Similarly, if it is third down and a team is close to getting in field goal range, it might prioritize gaining those few yards over getting into the endzone. To include these different situations, we added a new column called FieldGoalRange which could take one of the following values - 
1. if a team was in field goal range (inside the opponents 37 yard line)
2. if a team was close to field goal range (within 5 yards of field goal range)
3. otherwise

Offensive strategy also depends on how many yards a team has to gain to win a first down. For example, a team is more likely to pass if it is 3rd-and-8 than when it is 3rd-and-2. Therefore, we added a column called DistanceToFirst which took values according the following rules - 
1. if distance to first down is <=2 yards
2. if distance to first down is 3-7 yards
3. if distance to first down is >8 yards

Each team’s offensive philosophy also plays a large part in determining what they do in any given situation. To account for these differences, we incorporated each team’s passing tendencies with two features. One feature indicated the percentage that each team passed the ball out of all of its offensive plays for the previous (2014-15) season. The second feature used the test set to calculate the same percentage for the current (2015-2016) season and added it as a feature.
We added two more features - a binary feature indicating if the team in possession were on their own side of the field, and an interaction feature between the side of the field and the 50 yard value.

### MODEL SELECTION AND ANALYSIS
##### Baseline Models
We calculated two baseline models - a proportional model, and an Air-Raid model. The proportional model worked as follows - We divided the dataset into a train set which consisted of 65% of the instances and a test set which consisted of the other 35%. We calculated the proportion of each play in the train set and randomly predicted each play in the test set based on these proportions. For example - consider a model focused on predicting only between pass and run plays. If the train set consisted of 65% passes and 35% runs, we would randomly generate a number between 0 and 1 for each play in the test set. If the number was less than 0.65, we would predict pass and we would predict run otherwise. Over multiple runs, this model was around 36% accurate. In the Air-Raid model, we predicted pass for each play, giving us an accuracy of 49.7% in a ten-fold cross validation. This is reasonable as the dataset contained about 50% pass plays.

##### Ridge Model
We tested a standard ridge classifier for our first model to test accuracy with stronger regularization, and achieved 60.9 ± .399% accuracy in a ten-fold cross validation. From the classifications, we also saw that a majority of mistakes came from misclassifying runs and passes.

##### K-Nearest Neighbors
Our intuition behind using a KNN model was to attempt to find the “closest” situation that we can in the data set and assume that offenses act the same when put in similar situations. Our best model achieved an accuracy of 52.3% in a ten-fold CV with K set to 3. This model does not perform well because KNN gives each feature equal weighting when finding the closest situation, neutralizing the effects of important features such as Time and Down. 

##### Multilayer Perceptron (MLP) Classifier
We tried an MLP classifier and achieved 61.0 ± 2.00% accuracy in a ten-fold cross validation with a ReLu activation and the Broyden–Fletcher–Goldfarb–Shanno (BFGS) algorithm, an iterative quasi-Newton method to approximate the nonlinear weights, and also tried using  stochastic gradient descent. The BFGS weighting (default for the classifier used) yielded better results, but ultimately we still had a relatively low accuracy likely due to overfitting. Because of feature importances on team-specific information, as well as very limited observations for several play types, using a neural network was not too appropriate. Tuning on more data would be an interesting approach, as well as considering an LSTM for previous plays.

##### Random Forests
We tried a Random Forest model and achieved 71.35 ± 0.55% accuracy with 10-fold cross validation. From the confusion matrix, we can see that a substantial amount of the error from this model came from mixing up run and pass plays. The model was successful in predicting other types of play with one notable exception - this model predicted a lot more onside kicks than actually happened. This probably occurred because there weren’t many onside kicks in our dataset.
<img src="images\RFFI.png">
<img src="images\RFCM.png">

##### XGBoost
Our XGBoost performed the best, giving us an accuracy of 74.35 ± .353% in a ten-fold cross validation. Similar to Random Forests, much of the error came from misclassifying runs as passes and vice-versa. However, XGBoost did a much better job in classifying kickoffs and onside kicks.
<img src="images\XGBoostFI.png">
<img src="images\XGBoostCM.png">

### RESULTS 
A qualitative validation for the models came from the feature importances-- in both the top five included time left, the score differential, and field position. These are consistent with how we, as football fans, would evaluate the situation as well. Moreover, we saw that two of the features we added-- each team’s previous season pass ratio and pass ratio up to that point in the season, were also significant predictors. 

<img src="images\Performance.png">

### FUTURE WORK
To improve on the current model, we may look into finding data on the formation of the offense. The formation the offense is lined up in can give insight into the type of play the offense will run next. For example, extra linemen may indicate a higher likelihood for a run play. Another thing to consider is the actual players on the field for a given play. Including player specific attributes, such as QB Rating or Yards per Carry could help identify key players that the offense is more likely to utilize. Lastly, an interesting addition would be to identify whether or not the game clock is running so that we can properly predict spike plays. 

