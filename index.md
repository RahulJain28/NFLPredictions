# Predicting NFL Plays
An exercise in existential contemplation of the pedagogical virtues of contemporary interpretations of Machiavelli and Nietzsche

<img src="PlayTypes.png">
### TEAM MEMBERS
Keyur Muzumdar
Aditya Kharosekar
Rahul Jain

### INTRODUCTION
An important role of coaches, in any sport, is predicting what the opposing team will do. Nobody can predict it perfectly, but the best coaches make good adjustments. Coaches and players watch film to try and understand certain tendencies, and our objective is to take a data-driven approach in finding these. We know the obvious, “if you’re down by 2 you’ll kick a field goal,” but understanding smaller details about coaches, players, and the game itself can yield a competitive edge. A popular example of analytics-driven strategy is in the recent popularity of three-point shooting in the NBA. (taking a break but I’ll  finish this later) 
### DATA COLLECTION
We used a Kaggle dataset, Detailed NFL Play-by-Play Data 2015. This dataset contained 63 features on 46,129 plays in the 2015-16 NFL season. Examples of some features present in this dataset are : 
Ydstogo - how many yards until the first down marker
DefensiveTeam - initials of the team who is on defense for that particular possession
SideOfField - initials of the team whose side of the field the ball is at the moment
PlayType - A one-word keyword about the type of play (Pass, Run, Punt, Kickoff, etc…)

In addition to this data, we incorporated each team’s passing tendencies with two features. One feature indicated the percentage that each team passed the ball out of all of its offensive plays for the previous (2014-15) season. The second feature used the test set to calculate the same percentage for the current (2015-2016) season and added it as a feature. This data is important because the type of plays any given team will run is heavily dependent on the team’s offensive philosophies. Some teams pass the ball more while others run more and we wanted our model to account for these differences across teams.

### EXPLORATORY DATA ANALYSIS
The 63 features present in the dataset could broadly be divided into two categories - 
Situational - These provide information (metadata) about the play
Some examples - Quarter, SideOfField, Time
Result - These provide information about the play itself
Some examples - PlayType, PassLocation, Tackler1, RunGap
As our project focused on predicting offensive plays, there were several types of plays which were not relevant to us, such as Two-Minute Warning or End of Quarter. We decided to focus on the following plays - Pass, Run, Punt, Kickoff, Onside Kick, Field Goal, and QB Kneel. After removing the other plays, we were left with 39090 plays.The distribution of these selected play types was as follows - 

### FEATURE ENGINEERING
Body

### MODEL SELECTION AND ANALYSIS
We calculated two baseline models - a proportional model, and an Air-Raid model. The proportional model worked as follows - We divided the dataset into a train set which consisted of 65% of the instances and a test set which consisted of the other 35%. We calculated the proportion of each play in the train set and randomly predicted each play in the test set based on these proportions. For example - consider a model focused on predicting only between pass and run plays. If the train set consisted of 65% passes and 35% runs, we would randomly generate a number between 0 and 1 for each play in the test set. If the number was less than 0.65, we would predict pass and we would predict run otherwise. Over multiple runs, this model was around 36% accurate.
In the Air-Raid model, we predicted pass for each play, giving us an accuracy of 51%. This is reasonable as the dataset contained about 51% pass plays.


### RESULTS 
Interesting summary (incl. Confusion matrices and what certain models were good/ bad at) 

### FUTURE WORK
Body

APPENDIX
A. Title of this section
Body 

B. Title of this section
Body

C. Title of this section
Body 

D. Title of this section
Body







