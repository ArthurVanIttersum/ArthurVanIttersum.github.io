# Environment Art project
This is a Schoolproject where i made a 3D model of a gym. The objective is to use tilable textures and texture atlases to make kit to make the building. We also got a client brief and a lore guide. I started by searching for reference images and grayboxing.

In class i made a collage of references using PureRef.

![Screenshot of References](../Assets/EnvironmentArt/References.png)

Here is the grayboxing i did for this project. I decided to do it in Blender instead of Maya because i had a problem with my Maya license. I started with simple shapes and added more shapes to it. i kept copying the work for every step instead of iterating in the same place, so i could keep track of my older versions. I started in the top left and i copied to the right. If i wanted to try something else i went back to an older version and copied down. This creates a nice branching version history. On the right you can see blocks that represent the dimensions of the building that i could work with.

![Screenshot of Grayboxing](../Assets/EnvironmentArt/Grayboxing.png)

Once i had access to Maya i used maya's grid system to experiment with exact dimensions. this also lead to a slight change in shape compared to the last Blender version.
![Screenshot of References](../Assets/EnvironmentArt/MayaBlockout.png)

I also made a textureplan and a kit layout plan. The textureplan is a high-level plan of the texture space i had available and how i wanted to use that space. In Krita i made a new image of size 2048X2048. since i had 16 of these images worth of texture space i divided the image into 16 areas  of size 512X512. Each of those squares represents an area of 2048X2048. I cut each square in four again, to create a 64 by 64 grid of squares that each represent a 256X256 area of texture space. I used different layers, so i could turn the dividing lines on or off. I also added a text layer that i used to write down what each cell would be used for. blue text is used for 256X256 areas and red text is for 512X512 areas. the red text serves as a summary of what the texture atlas will be used for. The top half is reserved for the outside and the botom half is reserved for the inside. i also used green text in places to distinguish repeating textures from non-repeating textures. 

![Screenshot of References](../Assets/EnvironmentArt/TexturePlan.png)

While i was making this texture plan i wasn't sure whether the normal map and other types of maps would count as taking up texture space. If it does count i might run out of space. if it doesn't count i would have a lot of space left over. My teachers told me it would be explained later, and i should work on the actual model instead of more texture planning, so i did.

I made a Kit pieces plan. This was not part of the exercise, i came up with it myself. The point of the Kit pieces plan is to figure out what kit pieces i needed. I started with the shape of the building, and cut out parts of it that are used repeatedly. I used different colors to differentiate different parts of the building, which wouldn't otherwise be visible in 2D. For example, the roof has a lower part, a higher part and a circular part. Each of those are different. The big roof is long and straight, but the start and end are different from the main roof. This means that they need to be seperate kit pieces. This is how i made kit for the whole building.

![Screenshot of References](../Assets/EnvironmentArt/KitLayoutPlan.png)

I made kit for the main building first. I used bricks for the walls, with vertical wooden beams and windows to break up the texture. I then made the tower, using the wood both horizontal and vertical. I really liked how that looked, so i went back to the main building and added horizontal beams there as well. Instead of changing the kit i added the horizontal beam as a new element in the kit, to place it all around the building.

Here is A showcase of the kit i made for the outside.

![Screenshot of References](../Assets/EnvironmentArt/KitOutside.png)

Since the project uses HDRP all the special textures needed to be turned into a mask map, which the materials could then use. This meant that the texturespace i had left over in my textureplan would be used by those mask maps, but they wouldn't take up nearly as much space as i was worried about. after testing the model in Unity i added LOD-ing to the shutter next to the windows.

I then ran out of time, and had to work on the group project. After the group project i had one week to finish the interior and more. I decided to reuse the outside materials for the inside, to save a bit of time. I even re-used some of the kit, though i had to change things a bit. I then speedran making furniture and other things. I added one leather texture, as i had nothing soft in my texture pallet yet. I made the inside environment, added furniture, exported the project to unity, baked the lighting, added vines as a decal and handed in the project. Here is the final result.

![Screenshot of References](../Assets/EnvironmentArt/FrontView.png)

![Screenshot of References](../Assets/EnvironmentArt/Back.png)
