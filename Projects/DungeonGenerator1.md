# Dungeon Generator using room subdivision
This is a project i did for my algorithms course. The goal is to make a dungeon generator using the binary partition algorithm.

The idea of this algorithm is to diffine a rectangular area and repeatedly divide it until the rooms are small enough. This creates a space that looks a bit like a mondrian painting. This algorithm can be used to create a dungeon by adding walls where the lines are.

I started by creating a list of type RectInt. This is a datastructure that was provided to us. you can represent an axis aligned room using two vectors. One for the top left corner and one for the bottom right corner. The RectInt datastructure is really just a class that contains two integer vectors and a few extra methods. This way each RectInt in my list represents a room. I decided to use a list so i could add and remove rooms efficiently.

To display these rooms we got a special script that uses Unity's OnDrawGizmo system. This script has a method that can draw the RectInt as a gizmo for the next frame. By calling this method in Update for all rooms in the list it is easy to draw all rooms in the list. This makes it easy to see what's happening. To separate the rooms more clearly i gave each room a different color. I didn't want the color to be randomly generated every frame, because then it looks like a crazy disco show. My solution was to keep a list of colors in the class. Every time a room is added to the room list a random color is also added to the color list. In the update method where i loop through the rooms using a for loop to acces the indexes i also use that index to acces the color from the color list. this gives every room a different color, without the colors changing every frame.

I then made a method called SplitRoomsHorizontal, which takes a given room in the list and splits it into two rooms, removing the origional room from the list and adding the two new rooms to the list. First i made it split vertically down the middle. The math calculating the rectangles got a bit messy, so i added a method that creates a room from two Vector2Ints. This meant that i only needed to call this method for both new rooms using four corner points in total. Two of those corner points were freebies, because they are already in the origional rectangle. The other two points could be calculated using the split value for X and the top and bottom corner points for Y. This splits the room vertically.

I then copied this method and turned it into a vertical split version. I made the split value random using unity's built in randomization system instead of using half the width or height. Once i had this working i made a method called SplitRoom that calls either method randomly. Starting the list with one room and repeatedly calling Split room generates a bunch of rooms.

Next i added some safety mechanisms, to prevent rooms from getting too small. This was easy to check with just an if statement. If the room is too small the method returns early returning false, whether if it's not too small the normal logic is followed and the method returns false. In the method that randomizes whether to split horizontally or vertically it get this boolean returned, which is used to activate the other method. This means that of a room is too small to be split horizontally, but can be split vertically the system will first randomize which way to split and if it tries to split horizontally it will try to split vertically instead. If this also fails it means that the room is simply too small to be split at all. In this case the split rooms method returns false. This is usefull later for figuring out when to stop trying to split rooms.

I then added a for loop that repeatedly calls the split rooms method. If this method returns false i add one to the number of completed rooms if the number of completed rooms is larger or equal to the number of rooms in the list the loop breaks. This is how i generate the room data. 

![Screenshot of GeneratedDungeon](../Assets/DungeonGenerator1Layout.png)

![Screenshot of GeneratedDungeon](../Assets/DungeonGenerator1Doors.png)

![Screenshot of GeneratedDungeon](../Assets/DungeonGenerator1Graph.png)

![Screenshot of GeneratedDungeon](../Assets/DungeonGenerator1Result.png)
