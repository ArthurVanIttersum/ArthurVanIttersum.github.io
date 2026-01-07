# Sobermod
This is a mod for the game potioncraft i made to help someone on reddit with alcoholism. The game has an oil map, which reminded the user of alcohol every time they see it in the game. They asked for a mod that replaces all references to the wine map with something else. That is what i made.

__Install a mod to learn how it works__

I started the project by installing a mod that the community has already made. This way I could reverse engineer how the mod works to figure out how I could make my own mod. I installed BepInEx, a modloader for unity games. I then installed the crucible modding framework. This is a mod someone made where other modders can provide their own assets and they will be loaded and handled by crucible. Crucible supports adding custom ingredients, custom potion bottles and custom merchants. I also installed the mod JK_newIngredients. This is a mod using crucible that adds custom ingredients to the game. I then started up the game to see it work.

Once I knew the mod worked I looked through the files of the mod to see what was included. I found a json file and a whole bunch of images. In the json file I found a whole bunch of data related to each ingredient added by the mod. I then read the documentation for crucible to understand what the json is doing.



__Decompiling .dll files__

Since the ingredients mod did not have any files containing code, the gameplay must be changed by crucible, so that is where I looked next. In the files for crucible I found some .dll files. These are files that contain compiled code. When you write code in a coding language your computer cannot execute that code directly. The code must first be compiled. This means that abstract concepts described by the code are translated(compiled) to concrete instructions that a computer can execute. .dll files contain compiled code. To read the code it must first be decompiled, otherwise you just get gibberish. To do this I used DotPeek by Jetbrians. Dotpeek is an IDE(Integrated Development Environment) that allows you to open .dll files and it will decompile the code so you can read it.

I used this to read the .dll files of crucible and found out that the mod has code to read json files, and create ingredients, potions and NPCs and add them to the game. This was all interesting, but crucible does not have enough features for me to make all the changes I want to make built in. Crucible does have some code for saving and loading stuff, that could be useful, so I was still considering integrating the project with crucible.

Contacting the PotionCraft modding community on discord
I got in contact with the discord community for PotionCraft. The potioncraft discord has a modding channel, where I asked for advice from people who have experience modding the game. The advice I got was to make my mod using BepInEx exclusively not using crucible. I also tried doing research on the legality of decompilation and asked the modding community about it. they said that decompling code is fine as long as you don’t share it. That is why I will not be showing any decompiled code in this devlog.



__Bepinex tutorial__

When I got the advice to use BepInEx I looked up a tutorial for how to setup a BepInEx project. I found this tutorial and I followed it. the tutorial has 4 stages. The first stage is installing software, such as .net SDK, Visual studio, BepInEx, and plugin template. The second stage is collecting data for setting up the project correctly, such as the unity version of the game and the dot net version. The third step is setting up the project, so creating the files that I will be working in and setting up basic configurations such as giving the project a name, a unique identifier and a version. The fourth step is setting up fundamental infrastructure, such as loading the PotionCraft.Core.dll file as a library and a logging system.



The code in my project is a class that inherits from UnityPlugin. BepInEx will recognize the Unity plugin and load it into the game when the game starts. The class contains an OnAwake function that gets executed when BepInEx loads the plugin. In this function I used the logging system to write “Hello world” when the plugin is loaded. This was a lengthy and complicated process, but once it was setup I tested it and it worked.



__Directories and libraries__

Once I the mod was working I tried changing the gameplay itself. To do this I need to add the games code to the plugins directory. In other words, the plugin needs to be aware of the games code. To do this I copied the PotionCraft.Scripts.dll file to a folder on my desktop and added a reference to the file location in the project file and included a using directive in the source file. I had already done this with the PotionCraft.Core.dll file. I then tried changing the difficulty settings in the game. I quickly ran into trouble, because the code responsible for loading and saving the difficulty settings data was in Potioncraft.Settings.dll. Once I realized the mistake I added PotionCraft.Settings.dll as a library.



It took a while for me to realize that this was the problem, because I had never encountered it before and in DotPeek there is no clear distinction between the source files. It took me trying to do the same thing in two different ways and recognizing the same error message to realize that the problem wasn’t with how I was doing it, but what I was doing.

__Harmonyx__

The problem of the difficulty setting not being present in the directory was solved, but I quickly ran into another problem. The difficulty settings are stored in special classes that inherit from ScriptableObject. This means that these classes exist as a file, not as a game object. This means that they are not stored in scenedata, but in a json file somewhere In the games data files. These json files are not loaded when the game starts, but when you start a new run. Changing these values when the plugin is loaded doesn’t work, because the settings have not yet been initialized in the first place. To solve this I had to figure out a way to execute code when the player starts a new game. I did this using Harmonyx.



Harmonyx is a library that allows you to hook to specific functions in a program and patch your own code during runtime. To use Harmonyx you need to provide classes that specify which off the games methods you want to change and where it can be found. Harmonyx then looks through all the code in the directory to find the methods you want to change. Harmonyx than creates a hook to that method, meaning that when the method is called, Harmonyx will make a copy of the method, making the changes you specify and then execute that code instead. 

