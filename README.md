# **Dark Chess**

A Unity 3D souls-like game based on chess

## Table of Contents

- [**Dark Chess**](#dark-chess)
	- [Table of Contents](#table-of-contents)
	- [**Authors**](#authors)
	- [**Technologies Used**](#technologies-used)
	- [**Developer Notes**](#developer-notes)
		- [**Git Notes**](#git-notes)
	- [**Mechanics**](#mechanics)
		- [**Title Screen**](#title-screen)
		- [**Highlight Mechanic**](#highlight-mechanic)
			- [**How To Use**](#how-to-use)
			- [**How It Works**](#how-it-works)

## **Authors**

- **Justin Moore** - [Github](https://github.com/sirjust)
  
- **Arthur** - [Github](https://github.com/arthur-schevey)

- **Damian Dorosz** - [Github](https://github.com/exostin)

- **Hans** - [Github](https://github.com/unbekanntunity)
  
## **Technologies Used**

- Unity 2019.4.15f1
- ProBuilder 4.4.0

## **Developer Notes**

There is a dev folder and various folders beneath it. While in development, work within these folders, where you'll have complete freedom. When a scene or prefab is ready to be brought into the main project, submit a **Pull Request** with the asset in the correct folder.

Make many small **Pull Requests** rather than packing too much code into one branch.

We want every prefab or asset to be able to be used in any scene. If there are steps needed to get it working (initializing variables, etc), document them here under the [Mechanics](#mechanics) section.

We plan to demo all features weekly.

To keep our code homogenous, we'll use camel case for our variables (fields etc), like so: `private bool canAttack;`

Variables for lists and arrays should be pluralized, even if it makes the variable read awkwardly, like so: `List<string> attackTypes` or `IEnumerable<PlayerStatistics> playerStats`

Of particular note, we learned in Week 1 that due to how Unity works with `.meta` files, we should never commit an empty folder to source control. If we do, every other developer will have conflicts.

### **Git Notes**

We are using Github Projects to track issues, which we were previously calling tickets. The link is found [here](https://github.com/sirjust/DarkChess/issues). Each ticket is automatically given a number. When you start working on an issue, create a feature branch in git with the following syntax. You'll also need to assign yourself to the issue in the `Issues` tab.

``` git
git checkout -b 10-short-description-of-issue
git push -u origin 10-short-description-of-issue
```

At this point, the branch is both local and is tracked in your fork. You can then freely make changes to the branch and your `main` branch will be unaffected.

To keep your fork's `main` branch up to date with the central repository, initiate a pull request on Github that brings the current code on `sirjust:main` to `your-fork:main`. When that is complete, pull the changes into your local `main` branch and then merge the `main` branch into your feature branch. There are more streamlined ways to do this, but this is the simplest way.

We need to keep track of our issues, and close them when they are merged in. When we figure out how to close them automatically, we will implement that, but currently they sit there until we manually close them.

## **Mechanics**

### **Title Screen**

- GetVersion.cs - A script that pulls the current build version (We set that in Project Settings under Player tab) and puts it into a transparent text in the bottom left corner. It can for example indicate which version is a screenshot from.

### **Highlight Mechanic**

#### **How To Use**

1. Attach this script to the game object you want to highlight.

2. Create a highlight prefab and assign it to the game object like so. Keep clone empty.

![prefab image](https://i.ibb.co/FB33YT4/Screenshot-2020-12-05-141843.png)

#### **How It Works**

This script clones a prefab called `highlight` on top of the GameObject this script is attached to using the `OnMouseEnter()` event.

In this case, I am cloning a quad that is emissive (looks like a highlight).

![highlight image](https://i.ibb.co/6vX1CkF/Screenshot-2020-12-05-144732.png)

First I define my two variables.

```cs
public GameObject highlight;
public GameObject clone;
```

Here I am taking the position of the game object and assigning it to `objectPosition` with a position slightly above it. `0.02f`

I then `Instantiate` my highlight into the scene with the new `objectPosition` and set it equal to `clone` so I can destroy it later.

```cs
OnMouseEnter() {
    Vector3 objectPosition = new Vector3(transform.position.x, 0.02f, transform.position.z);
    clone = (GameObject)Instantiate(highlight, objectPosition, Quaternion.Euler(Vector3.right * 90));
}
```

Destroy clone when mouse isn't hovering over object anymore.

```cs
OnMouseExit() {
    Destroy(clone);
}
```

If I tried to destroy the original "highlight" it would attempt to delete the prefab from our assets which would not work.

### **Battle Menu**

#### **Components / sections**
The Battle menu contains several components which can be used individually until a specific point. Each component add a functionality to the Battle Menu

1. [**CharInfo**](#charInfo)
2. [**SkillInfo**](#skillInfo)
3. [**Health**](#health)
4. [**CardHolding**](#cardHolding)
5. [**Important Notes**](#importantNotes)

#### **charInfo**

##### **function**
display the character informations(name, picture, health, mana, strenght, critRate, dogdeRate, armor) of the last selected character in the scene

##### **How to use**
1. Attach `CharInfo.cs` to the game object you want to use as a display for the character informations
2. then assign the different components to the variables under the `Requiered` header 

![charInfo image](https://i.ibb.co/gT9Qh4D/CharInfo.png)

#### **skillInfo**

##### **function**
display the name and description of the last played card/skill in the scene

##### **How to use**
1. Attach `SkillInfo.cs` to the game object you want to use as a display for the skill  informations
2. then assign the different components to the variables under the `Requiered` header 

![skillInfo image](https://i.ibb.co/BfZhB15/Skill-Info.png)

#### **health**

##### **function**
display the players healthbar and manabar 

##### **How to use**
1. Attach `GetBarInfo.cs` to the game object you want to use as a display the character's health and mana
2. then assign the different components to the variables under the `Requiered` header  

![skillInfo image](https://i.ibb.co/s3Z64Fx/Health.png)

##### **cardHolding**

##### **function**
Used to draw, display and play cards/skills

Note: There is a difference between cards and card objects. 
- cards:  scriptableObject which are linked to a card object
-  card objects: the whole gameObject with additional game objects and scripts e.g UI elements or the `DragDrop.cs`  

![cardHolding image](https://i.ibb.co/qkcB0FD/Card-System.png)

#### **How to use**
1. Attach `CardSystem.cs` to the game object you want to use as container for the cards
2. then assign the different components to the variables under the `Requiered` header 

- `Starting Card Count`: specify the amount of cards which will be created at the beginning
- `Max Card Count`: specify the highest number of cards in the hand 
- `Y_start`: specify the y-coordinate of the instantiated card.
- `Gap`: the distance between every card on the hand
- `Selected Pos`: Specify the position which will add to the current Position of the Card, if the card is selected

#### **How it works**

##### **Card spawning**
Once the scene starts the `CardSystem.cs`  instatiates empty objects. These empty objects (`place` are saved in a array. Afterwards the script spawn a specific amount of `card objects` on the position of the empty objects in the array. In addition all card objects will receive a index which represent the index of the `place` which the cards are children of. These `place` object and the cards will be saved in a seperate array. 

Note: All posibile cards which can be played/drawed are saved in the scriptableObject of the player. 

![CardArrays Image](https://i.ibb.co/PxSJRR3/Card-Arrays.png)

##### **Drag and Drop**
In order to use the unity drag and drop functionality I have to import the different Interfaces( `IPointerDownHandler`, `IBeginDragHandler`, `IEndDragHandler`, `IDragHandler` ). Each Interface add a new method into the `DragDrop.cs` script which will triggered in the different stages in the drag and drop process. Now I can modify the different methods and add new functionalities in it. 

##### **Play a Card(visual aspect)**
Once the card object is moved the drag and drop process begin and the position of the card object will be save in a variable called `lastPos`. Since the card is always clicked and selected when moving it, I have to subtract the `SelectedPos` from the position

```cs
public void OnBeginDrag(PointerEventData eventData)
{
        lastPos = this.transform.position - selectedPos;
}

```

Now if the y-coordinate of the card object is higher than the variable `height UI`, a method named `PlayCard()` is triggered in the `CardSystem.cs`.  Otherwise the position of the card object will be reset to the `lastPos`. The `PlayCard()` method destroys the card and moves all other cards one position to the left. 
1. destroys the played card object
2. saves the next card object in a temporary variable called `old_cardObj`
3. destroys the next card object 
4. instatiated the next card object to position of the new place
5. decreased the index of the next card object 

```cs            
var old_cardObj = places[i].GetComponentInChildren<DragDrop>().gameObject;

Destroy(old_cardObj);

var new_cardObj = Instantiate(old_card.template, places[i - 1].transform);
```
This process goes through each card until all have been moved

##### **Use a card**
Play and use a card are two different things. To play a card the player has only to move the card object to a specific position. To use a card the player has also to select the right tiles and on the tiles has to be a object which can be detected e.g a other character object. 

-> AllSkills() mit einbringen und auch detecting system verwenden

##### **Draw a Card**
As soon as the player presses on the `DrawButton`, it is checked whether the maximum number of cards is exceeded or not. If this is not the case, then the `PickRandCard()` method is called. This takes an array of cards and randomly picks one. This card is then returned. The linked card object of the returned card will then instantiated in first free `place` object. 

Note: "Free" means that the object hasnt any card object as a children

##### **Select a Card**
The range of every card/skill are saved in the scriptableObject in a array. If the player only clicks on the card object and does not move, it will be selected. This selected card then will trigger a method called ` GenerateSkillTiles()`. This method read the saved relative positions in the array of the card/skill and add them to the current position of the user. After the calculations the method will instantiate the `highlight` object to the calculated object and save them into a other array called `skillrangeTiles`. This will be important for the Combat System

#### **Important notes**

##### ScriptableObject
- in this project work with scriptableObjects
- currently there are two types of scriptableObjects(Character, Card)
- ScriptableObject are containers for different values e.g health or mana cost
- These ScriptableObject also contains some other scripts

##### Player
- this variable has to contains a scriptable Object which was created with the `character.cs` 

##### Character objects
- game objects in the scene  which represents a character has to have the `GetStats.cs` script in order to use it with battle menu. In addition the game Object will receive stats, which will be necessary for the Combat System    
  
##### GridGenerator
- be sure that you only have one game Object in the scene which the `EditedGridGenerator.cs` script is attached to
