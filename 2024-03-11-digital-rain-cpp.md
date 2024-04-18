---
layout: post
title: A Project in Modern C++
tags: cpp coding project
categories: demo
---

## Introduction

This is my digital Rain project for c++ module, it has an effect that can have rainging charactors on the screen,
Just like the Matrix Movie!!.With the power of C++ this can be achieved within VisualStudio and can print the Digtial Rain effect just like the movie. With the power of vectors filling Matrixes this can be achived and using the FillRand fucntion from the random number library we can fill the rainingCharactors array with random charactors to make it seem like the charactors are random in the matrix like the marrix movie.       

##Design & Test


To design the DigitalRain I used C++ programming langauge and use visaul studio IDE.
I used vectors to fill a 2d matrix that will fill the matrix with randomcharactors, to print the matrix before it could print the random charactors which is achived by using the random number library. In order to intialise the matrix I had to resize my matrix before filling it with raining charactors vector, so that it can print the matrix using my run function where I have for nested loops, looping through the collumns and rows by moving the charactors down, after moving the characters down I am using the fill rand function to fill the top row with new random characters, then I am going to use another nested for loop that first will print the collumns and rows to create a rain effect.

````/*
.------..------..------..------..------..------..------.     .------..------..------..------..------..------.
| S.--. || T.--. || E.--. || P.--. || H.--. || E.--. || N.--. | . - . | H.--. || A.--. || R.--. || N.--. || E.--. || Y.--. |
| : / \ : || : / \ : || (\ / ) || : / \ : || : / \ : || (\ / ) || : () : ((5)) | : / \ : || (\ / ) || : () : || : () : || (\ / ) || (\ / ) |
| : \ / : || (__) || : \ / : || (__) || (__) || : \ / : || ()() | '-.-.| (__) || :\/: || ()() || ()() || :\/: || :\/: |
| '--'S || '--'T || '--'E || '--'P || '--'H || '--'E || '--'N | ((1)) '--'H || '--'A || '--'R || '--'N || '--'E || '--'Y |
`----- - '`------'`----- - '`------'`----- - '`------'`----- - '  ' - '`------'`----- - '`------'`----- - '`------'`----- - '

Stephen Harney
19/02/2024


*/ 
#include <iostream>
#include <string>
#include <vector>
#include <iterator> 
#include <random>


#include <windows.h>
#include "DigitalRain.h"

namespace DigitalRainR {

	int DigitalRain::rainCount = 0;

	int DigitalRain::GetRainCount() {
		return rainCount;

	}

	DigitalRain::DigitalRain() : rows{ 0 }, cols{ 0 }, rain{ "" }, Charactors{ 0 }, rainingCharactors{} {
#if VERBOSE
		std::cout << "Default constructor" << std::endl;
#endif
		rainCount++;
	}

	DigitalRain::DigitalRain(int r, int c) : rows{ r }, cols{ c }, Charactors{0} {
#if VERBOSE 
		std::cout << "Executing Matrix 2 argument constructor with rows & cols" << std::endl;
#endif 

		rainingCharactors.resize(r);
		for (auto i = rainingCharactors.begin(); i != rainingCharactors.end(); ++i) {
			i->resize(c);
			FillRand(*i); // Fill each row with random characters
			rainCount++;
		}
	}


	int DigitalRain::GetRows() const {

		return rows;

	}


	int DigitalRain::GetCols() const {

		return cols;


	}



	DigitalRain::~DigitalRain() {
#if VERBOSE
		std::cout << "Destructing Charactors" << std::endl;
#endif
		rainCount--;
	}


	DigitalRain& DigitalRain::operator=(const DigitalRain& d) {
	
		rain = d.rain;
		Charactors = d.Charactors;
		rainingCharactors = d.rainingCharactors;
		return *this;
	}


	
	void DigitalRain::Run() {
		while (true) {
			//loops through the rows and  and move the charactors down
			for (int j = 0; j < cols; j++) {
				for (int i = rows - 1; i > 0; i--) {
					rainingCharactors[i][j] = rainingCharactors[i - 1][j];
				}
			}
			// fills the rows and colloumns
			for (int j = 0; j < cols; j++) {
				FillRand(rainingCharactors[0]);
			}

			//loops through the rows and colloumns and int j =10 will print the collumn
			for (int j = 0 ; j < cols; j++) {
				Sleep(5);
		
				// int j = 1;
				
			
				for (int i = 0; i < rows; i++) {//This does not loop trough the collumns as when its not looping through the collousm inj=1; is run after the loop happpens for rows 
				Sleep(5);
			
					GotoXY(j,i);
					std::cout << rainingCharactors[i][j];
					
				}
				std::cout << std::endl;
			}

			Sleep(100); 
			system("CLS"); 
			std::system("COLOR 02"); 
		}
	}
	void DigitalRain::FillRand(std::vector<char>& v) {

		static std::default_random_engine e;
		static std::uniform_int_distribution <int> u(48, 122);

		for (auto& r : v)
			r = static_cast<char>(u(e));


	}

	void DigitalRain::GotoXY(int x, int y) {
		COORD coord;
		coord.X = x;
		coord.Y = y;
		SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE), coord);
		
	}

}

`````
As you can see in my DigitalRain.cpp file I am using the FillRand from the random number library to fill the matrixs with random characters as seen in the FillRand function and using the GotoXY windows console cursor I can print the matrix at certain poistions on the terminal.

             
##Algorithm 
Within my DigitalRain I am using the Run funtion to run my digitalrain effect.
-It uses nested for loops to loop through the columns and rows in order for it to print it in collumns like the matrix effect,
-I am also using another nested for loop to move the charcters down the matrix,
-Then I am using the FillRand function to fill the top row with random charactors after it moves the characters down a row,
-The last loop iterates through each collumn and row and prints the characters in collumns,
-I am using the GotoXY(j,i) to set the postions of the cursor at the desired location,
-``std::cout << rainingCharactors[i][j]; this prints the characters at the postions fo the GotoXY() cursor.
````
void DigitalRain::Run() {
		while (true) {
			//loops through the rows and  and move the charactors down
			for (int j = 0; j < cols; j++) {
				for (int i = rows - 1; i > 0; i--) {
					rainingCharactors[i][j] = rainingCharactors[i - 1][j];
				}
			}
			// fills the rows and colloumns
			for (int j = 0; j < cols; j++) {
				FillRand(rainingCharactors[0]);
			}

			//loops through the rows and colloumns and int j =10 will print the collumn
			for (int i = 0 ; i < rows; i++) {
				Sleep(5);
		
				// int j = 1;
				
			
				for (int j = 0; j < cols; j++) {//This does not loop trough the collumns as when its not looping through the collousm inj=1; is run after the loop happpens for rows 
				Sleep(5);
			
					GotoXY(j,i);
					std::cout << rainingCharactors[i][j];
					
				}
				std::cout << std::endl;
			}

			Sleep(100); 
			system("CLS"); 
			std::system("COLOR 02"); 
		}
	}
````

##Problem-solving

For this project I kept getting the same output grid which it was printing the random numbers from the top row after filling the matrix with raingingCharacters, but the problem was that I wasn't printing collumns which meant I was only looping through the rows and not the collumns. Thus when its not looping through the collumns first it will only loop through the rows and print it only in the grid which is as you can see here but once I changed backit back to print the collumns first than the rows it will print the collumns first. 
<img src="https://raw.githubusercontent.com/StephenHarney/digital-rain-cpp/main/docs/assets/RowsError.png" width="400" height="300">


##Modren C++



##References



