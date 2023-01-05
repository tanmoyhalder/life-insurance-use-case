Author: Tanmoy Halder
Date: 04/01/2022

# This markdown describes the logics for data augmentation for the project

- Existing Data: Policy number, proposal received date, policy issue date, owner age, agent code, agent dob, agent doj, agent status

The below data were created based on author's best knowledge about the industry and publicly available blogs for US market. The author has tried to reduce bias and include hypothesis testable data for best possible modelling performance. 

The complete data preparation has been done in excel and the column references have been preserved for reproducibility. There is a data catalogue which also includes the data dictionary. The look up table is the reference table for creating most of the logic below. 


### Logic for premium range

- Step 1
The calculation is for 20 years term insurance for $500000 sum insured, (std premium)

=IF(AND(age<30,OR(gender="Female", gender="Others")),RANDBETWEEN([lookup_table.xlsx]premium calc sheet!$C$2,[lookup_table.xlsx]premium calc sheet!$D$2),
	IF(AND(age>=30,age<40,OR(gender="Female", gender="Others")),RANDBETWEEN([lookup_table.xlsx]premium calc sheet!$C$3,[lookup_table.xlsx]premium calc sheet!$D$3),
		IF(AND(age>=40,age<50,OR(gender="Female", gender="Others")),RANDBETWEEN([lookup_table.xlsx]premium calc sheet!$C$4,[lookup_table.xlsx]premium calc sheet!$D$4),
			IF(AND(age>=50,age<60,OR(gender="Female", gender="Others")),RANDBETWEEN([lookup_table.xlsx]premium calc sheet!$D$4,[lookup_table.xlsx]premium calc sheet!$C$5),
				IF(AND(age>=60,age<70,OR(gender="Female", gender="Others")),RANDBETWEEN([lookup_table.xlsx]premium calc sheet!$C$5,[lookup_table.xlsx]premium calc sheet!$D$5),
					IF(AND(age>=70,OR(gender="Female", gender="Others")),RANDBETWEEN([lookup_table.xlsx]premium calc sheet!$C$6,[lookup_table.xlsx]premium calc sheet!$D$6),
						IF(AND(age<30,OR(gender="Male", gender="Others")),RANDBETWEEN([lookup_table.xlsx]premium calc sheet!$C$7,[lookup_table.xlsx]premium calc sheet!$D$7),
							IF(AND(age>=30,age<40,OR(gender="Male", gender="Others")),RANDBETWEEN([lookup_table.xlsx]premium calc sheet!$C$8,[lookup_table.xlsx]premium calc sheet!$D$8),
								IF(AND(age>=40,age<50,OR(gender="Male", gender="Others")),RANDBETWEEN([lookup_table.xlsx]premium calc sheet!$C$9,[lookup_table.xlsx]premium calc sheet!$D$9),
									IF(AND(age>=50,age<60,OR(gender="Male", gender="Others")),RANDBETWEEN([lookup_table.xlsx]premium calc sheet!$D$9,[lookup_table.xlsx]premium calc sheet!$C$10),
										IF(AND(age>=60,age<70,OR(gender="Male", gender="Others")),RANDBETWEEN([lookup_table.xlsx]premium calc sheet!$C$10,[lookup_table.xlsx]premium calc sheet!$D$10),
											IF(AND(age>=70,OR(gender="Male", gender="Others")),RANDBETWEEN([lookup_table.xlsx]premium calc sheet!$C$11,[lookup_table.xlsx]premium calc sheet!$D$11)))))))))))))


- Step 2
Proportionate with policy term (eg. for 25 years policy premium = std premium/ 20 * 25)

- Step 3
Proportionate with smoker (eg. if smoker, premium = premium * 1.5)

- Step 4
Proportinate with medical (eg. if(AND(age > 30, medical = no), premium = premium*1.2, premium))


### Logic for family member

=IF(marital_status = "Single", 1, 
	IF(AND(marital_status = "Married", age >= 40), RANDBETWEEN(2,4), 
		IF(AND(marital_status = "Married", age < 40), RANDBETWEEN(2,3),  
			IF(AND(marital_status = "Divorced", age >= 40), RANDBETWEEN(2,3), 1))))
			

### Logic for medical

= IF(AND(age < 45 and smoker = "No"), "No", 
	IF(AND(age > 25, smoker = "Yes"), "Yes", 
		IF(age > 45, "Yes", "No")))
		
		
### Logic for number of nominee

= IF(OR(marital_status = "Single", marital_statis = "Widowed", 1, RANDBETWEEN(1,3))


### Logic for sum insured

RANDBETWEEN(250000, 1000000) --> proportionate by policy term and premium


### Logic for number of nominee

=IF(marital status = "Single", 1, 
	IF(AND(marital status = "Married", age < 35) , 1, 
		IF(AND( marital status = "Married", age >=35), RANDBETWEEN( 1,2), 
			IF(AND(marital status = "Widowed", age < 30), 1, RANDBETWEEN(1,3)))))
			
			
### Logic for occupation

=IF($K2 = "Lt High School",RANDBETWEEN(127,135),
	IF($K2 = "Graduate",RANDBETWEEN(136,148),
		IF($K2 = "Post Graduate",RANDBETWEEN(149,157),
			IF(AND($D2<=25,OR($E2="Female", $E2="Others")),RANDBETWEEN(1,36),
				IF(AND($D2>25,$D2<=65,OR($E2="Female", $E2="Others")),RANDBETWEEN(37,59), 
					IF(AND($D2 > 65, OR($E2="Female", $E2="Others")), RANDBETWEEN(109,121), 
						IF(AND($D2 <= 25, OR($E2="Male", $E2="Others")), RANDBETWEEN(60,88), 
							IF(AND($D2>25,$D2<=65, OR($E2="Male", $E2="Others")), RANDBETWEEN(89,108), 
								IF(AND($D2 > 65, $E2 = "Male"), RANDBETWEEN(122, 126))))))))))

- 1st adjustment --> If age < 22, "Student", if(and(age < 25, occupation = "doctor"), choose(Randbetween(1,7), lookup_table.xlsx!Table2!(IT service, Accountant, Sales, Construction, Other Service, Govt Service, Unemployed)), occupation)

- 2nd adjustment --> if (age > 68, "Retired", occupation)

													
### Logic for experience

=IF(OR($K2="Student",$K2="Housewife"),0,
	IF($K2="Retired",RANDBETWEEN(35,40),
		IF($J2="Graduate",($D2-21),
			IF($J2="Post Graduate",($D2-24),
				IF($J2="Lt High School",($D2-16),
					IF($J2="High School",($D2-18),
						IF($D2<30,RANDBETWEEN(3,6), 
							IF($D2 < 40, RANDBETWEEN(13,16), 
								IF($D2 < 50, RANDBETWEEN(23, 26), 
									IF($D2 < 65, RANDBETWEEN(33,36), ($D2 - 30)))))))))))


### Logic for Income

=IF($K2="Accountant",MAX('[lookup_table.xlsx]table 2'!$B$2,(RANDBETWEEN('[lookup_table.xlsx]table 2'!$B$2,'[lookup_table.xlsx]table 2'!$C$2)/'[lookup_table.xlsx]table 2'!$G$2*$L2)),
  IF($K2="Agricultural",MAX('[lookup_table.xlsx]table 2'!$B$3,(RANDBETWEEN('[lookup_table.xlsx]table 2'!$B$3,'[lookup_table.xlsx]table 2'!$C$3)/'[lookup_table.xlsx]table 2'!$G$3*$L2)),
  IF($K2="Businessman",MAX('[lookup_table.xlsx]table 2'!$B$4,(RANDBETWEEN('[lookup_table.xlsx]table 2'!$B$4,'[lookup_table.xlsx]table 2'!$C$4)/'[lookup_table.xlsx]table 2'!$G$4*$L2)),
  IF($K2="Construction",MAX('[lookup_table.xlsx]table 2'!$B$5,(RANDBETWEEN('[lookup_table.xlsx]table 2'!$B$5,'[lookup_table.xlsx]table 2'!$C$5)/'[lookup_table.xlsx]table 2'!$G$5*$L2)),
  IF($K2="Doctor",MAX('[lookup_table.xlsx]table 2'!$B$6,(RANDBETWEEN('[lookup_table.xlsx]table 2'!$B$6,'[lookup_table.xlsx]table 2'!$C$6)/'[lookup_table.xlsx]table 2'!$G$6*$L2)),
  IF($K2="Govt Service",MAX('[lookup_table.xlsx]table 2'!$B$7,(RANDBETWEEN('[lookup_table.xlsx]table 2'!$B$7,'[lookup_table.xlsx]table 2'!$C$7)/'[lookup_table.xlsx]table 2'!$G$7*$L2)),
  IF($K2="IT Service",MAX('[lookup_table.xlsx]table 2'!$B$9,(RANDBETWEEN('[lookup_table.xlsx]table 2'!$B$9,'[lookup_table.xlsx]table 2'!$C$9)/'[lookup_table.xlsx]table 2'!$G$9*$L2)),
  IF($K2="Lawyer",MAX('[lookup_table.xlsx]table 2'!$B$10,(RANDBETWEEN('[lookup_table.xlsx]table 2'!$B$10,'[lookup_table.xlsx]table 2'!$C$10)/'[lookup_table.xlsx]table 2'!$G$10*$L2)),
  IF($K2="Manager",MAX('[lookup_table.xlsx]table 2'!$B$11,(RANDBETWEEN('[lookup_table.xlsx]table 2'!$B$11,'[lookup_table.xlsx]table 2'!$C$11)/'[lookup_table.xlsx]table 2'!$G$11*$L2)),
  IF($K2="Manufacturing",MAX('[lookup_table.xlsx]table 2'!$B$12,(RANDBETWEEN('[lookup_table.xlsx]table 2'!$B$12,'[lookup_table.xlsx]table 2'!$C$12)/'[lookup_table.xlsx]table 2'!$G$12*$L2)),
  IF($K2="Military",MAX('[lookup_table.xlsx]table 2'!$B$13,(RANDBETWEEN('[lookup_table.xlsx]table 2'!$B$13,'[lookup_table.xlsx]table 2'!$C$13)/'[lookup_table.xlsx]table 2'!$G$13*$L2)),
  IF($K2="Other Engineering",MAX('[lookup_table.xlsx]table 2'!$B$14,(RANDBETWEEN('[lookup_table.xlsx]table 2'!$B$14,'[lookup_table.xlsx]table 2'!$C$14)/'[lookup_table.xlsx]table 2'!$G$14*$L2)),
  IF($K2="Other Service",MAX('[lookup_table.xlsx]table 2'!$B$15,(RANDBETWEEN('[lookup_table.xlsx]table 2'!$B$15,'[lookup_table.xlsx]table 2'!$C$15)/'[lookup_table.xlsx]table 2'!$G$15*$L2)),
  IF($K2="Professional",MAX('[lookup_table.xlsx]table 2'!$B$16,(RANDBETWEEN('[lookup_table.xlsx]table 2'!$B$16,'[lookup_table.xlsx]table 2'!$C$16)/'[lookup_table.xlsx]table 2'!$G$16*$L2)),
  IF($K2="Retired",MAX('[lookup_table.xlsx]table 2'!$B$17,(RANDBETWEEN('[lookup_table.xlsx]table 2'!$B$17,'[lookup_table.xlsx]table 2'!$C$17)/'[lookup_table.xlsx]table 2'!$G$17*$L2)),
  IF($K2="Sales",MAX('[lookup_table.xlsx]table 2'!$B$18,(RANDBETWEEN('[lookup_table.xlsx]table 2'!$B$18,'[lookup_table.xlsx]table 2'!$C$18)/'[lookup_table.xlsx]table 2'!$G$18*$L2)),
  IF($K2="Shop Owner",MAX('[lookup_table.xlsx]table 2'!$B$19,(RANDBETWEEN('[lookup_table.xlsx]table 2'!$B$19,'[lookup_table.xlsx]table 2'!$C$19)/'[lookup_table.xlsx]table 2'!$G$19*$L2)),
  IF($K2="Student",RANDBETWEEN('[lookup_table.xlsx]table 2'!$B$20,'[lookup_table.xlsx]table 2'!$C$20),
  IF($K2="Teacher",MAX('[lookup_table.xlsx]table 2'!$B$21,(RANDBETWEEN('[lookup_table.xlsx]table 2'!$B$21,'[lookup_table.xlsx]table 2'!$C$21)/'[lookup_table.xlsx]table 2'!$G$21*$L2)),
  IF($K2="Unemployed",RANDBETWEEN('[lookup_table.xlsx]table 2'!$B$22,'[lookup_table.xlsx]table 2'!$C$22),
  IF($K2="Housewife",0)))))))))))))))))))))

- Adjustment - 
=IF($X2> 0.2,$W2*RANDARRAY(1,1,5,20,TRUE),$M2)


### Logic for state and county

There are 33869 zipcodes present in USA. USed a random selection so that the state wise % of county counts remain same across policy holders.


### Logic for negative zip code

From the external data, used columns for creating the score for each zip code

- 100% below poverty (in %)
- income to poverty ratio less than 1 (in %)
- median age
- age 18-24 less than high school (in %)
- age 25+ less than high school (in %)
- number of workers
- median income

Normalised the values for each column. Added values of 1-5 columns and substracted 6th and 7th column to get the final score for each zip code. Hypothesis - Higher the score poor the economy.


### Logic for family medical history

=CHOOSE(RANDBETWEEN(1,4), '[lookup_table.xlsx]table 1'!$I$2,'[lookup_table.xlsx]table 1'!$I$3,'[lookup_table.xlsx]table 1'!$I$4,'[lookup_table.xlsx]table 1'!$I$5)


### Logic for existing policy

=CHOOSE(RANDBETWEEN(1,6), '[lookup_table.xlsx]table 1'!$H$2,'[lookup_table.xlsx]table 1'!$H$3,'[lookup_table.xlsx]table 1'!$H$4,'[lookup_table.xlsx]table 1'!$H$5, '[lookup_table.xlsx]table 1'!$H$6, '[lookup_table.xlsx]table 1'!$H$7)


### Logic for agent education

- First: dedup the agent_id and then use the below formula in the look up table and finally use vlook up 

=CHOOSE(RANDBETWEEN(1,12), '[lookup_table.xlsx]table 1'!$K$2,'[lookup_table.xlsx]table 1'!$K$3,'[lookup_table.xlsx]table 1'!$K$4,'[lookup_table.xlsx]table 1'!$K$5,'[lookup_table.xlsx]table 1'!$K$6,'[lookup_table.xlsx]table 1'!$K$7,'[lookup_table.xlsx]table 1'!$K$8,'[lookup_table.xlsx]table 1'!$K$9,'[lookup_table.xlsx]table 1'!$K$10,'[lookup_table.xlsx]table 1'!$K$11,'[lookup_table.xlsx]table 1'!$K$12,'[lookup_table.xlsx]table 1'!$K$13)

- Adjustment: If age < 21, chhose from Lt High School, High School or Some College


### Logic for agent age

Agent age is defined as agent age at the time of submitting the policy (proposal received date - agent dob)

There are DoBs which are in 1930 and practically impossible, Hence used randbetween(1970, 1998) where original DoB is < 1955
Also, replaced DoB yea 2000 and 2001 with 1990 and 1991 respectively


### Logic for AGent tenure

proposal_received_date - Agent DoJ


### Logic for agent_persistency

IF (AND(Policy Issue date - joining date) < 395,agent_status = "Inactive"), NA, IF(age < 35, RANDARRAY(1,1,.5,.9), RANDARRAY(1,1,.7,1))

- 395 = 365 days + 30 days grace period


### Logic for last 6 MOnths Submission

=IF(AND($AB2 = "Active", $AD2< 35), RANDBETWEEN(15,40), IF(AND($AB2 = "Active", $AD2 >= 35), RANDBETWEEN(40,60), IF($AB2 = "Inactive", -1)))


### Logic for Agent Reinstated

=CHOOSE(RANDBETWEEN(1,5), '[lookup_table.xlsx]table 1'!$L$2,'[lookup_table.xlsx]table 1'!$L$3,'[lookup_table.xlsx]table 1'!$L$4,'[lookup_table.xlsx]table 1'!$L$5,'[lookup_table.xlsx]table 1'!$L$6)


### Logic for previous persistency

=ROUND(IF($AI2= 0, -1,IF(AND($AD2 < 35,$AI2= 1), RANDARRAY(1,1,0.5,0.9), IF(AND($AD2 >=35,$AI2= 1),RANDARRAY(1,1,0.7,1)))),2)


### Logic for number of complaints

=IF(AND($AD2>=25, $AC2 = "Lt High School",  $AE2 > 730), RANDBETWEEN(10,30), IF(AND(AD$2 < 25, $AC2 = "Lt High School"), RANDBETWEEN(5,13), RANDBETWEEN(0,8)))
Adjustment: Create a pivot table with max number of complaints for each agent. Replace these numbers to actual table using vlookup for each agent


### Logic for target completion

=ROUND(IF(AND($AK2 > AVERAGE($AK$2:$AK$44949), $AH2 < PERCENTILE.INC($AH$2:$AH$44949,0.3), $AF2 < 0.6),RANDARRAY(1,1,0.6,0.8, FALSE),RANDARRAY(1,1,0.8,0.98, FALSE)),2)


### Logic for contacted in last 6 months

=CHOOSE(RANDBETWEEN(1,3), '[lookup_table.xlsx]table 1'!$O$2,'[lookup_table.xlsx]table 1'!$O$3,'[lookup_table.xlsx]table 1'!$O$4)


### Logic for Credit Score

=IF(AND($D2 < 25, OR($J2 = "Lt High School", $J2 = "High School")), RANDBETWEEN(600,700), IF(AND($D2 < 35, OR($J2 = "High School", $J2 = "Graduate", $J2 = "Some College")), RANDBETWEEN(700,800), IF(AND($D2 >= 35, OR($J2 = "Graduate", $J2 = "Post Graduate")), RANDBETWEEN(700,850), IF(AND($M2< MEDIAN($M$2:$M$44949), OR($J2 = "Lt High School", $J2 = "High School", $J2 = "Some College")), RANDBETWEEN(500, 670), IF(OR($J2= "Graduate", $J2 = "Post Graduate", $M2 > MEDIAN($M$2:$M$44949)), RANDBETWEEN(670,850), RANDBETWEEN(670,740))))))


### Logic for lapse (Target Variable)

Either of the below
- credit score < 550
- hasn't been contacted in last 6 months
- number of complaints > average complaints
- agent persistency < percentile(persistency, .3)
- negative zipcode = 1
- education = Lt high school, High school, college

Then

CHOOSE(RANDBETWEEN(1,10), [lookup_table.xlsx]table 1!$P$2,[lookup_table.xlsx]table 1!$P$3,[lookup_table.xlsx]table 1!$P$4,[lookup_table.xlsx]table 1!$P$5,[lookup_table.xlsx]table 1!$P$6,[lookup_table.xlsx]table 1!$P$7,[lookup_table.xlsx]table 1!$P$8,[lookup_table.xlsx]table 1!$P$9,[lookup_table.xlsx]table 1!$P$10,[lookup_table.xlsx]table 1!$P$11), CHOOSE(RANDBETWEEN(1,10), [lookup_table.xlsx]table 1!$Q$2,[lookup_table.xlsx]table 1!$Q$3,[lookup_table.xlsx]table 1!$Q$4,[lookup_table.xlsx]table 1!$Q$5,[lookup_table.xlsx]table 1!$Q$6,[lookup_table.xlsx]table 1!$Q$7,[lookup_table.xlsx]table 1!$Q$8,[lookup_table.xlsx]table 1!$Q$9,[lookup_table.xlsx]table 1!$Q$10,[lookup_table.xlsx]table 1!$Q$11))

The objective is to have a distribution of ~80% collection and 20% lapse. After a few trial and error, achieved the distribution.
Next: Replaced 1 with 0 and 0 with 1

- Note: We want to focus on lapse and that's why have kept target variable as lapse and not as collection. The modelling objective would be to maximise the recall (We want to correctly classify the lapses out of original lapses, since this would give actionable outcome)

## END OF REPORT