At first I tried hooking to a function I made on my own project, just to test if it works, but I discovered that that test won’t work because Harmonyx will only create hooks to functions in the original code, not my plugin.

I looked through the games code and found the method I wanted to change. In PlayerSettings there is a method called SetInitGold. This method is called when the player starts a new run. This method is called early enough that the most important settings have not been used much and late enough that the settings have been initialized. I added a hook to this function and changed the value of _Gold in a postfix. This worked, but the change is only visible once the value of gold updates again, such as when the player trades with a customer or a merchant.



__Game difficulty settings__

So far I had only overwritten the value of gold after it is used. What I really wanted to do is change the value of the games difficulty settings. To do this I had to figure out the datastructure. I used the logging system to learn about the values of the settings and how they are stored. The settings are stored using a singleton pattern. GameDificultyStartingGold is a class that contains a static variable of it’s one and only instance. To access that instance you can call the class and then use the .Asset variable. The class inherits from multiple abstract classes, one of which inherits from ScriptableObject. The class contains a dictionary which is defined somewhere else. The dictionary only contains one key. The value of that key is a list of a KeyValuePair, which contains an enum and a float. 



To log the values I can take the first item in the dictionary, then get the value of it and convert it to an array, then loop through the array and log the KeyValuePairs. To change the value I can again get the first item in the dictionary, get the value of it and index using an enum and change the corresponding float.



I really don’t like this datastructure, but I’m sure the developers have a good reason to do it this way. Either way I figured out how to change the values. I changed the Harmonyx method to change the value of GameDificultyStartingGold in a prefix of the SetInitGold method. This way the value of the games settings is changed before it is used. This way, the changed gold value is visible at the start of the game instead of when updated later.

Once I had this working, I could also change other settings, such as the price of ingredients when buying them from merchants, the amount of ingredients you get when harvesting the garden and the amount of experience you get overall.

The code ended up looking like this:



__Harvesting__

Changing the difficulty settings was an important step, but it doesn’t allow me to make all of the changes I wanted to make. While looking through the code for the games difficulty settings I discovered that part of the reason the player gets too many ingredients is because the code that calculates the price of ingredients seems to be bugged. In the game you can use experience points to buy skills to upgrade among other things how many ingredients you get from harvesting herbs or mushrooms in the garden. The code keeps track of how many extra ingredients you get from the skills you have. The code that calculates the amount of ingredients uses a strange formula that includes this extra ingredients variable. Then before the value is returned the extra ingredients variable is added to the value again. It doesn’t make sense why this value would be added twice other than that it is an oversight by the developers. Since the formula is all on one line, and the formula is quite complicated, the line of code is quit long I have to horizontal bar in DotPeek to read the whole formula. I suspect that the formula was once spread out. The devs then combined it into one line and forgot to remove the extra addition. Later when they looked at the code again they didn’t see that it was already added in the first line, and therefore didn’t remove it. this explains why the balance is so off. The garden is not just overpowered, it’s bugged!

__Reflection__

Even if I’m wrong and it isn’t bugged it still makes the formula worse, because changing the difficulty settings for the ingredientscount doesn’t give me much freedom when working with multiples of 2. I decided to overwrite the whole method using Harmonyx. This way I could implement my own formula. One problem with this is that it’s a private method that gathers information from private variables and functions and use data structures that are also private. I was able to avoid the problem when setting the gold, because there was one private field and a public variable, so I was able to change the one that was public. For this situation I had to approach the problem head-on using reflection.

Reflection is a functionality in C# that allows you to inspect and manipulate classes methods and fields at runtime. Reflections doesn’t care if something is private or public. This means that instead of simply calling a method, I can use reflection to find the method in the assembly at runtime, and use reflection to execute it. This solution causes a bit more lag than calling the method directly, and the code becomes very wordy, but this does get around the problem of private variables and methods.

I used Harmonyx to replace the ingredient calculation method with my own method, and used reflection to get data from private fields and methods. I ended up mostly copying the original formula, but I took out the supposed bug.
The code ended up looking like this: 



__Ingredient Prices__

I had already figured out how to use game difficulty settings to change the price of ingredients, but I wanted more control over the prices. I wanted to change not just the prices overall, but also the difference in prices between ingredients. I wanted cheap ingredients to become a little bit more expensive and expensive ingredients to become a bit cheaper. This way players are incentivized to use expensive, long distance ingredients instead of spamming cheap, short distance ingredients. I used the logging system to get a list of all the prices and put them in an excel sheet to come up with a formula that changes the prices to the values I want. The formula I came up with is raising the price to the power of 0.75, then multiplying by 2.5.



This makes the cheapest ingredients a bit more expensive, while keeping medium priced ingredients about the same and making expensive ingredients cheaper. Once I had the formula I used Harmonyx to hook to the GetPrice method which is the method that calculates the price. I replace the method with my prefix which uses my formula before returning the resulting value.

The code ended up looking like this:









