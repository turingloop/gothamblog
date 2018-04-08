---
layout: post
title: Random function in C
author: Goutham
---

This code demonstrates how we can use time.h and srand to generate random number everytime you run the program.

{% highlight C %}
/*
	This program generates pi value using the probability of a random dart landing in the circle
	inscribed in a square. I am using srand() and time() to generate new seed value, so that
	everytime i call rand(), i get a new random number.
*/
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#include <time.h>

double estimatePi(int numThrows); //estimatePi Prototype

int main(){
	int numThrows[] = {10, 100, 1000, 5000, 10000, 50000, 100000, 500000, 1000000};//number of dart throws
	int numOfRuns = sizeof(numThrows)/sizeof(int); //number of elements in the array
	printf("Throws         Pi\n"); //print table column headers
	for(int j = 1;j<=22;j++) //print "=" 22 times
		printf("=");
	printf("\n");
	int i = 0; //iterative variable for array
	while(i < numOfRuns){
		estimatePi(numThrows[i]); //call estimatePi
		i++; //increment
	}
	return 0;
}

//this function estimates pi value by using square and inscribed circle, using random numbers.
double estimatePi(int numThrows){
	double x, y; //x, y coordinates
	int numDartsInCircle = 0; //variable to count number of darts in the circle
	double distance = 0; //distance
	double fractionOfDartsInCircle; //variable to store fraction of number of darts inside and outside circle.
	double pi; //calculate pi
	srand((unsigned int)time(NULL)); //changes seed everytime we call rand(), so we random number everytime we call rand()
	
	for(int i = 1; i <= numThrows; i++){
		x = (double)rand()/RAND_MAX * 2.0 - 1.0;//get random double value within -1 and 1 for x
		y = (double)rand()/RAND_MAX * 2.0 - 1.0;//get random double value within -1 and 1 for y
		distance = sqrt(pow(x,2)+pow(y,2));//calculate distance of the x-y coordiante from center.
		if(fabs(distance) <= 1.0){ //if absolute distance is less than 1.0, then the coordinate is in circle
			numDartsInCircle++;
		}
	}

	fractionOfDartsInCircle = (double)numDartsInCircle/(double)numThrows;//fraction of darts in circle
	pi = fractionOfDartsInCircle * 4; //pi value is calculated
	printf("%8d   %10.5f\n", numThrows, pi); //prints the number of throws and pi value generated.
	return 0.0;
}
{% endhighlight %}