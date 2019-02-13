---
layout: post
title: Poker Simulator
categories: multiagentsystems
---

This software validates different poker card patterns and decides which poker card pattern has the highest value. I wrote this for a research project. This needs to be modularised and a lot of documentation needs to be added. But it works.

{% highlight JAVA %}

/*
Written by: Goutham Neravetla.
Copyrighted. If you need to use this source code, ask me for permission.
Email: gr@goutham.net
*/
import java.util.*;
import java.io.*;
import java.util.Collections;

public class setupgame{

	public static ArrayList<Integer> suitFrequency = new ArrayList<Integer>();
	public static ArrayList<Integer> valueFrequency = new ArrayList<Integer>();
	public static HashMap<String, Integer> hashCardToNumber = new HashMap<String, Integer>();
	public static HashMap<Integer, String> hashNumberToCard = new HashMap<Integer, String>();
	public static int p1Rank, p2Rank;

    public static void main(String[] args){


		defaultSuitValue();
        ArrayList<String> deck = new ArrayList<String>();
        ArrayList<String> p1 = new ArrayList<String>();
        ArrayList<String> p2 = new ArrayList<String>();
        ArrayList<Integer> p1SuitFrequency = new ArrayList<Integer>();
        ArrayList<Integer> p2SuitFrequency = new ArrayList<Integer>();
        ArrayList<Integer> p1ValueFrequency = new ArrayList<Integer>();
        ArrayList<Integer> p2ValueFrequency = new ArrayList<Integer>();
        initCardToNumber();
		initNumberToCard();
        String retString = new String();
        int randIndex;
		Random randnum = new Random();
		deck.addAll(Arrays.asList("sA","s2","s3","s4","s5","s6","s7","s8","s9","sT","sJ","sQ","sK","hA","h2","h3","h4","h5","h6","h7","h8","h9","hT","hJ","hQ","hK","dA","d2","d3","d4","d5","d6","d7","d8","d9","dT","dJ","dQ","dK","cA","c2","c3","c4","c5","c6","c7","c8","c9","cT","cJ","cQ","cK"));
		for(int i=0;i<5;i++){
			randIndex = randnum.nextInt(deck.size());
			p1.add(deck.get(randIndex));
			deck.remove(randIndex);
			randIndex = randnum.nextInt(deck.size());
			p2.add(deck.get(randIndex));
			deck.remove(randIndex);
			//System.out.println(retString + " " + randIndex);
		}


		for(int i=0;i<5;i++){
			updateValues(p1.get(i));
		}
		for(int i=0;i<4;i++){
			p1SuitFrequency.add(suitFrequency.get(i));
			//p1ValueFrequency.add(valueFrequency.get(i));
		}
		for(int i=0;i<13;i++){
			p1ValueFrequency.add(valueFrequency.get(i));
		}
		defaultSuitValue();
		for(int i=0;i<5;i++){
			updateValues(p2.get(i));
		}
		for(int i=0;i<4;i++){
			p2SuitFrequency.add(suitFrequency.get(i));
			//p2ValueFrequency.add(valueFrequency.get(i));
		}
		for(int i=0;i<13;i++){
			p2ValueFrequency.add(valueFrequency.get(i));
		}
		defaultSuitValue();

		p1Rank = getRank(p1SuitFrequency, p1ValueFrequency, p1);
		p2Rank = getRank(p2SuitFrequency, p2ValueFrequency, p2);

		System.out.println("Player1 cards: "+p1.get(0)+" "+p1.get(1)+" "+p1.get(2)+" "+p1.get(3)+" "+p1.get(4));
		//System.out.println("Player1 Suit Frequencies: "+"Clubs: "+p1SuitFrequency.get(0)+" Diamonds: "+p1SuitFrequency.get(1)+" Hearts: "+p1SuitFrequency.get(2)+" Spades: "+p1SuitFrequency.get(3));
		System.out.println("Player2 cards: "+p2.get(0)+" "+p2.get(1)+" "+p2.get(2)+" "+p2.get(3)+" "+p2.get(4));
		//System.out.println("Player2 Suit Frequencies: "+"Clubs: "+p2SuitFrequency.get(0)+" Diamonds: "+p2SuitFrequency.get(1)+" Hearts: "+p2SuitFrequency.get(2)+" Spades: "+p2SuitFrequency.get(3));
		System.out.println("Player1 Rank: "+p1Rank);
		System.out.println("Player2 Rank: "+p2Rank);
		if(p1Rank < p2Rank)
			System.out.println("Player 1 won!");
		else if(p1Rank > p2Rank)
			System.out.println("Player 2 won!");
		else
			if(p1Rank == 10 && p2Rank == 10){
				int p1HighCardRank, p2HighCardRank;
				p1HighCardRank = checkHighCard(p1SuitFrequency, p1ValueFrequency, p1);
				p2HighCardRank = checkHighCard(p2SuitFrequency, p2ValueFrequency, p2);
				if(p1HighCardRank == 1 && p2HighCardRank == 1)
					System.out.println("Player 1  and Player 2 has the high card, So it is a draw!");
				else if(p1HighCardRank == 1)
					System.out.println("Player1 has the high card, so Player 1 wins!");
				else if(p2HighCardRank == 1)
					System.out.println("Player2 has the high card, so Player2 wins!");
				else if(p1HighCardRank > p2HighCardRank)
					System.out.println("Player1 has the high card, so Player 1 wins!");
				else
					System.out.println("Player2 has the high card, so Player2 wins!");

			} else {
				System.out.println("It is a draw!");
			}
    }
    public static int getRank(ArrayList<Integer> playerSuitFrequency, ArrayList<Integer> playerValueFrequency, ArrayList<String> playerCards){
		if(checkRoyalFlush(playerSuitFrequency, playerValueFrequency, playerCards))
			return 1;
		else if(checkStraightFlush(playerSuitFrequency, playerValueFrequency, playerCards))
			return 2;
		else if(check4OfAKind(playerSuitFrequency, playerValueFrequency, playerCards))
			return 3;
		else if(checkFullHouse(playerSuitFrequency, playerValueFrequency, playerCards))
			return 4;
		else if(checkFlush(playerSuitFrequency, playerValueFrequency, playerCards))
			return 5;
		else if(checkStraight(playerSuitFrequency, playerValueFrequency, playerCards))
			return 6;
		else if(check3OfAKind(playerSuitFrequency, playerValueFrequency, playerCards))
			return 7;
		else if(check2Pair(playerSuitFrequency, playerValueFrequency, playerCards))
			return 8;
		else if(check1Pair(playerSuitFrequency, playerValueFrequency, playerCards))
			return 9;
		else
			return 10;

	}
    public static String getSuit(String retString){
		String returnSuit = Character.toString(retString.charAt(0));
		return returnSuit;
	}
	public static String getValue(String retString){
		String returnVal = Character.toString(retString.charAt(1));
		return returnVal;
	}
	public static void defaultSuitValue(){
		suitFrequency.clear();
		valueFrequency.clear();
		//suit order: 0->club,1->diamond,2->heart,3->spade
		suitFrequency.addAll(Arrays.asList(0, 0, 0, 0));
		valueFrequency.addAll(Arrays.asList(0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0));
	}
	public static void updateValues(String retString){
		String suit = getSuit(retString);
		String value = getValue(retString);
		switch(suit){
			case "s":	suitFrequency.set(3, suitFrequency.get(3)+1);
						break;
			case "d":	suitFrequency.set(1, suitFrequency.get(1)+1);
						break;
			case "c":	suitFrequency.set(0, suitFrequency.get(0)+1);
						break;
			case "h":	suitFrequency.set(2, suitFrequency.get(2)+1);
						break;
		}
		switch(value){
			case "A":	valueFrequency.set(0, valueFrequency.get(0)+1);
						break;
			case "2":	valueFrequency.set(1, valueFrequency.get(1)+1);
						break;
			case "3":	valueFrequency.set(2, valueFrequency.get(2)+1);
						break;
			case "4":	valueFrequency.set(3, valueFrequency.get(3)+1);
						break;
			case "5":	valueFrequency.set(4, valueFrequency.get(4)+1);
						break;
			case "6":	valueFrequency.set(5, valueFrequency.get(5)+1);
						break;
			case "7":	valueFrequency.set(6, valueFrequency.get(6)+1);
						break;
			case "8":	valueFrequency.set(7, valueFrequency.get(7)+1);
						break;
			case "9":	valueFrequency.set(8, valueFrequency.get(8)+1);
						break;
			case "T":	valueFrequency.set(9, valueFrequency.get(9)+1);
						break;
			case "J":	valueFrequency.set(10, valueFrequency.get(10)+1);
						break;
			case "Q":	valueFrequency.set(11, valueFrequency.get(11)+1);
						break;
			case "K":	valueFrequency.set(12, valueFrequency.get(12)+1);
						break;
		}
	}
	public static void initCardToNumber(){		
		hashCardToNumber.put("A", 1);
		hashCardToNumber.put("2", 2);
		hashCardToNumber.put("3", 3);
		hashCardToNumber.put("4", 4);
		hashCardToNumber.put("5", 5);
		hashCardToNumber.put("6", 6);
		hashCardToNumber.put("7", 7);
		hashCardToNumber.put("8", 8);
		hashCardToNumber.put("9", 9);
		hashCardToNumber.put("T", 10);
		hashCardToNumber.put("J", 11);
		hashCardToNumber.put("Q", 12);
		hashCardToNumber.put("K", 13);
	}
	public static void initNumberToCard(){
		hashNumberToCard.put(1, "A");
		hashNumberToCard.put(2, "2");
		hashNumberToCard.put(3, "3");
		hashNumberToCard.put(4, "4");
		hashNumberToCard.put(5, "5");
		hashNumberToCard.put(6, "6");
		hashNumberToCard.put(7, "7");
		hashNumberToCard.put(8, "8");
		hashNumberToCard.put(9, "9");
		hashNumberToCard.put(10, "T");
		hashNumberToCard.put(11, "J");
		hashNumberToCard.put(12, "Q");
		hashNumberToCard.put(13, "K");
	}
	public static int getCardToNumber(String card){
		int number = hashCardToNumber.get(card);
		return number;
	}
	public static String getNumberToCard(int number){
		String card = hashNumberToCard.get(number);
		return card;
	}
	//public static ArrayList<Integer>
	public static boolean checkRoyalFlush(ArrayList<Integer> playerSuitFrequency, ArrayList<Integer> playerValueFrequency, ArrayList<String> playerCards){
		if(playerSuitFrequency.get(0) == 5 || playerSuitFrequency.get(1) == 5 || playerSuitFrequency.get(2) == 5 || playerSuitFrequency.get(3) == 5){
			ArrayList<Integer> cardNumberRepresent = new ArrayList<Integer>();
			for(int i=0;i<5;i++){
				cardNumberRepresent.add(hashCardToNumber.get(getValue(playerCards.get(i))));
			}
			Collections.sort(cardNumberRepresent);
			if(cardNumberRepresent.get(0) == 1 && cardNumberRepresent.get(1) == 10 && cardNumberRepresent.get(2) == 11 && cardNumberRepresent.get(3) == 12 && cardNumberRepresent.get(4) == 13){
				return true;
			} else {
				return false;
			}
		}else {
				return false;
		}
	}
	public static boolean checkStraightFlush(ArrayList<Integer> playerSuitFrequency, ArrayList<Integer> playerValueFrequency, ArrayList<String> playerCards){
		if(playerSuitFrequency.get(0) == 5 || playerSuitFrequency.get(1) == 5 || playerSuitFrequency.get(2) == 5 || playerSuitFrequency.get(3) == 5){
			ArrayList<Integer> cardNumberRepresent = new ArrayList<Integer>();
			for(int i=0;i<5;i++){
				cardNumberRepresent.add(hashCardToNumber.get(getValue(playerCards.get(i))));
			}
			for(int i=0;i<5;i++){
				System.out.println(cardNumberRepresent.get(i));
			}
			Collections.sort(cardNumberRepresent);
			int firstCardNum = cardNumberRepresent.get(0);
			if(cardNumberRepresent.get(1) == firstCardNum+1 && cardNumberRepresent.get(2) == firstCardNum+2 && cardNumberRepresent.get(3) == firstCardNum+3 && cardNumberRepresent.get(4) == firstCardNum+4){
				return true;
			} else {
				return false;
			}
		} else {
			return false;
		}
	}
	public static boolean check4OfAKind(ArrayList<Integer> playerSuitFrequency, ArrayList<Integer> playerValueFrequency, ArrayList<String> playerCards){
		ArrayList<Integer> cardNumberRepresent = new ArrayList<Integer>();
		for(int i=0;i<5;i++){
			cardNumberRepresent.add(hashCardToNumber.get(getValue(playerCards.get(i))));
		}
		/*for(int i=0;i<5;i++){
			System.out.println(cardNumberRepresent.get(i));
		}*/
		Collections.sort(cardNumberRepresent);
		if((cardNumberRepresent.get(0) == cardNumberRepresent.get(1) && cardNumberRepresent.get(1) == cardNumberRepresent.get(2) && cardNumberRepresent.get(2) == cardNumberRepresent.get(3)) || (cardNumberRepresent.get(1) == cardNumberRepresent.get(2) && cardNumberRepresent.get(2) == cardNumberRepresent.get(3) && cardNumberRepresent.get(3) == cardNumberRepresent.get(4))){
			return true;
		} else {
			return false;
		}
	}
	public static boolean checkFullHouse(ArrayList<Integer> playerSuitFrequency, ArrayList<Integer> playerValueFrequency, ArrayList<String> playerCards) {
		ArrayList<Integer> cardNumberRepresent = new ArrayList<Integer>();
		for(int i=0;i<5;i++){
			cardNumberRepresent.add(hashCardToNumber.get(getValue(playerCards.get(i))));
		}
		Collections.sort(cardNumberRepresent);
		if(((cardNumberRepresent.get(0) == cardNumberRepresent.get(1)) && ((cardNumberRepresent.get(2) == cardNumberRepresent.get(3)) && (cardNumberRepresent.get(3) == cardNumberRepresent.get(4)))) || (((cardNumberRepresent.get(0) == cardNumberRepresent.get(1)) && (cardNumberRepresent.get(1) == cardNumberRepresent.get(2))) && (cardNumberRepresent.get(3) == cardNumberRepresent.get(4)))){
			return true;
		} else {
			return false;
		}
	}
	public static boolean checkFlush(ArrayList<Integer> playerSuitFrequency, ArrayList<Integer> playerValueFrequency, ArrayList<String> playerCards) {
		if(playerSuitFrequency.get(0) == 5 || playerSuitFrequency.get(1) == 5 || playerSuitFrequency.get(2) == 5 || playerSuitFrequency.get(3) == 5){
			return true;
		} else {
			return false;
		}
	}
	public static boolean checkStraight(ArrayList<Integer> playerSuitFrequency, ArrayList<Integer> playerValueFrequency, ArrayList<String> playerCards){
		ArrayList<Integer> cardNumberRepresent = new ArrayList<Integer>();
			for(int i=0;i<5;i++){
				cardNumberRepresent.add(hashCardToNumber.get(getValue(playerCards.get(i))));
			}
			Collections.sort(cardNumberRepresent);
			int firstCardNum = cardNumberRepresent.get(0);
			if(cardNumberRepresent.get(1) == firstCardNum+1 && cardNumberRepresent.get(2) == firstCardNum+2 && cardNumberRepresent.get(3) == firstCardNum+3 && cardNumberRepresent.get(4) == firstCardNum+4){
				return true;
			} else {
				return false;
			}
	}
	public static boolean check3OfAKind(ArrayList<Integer> playerSuitFrequency, ArrayList<Integer> playerValueFrequency, ArrayList<String> playerCards){
		ArrayList<Integer> cardNumberRepresent = new ArrayList<Integer>();
		for(int i=0;i<5;i++){
			cardNumberRepresent.add(hashCardToNumber.get(getValue(playerCards.get(i))));
		}
		Collections.sort(cardNumberRepresent);
		if(((cardNumberRepresent.get(0) == cardNumberRepresent.get(1)) && (cardNumberRepresent.get(1) == cardNumberRepresent.get(2))) || ((cardNumberRepresent.get(1) == cardNumberRepresent.get(2)) && (cardNumberRepresent.get(2) == cardNumberRepresent.get(3))) || ((cardNumberRepresent.get(2) == cardNumberRepresent.get(3)) && (cardNumberRepresent.get(3) == cardNumberRepresent.get(4)))){
			return true;
		} else {
			return false;
		}
	}
	public static boolean check2Pair(ArrayList<Integer> playerSuitFrequency, ArrayList<Integer> playerValueFrequency, ArrayList<String> playerCards){
		ArrayList<Integer> cardNumberRepresent = new ArrayList<Integer>();
		for(int i=0;i<5;i++){
			cardNumberRepresent.add(hashCardToNumber.get(getValue(playerCards.get(i))));
		}
		Collections.sort(cardNumberRepresent);
		if((cardNumberRepresent.get(0) == cardNumberRepresent.get(1) && cardNumberRepresent.get(2) == cardNumberRepresent.get(3)) || (cardNumberRepresent.get(0) == cardNumberRepresent.get(1) && cardNumberRepresent.get(3) == cardNumberRepresent.get(4)) || (cardNumberRepresent.get(1) == cardNumberRepresent.get(2) && cardNumberRepresent.get(3) == cardNumberRepresent.get(4))){
			return true;
		} else {
			return false;
		}
	}
	public static boolean check1Pair(ArrayList<Integer> playerSuitFrequency, ArrayList<Integer> playerValueFrequency, ArrayList<String> playerCards) {
		ArrayList<Integer> cardNumberRepresent = new ArrayList<Integer>();
		for(int i=0;i<5;i++){
			cardNumberRepresent.add(hashCardToNumber.get(getValue(playerCards.get(i))));
		}
		Collections.sort(cardNumberRepresent);
		if((cardNumberRepresent.get(0) == cardNumberRepresent.get(1)) || (cardNumberRepresent.get(1) == cardNumberRepresent.get(2)) || (cardNumberRepresent.get(2) == cardNumberRepresent.get(3)) || (cardNumberRepresent.get(3) == cardNumberRepresent.get(4))){
			return true;
		} else {
			return false;
		}
	}
	public static int checkHighCard(ArrayList<Integer> playerSuitFrequency, ArrayList<Integer> playerValueFrequency, ArrayList<String> playerCards){
		ArrayList<Integer> cardNumberRepresent = new ArrayList<Integer>();
		for(int i=0;i<5;i++){
			cardNumberRepresent.add(hashCardToNumber.get(getValue(playerCards.get(i))));
		}
		Collections.sort(cardNumberRepresent);
		if(cardNumberRepresent.get(0) == 1)
			return 1;
		else
			return cardNumberRepresent.get(4);
	}

}

{% endhighlight %}
