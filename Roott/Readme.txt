Task 1: 
To Plot hit position as a function of track position
	code: hitsvsextrapolated.root 
	result: ResultingPlots/HitPositionVsExpectedPosition.png	
	Approach: 
	-> The tree is obtained from the file angularScan/BTanalysis_831.root
	-> The variables are extracted, and a new to point is added to a graph for every entry
	-> This is done for both the sensors
	-> Both the graphs are drawn together
	
	Observation:
	-> most of the entries lie around the y=x line (as expected from a calibrated tracker)
	
Task 2: 
To plot the size of the cluster vs angle
	code: meanwidth.root, clustervsAngle.root
	result: ResultingPlots/ClusterWidthVsAngle.png
	Approach: 
	-> A macro meanwidth.root is made which gives the mean cluster width over both the sensors
	-> the width for each file in angularScan is copied and eneterd in the spreadsheet Angles.ods
	-> the colums of angles (from png) and mean cluster width is copied into a text file angle.txt
	-> The graph is plotted from clustervsAngle.root. 
	-> The graph is fitted for a quadric polynomial 
		(There was no motivation behind it other than how good the fit was (Chi^2<0.0005)
		At first I tried to fit with (1*(1-[H]tan(A))+2*([H]*tan(A)) = (1+[H]tan(A)). This was 			assuming most of the clusters are od size 1,2 and strips were each rectangles with a hit
		always when particle touched any strip, when path was at an angle to length of the strip.
		The length/wigth ratio is [H]
		But this did not result in a good fit. Generalizing it did give a better fit but now it 
		had the same degrees of freedom as quadratic polynomial and the latter gave an even better
		fit. So quadratic fit was used.
	meanwidth.root:
		-> The following logic is used for mean cluster width:
			If the size of the vector is s and the number of instances where the k-th entry 			is not 1 less than k+1-th entry is l then the mean cluster width (MCW) is s/(l+1)
			Eg: {5, 23, 24, 88} here s=4 and l=2  ({5,23},{24,88}) MCW=4/3
		-> in the macro, the total number of clusters(sum of l+1 over all entrie and both
		sensors) is calculated and so is the total number of entries. The MCW is the ratio of 
		these two quantities
		  
	Observation:
	-> MCW increases with increase in angle. this is expected as as angles is increased, particle has 		higher chance if hitting the boundry between 2 slits and hence has a higher chance of forming 
	clusters. 
Task 3:
To plot the residual
	code: residual.root
	result: ResultingPlots/Residual_831.png
	Approach:
	-> In run 831, For each sensor, we first obtain the data from the tree
	-> 2 histograms are made, one for each sensor. 
	-> for any vector, (ClusterPosition-ExpectedPosition) is filled into the respective histogram
	   in global coordinates, for all clusters. 
	Finding Cluster position:
	-> As we iterate through the elements of a vector, we do the following: 
		-> we define "mid" to be the index first element
		-> if the next element is in the cluster, we add 0.5 to mid
		-> if the next element is not in the cluster, we make an entry of the position
		   of last cluster, with middle element given by index mid (if mid is half integer,
		   we add 0.5 to element with index mid-0.5)
		Eg: {5, 23, 24, 88}
		Here the first entry is 5, then next is 23.5 (mid is increased by 0.5) and last is 88
		Hence the three clusters are entered
	-> Histograms are plotted 

	Observation: 
	->While sensor 1 is such that mean residual is almost 0, 
	  sensor 0 is at an offset of about 0.3 units.
	

Note: I could not see where the thresholdScan folder was used, So i left it as is. For task 1 and 3, only run 831 of angularScan was used.
































