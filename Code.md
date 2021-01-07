# Application/Main class
Code:
```java
import java.util.*;

/*
 * Main (main method):
	Class = Main class.
	Responsibility = To start of the program by creating a Gamer object and calling the create method.
	Collaboration = Gamer class.
 */
public class Main {

	public static void main(String[] args) throws Exception {
		// TODO Auto-generated method stub
		Gamer g1 = new Gamer();//Creates a Gamer object (g1).
		g1.create();//Calls the method (create) on g1.
		boolean quit = false;//Initalizes a boolean object (quit) to false.
		int numOfSteps = 0;//Initalizes an int object (numOfSteps) to 0.
		do { //A do while loop.
			System.out.println("Please enter a command, q to quit the program and any other letter to move discs:");
//Tells the user to enter a command whether the user wants to continue or not.
			Scanner scanner = new Scanner(System.in);/*Creates a Scanner object
(scanner) with System.in.*/
			String input = scanner.next();/*Initalizes a String object (input)
and sets it to the user's input using scanner.*/
			char command = input.charAt(0);/*Initalizes a char object (command)
and sets it to the first letter of the String input.*/
			if (command == 'q') {/*Creates an if/else loop and checks if 
command is equal to q.*/
				quit = true;/*Sets quit to true so the program exits the do 
while loop.*/
				System.out.println("Thank you for playing Tower of Honai!");
//Thanks the user for playing TOH.
			} else {//Executes the below code if command is not equal to q.
				System.out.println("Please enter the Tower number you need to move the disc from:");
//Asks the user to enter the tower number they need the disc from.
				int towerFrom = scanner.nextInt();/*Initalizes an int object (towerFrom)
and sets it to the user's int input using scanner.*/
				System.out.println("Please enter the Tower number which you want to move your disc to:");
//Asks the user to enter the tower number they want to move the disc to.
				int towerRecieve = scanner.nextInt();/*Initalizes an int object
(TowerRecieve) and sets it to the user's int input using scanner.*/
				boolean result = false;/*Initalizes a boolean object (result)
and sets it to false.*/
				try {//Creates a try catch statement.
					result = g1.move(towerFrom, towerRecieve);/*Sets result
to the boolean value of method move of g1.*/
					System.out.println("Succesfully moved the discs!");
//Prints out the the discs have been succesfully moved.
					numOfSteps ++;//Increases the numOfSteps by 1.
				} catch (Exception e) {//Catches Exception e.					
					System.out.println(e.getMessage());/*Prints out Exception e's 
message.*/
				}
				if (result == true) {//Checks if result is equal to true.
					System.out.println("Congrats, you won the game by taking " + numOfSteps + " succesfull steps!");
//Tells the user they won the game by taking the int value of numOfSteps steps.
					break;//Breaks the do while loop.
				}
			}
		} while (quit == false);//Loops the do while loop while quit equals false.

	}

}

```
# Gamer class
Code:
```java
import java.util.*;

public class Gamer {
	ArrayList<Tower> towers = new ArrayList<Tower>();/*Creates an ArrayList of datatype Tower named 
towers.*/

	private void draw() {/*Creates a private method draw that has no inputs and doesn't have a 
return statement.*/
		for (int i = 0; i < towers.size(); i++) {/*Creates a for loop that repeats for the repeats
for the length of towers ArrayList.*/
			Tower t = towers.get(i);//Creates a Tower object (t) that gets int i of towers ArrayList.
			System.out.println(t.name + ":");//Prins out the tower name.
			t.draw();//Draws the tower using the draw method of Tower class.
		}
	}

	void create() throws Exception {/*Creates a method create that throws an exception if an 
exception is incurred. Also, create method creates the game layout.*/
		Tower t1 = new Tower("Tower 1");/*Creates a Tower object (t1) and since its a constructor
the String "Tower 1" is created with it.*/
		towers.add(t1);//Adds t1 to the towers ArrayList.
		Tower t2 = new Tower("Tower 2");/*Creates a Tower object (t2) and since its a constructor
the String "Tower 2" is created with it.*/
		towers.add(t2);//Adds t2 to the towers ArrayList.
		Tower t3 = new Tower("Tower 3");/*Creates a Tower object (t3) and since its a constructor
the String "Tower 3" is created with it.*/
		towers.add(t3);//Adds t3 to the towers ArrayList.
		DiscFactory df1 = new DiscFactory();//Creates a DiscFactory object (df1).
		Disc smallDisc = df1.getSmall();//Initalizes a Disc object (smallDisc) and sets it to df1.getSmall
		Disc largeDisc = df1.getLarge();//Initalizes a Disc object (smallDisc) and sets it to df1.getSmall
		Disc mediumDisc = df1.getMedium();//Initalizes a Disc object (smallDisc) and sets it to df1.getSmall
		t1.addDisc(largeDisc);//Adds largeDisc to t1.
		t1.addDisc(mediumDisc);//Adds mediumDisc to t1.
		t1.addDisc(smallDisc);//Adds smallDisc to t1.

		this.draw();//Calls Gamer's earlier method draw.
	}

	boolean move(int from, int to) throws Exception {/*Creates a method move that returns a boolean
value and takes in int inputs from and to. Also, it throws an expection if an excepton is incurred.*/
		Tower from1 = towers.get(from - 1);/*Initalizes a Tower object (from1) that is set to the value
of int from minus 1 in the towers ArrayList.*/
		Tower to1 = towers.get(to - 1);/*Initalizes a Tower object (to1) that is set to the value
of int to minus 1 in the towers ArrayList.*/
		Disc d1 = from1.removeDisc();/*Initalizes a Disc object (d1) that is set to the value of
removed disc from the from1 tower.*/ 
		try {//Creates a try catch statement.
			to1.addDisc(d1);//Adds Disc (d1) to the to1 tower.
		} catch (Exception e2) {//Catches Exception e2.
			from1.addDisc(d1);//Returns the Disc (d1) to the from1 tower.
			throw e2;//Throws e2 to Main/Application class.
		}
		this.draw();//Calls Gamer's earlier method draw.
		if (towers.get(1).getCount() == 3 || towers.get(2).getCount() == 3) {/*Checks if
the number of discs on tower 2 or 3 are equal to 3.*/
			return true;//Returns true.
		} else {
			return false;//Returns false.
		}
	}

}

```
# Tower class
Code:
```java
import java.util.*;

public class Tower {
	ArrayList<Disc> discKeeper = new ArrayList<Disc>();/*Creates an ArrayList of datatype
Disc and names it discKeeper.*/
	String name;//Creates a String object (name) in the class.

	Tower(String towerName) {/*Creates a constructor method of Tower that takes in the 
String towerName.*/
		this.name = towerName;/*Sets the inputted towerName to the value of String name in 
the class.*/ 
	}

	void drawTower() {//Creates a drawTower method.
		for (int i = 0; i < 4; i++) {//Creates a for method that repeats 4 times.
			System.out.println("    |");//Prints out "|".
		}
	}

	void draw() {//Creates a draw method.
		this.drawTower();//Calls the earlier method drawTower.
		for (int i = discKeeper.size() - 1; i >= 0; i--) {/*Creates a for loop
that repeats for the duration of discKeeper's size/length.*/
			Disc d = discKeeper.get(i);/*Initalizes a Disc object (d) and sets
it to the value of i of the discKepper ArrayList.*/
			d.draw();//Calls the draw method of Disc d.
		}
	}

	void addDisc(Disc d1) throws Exception {/*Creates an addDisc method that has no 
return statement and takes in an input of a Disc object. Also, it throws an
exception if its incurred.*/
		if (this.discKeeper.size() > 0) {/*Creates an if else satement that checks
if the discKeeper.size is greater than 0.*/
			int lastIndex = this.discKeeper.size() - 1;/*Initalizes an int object
(lastIndex) that is set to the value of the size of discKeeper ArrayList minus 1.*/
			Disc topDisc = this.discKeeper.get(lastIndex);/*Initalizes a Disc object
(topDisc) that is set to the Disc in the lastIndex of the discKeeper ArrayList.*/
			if (d1.size > topDisc.size) {/*Checks if the inputted disc for moving is 
larger than the topDisc of the tower the inputted disc is being added to.*/
				Exception e2 = new Exception("You cannot move the disc to this tower.");
/*Initalizes an Exception object (e2) with a message telling the user they cannot 
move the disc to this tower.*/
				throw e2;//Throws e2 to Gamer.
			}
		}
		this.discKeeper.add(d1);//Adds d1 to discKeeper ArrayList.
	}

	Disc removeDisc() throws Exception {/*Creates a removeDisc method that has
a return statement of datatype Disc, has no inputs, and throws an exception 
if its incurred.*/
		if (discKeeper.size() == 0) {//Checks if discKeeper ArrayList size is 0.
			Exception e = new Exception("The first tower has no discs so this move cannot be completed.");
/*Initalizes an Exception object (e) with a message telling the user the tower
which is having its disc(s) removed has 0 discs so that move cannot be completed.*/
			throw e;//Throws e to Gamer.
		}

		Disc d1 = this.discKeeper.remove(this.discKeeper.size() - 1);/*Creates a 
Disc object (d1) and sets it to the value of disc being removed.*/
		return d1;//Returns d1.
	}

	int getCount() {//Creates a method (getCount) that has a return statement of int.
		return discKeeper.size();//Returns the size of discKeeper.
	}
}

```
# Disc class
Code:
```java
/*
 * Disc:
	Class = Disc class.
	Responsibility = To print out the discs.
	Collaboration = None.
 */
public class Disc {
	int size;//Creates an int object (size).

	Disc(int size) {//Creates a constructor that takes in an int.
		this.size = size;/*Sets the inputted int to the value of the int size in
the class.*/
	}

	void draw() {//Creates a draw method.
		for (int i = 0; i <= size; i++) {/*Creates a for method that repeats
for the size of the disc.*/
			System.out.print("-");//Prints out "-".
		}
		System.out.println("");/*Prints out a blank line to prevent the next
print from being in the same line.*/
	}
}

```
# DiscFactory class
Code:
```java
public class DiscFactory {
	Disc getSmall() {/*Creates a getSmall method with a return statement of
datatype Disc.*/
		return new Disc(12);//Returns a disc of size 12.
	}

	Disc getMedium() {/*Creates a getMedium method with a return statement of
datatype Disc.*/
		return new Disc(18);//Returns a disc of size 18.
	}

	Disc getLarge() {/*Creates a getLarge method with a return statement of
datatype Disc.*/
		return new Disc(24);//Returns a disc of size 24.
	}
}

```
