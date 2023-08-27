+++
title = "MonoGame – Tutorial 2 – Spritesheets"
date = "2023-08-27T00:00:00+00:00"
author = "mort8088"
authorTwitter = "mort8088" #do not include @
cover = "images/SpriteSheets.webp"
tags = ["sprites", "spriteSheet", "sprpack", "gameDevelopment", "mgcb_Snippets", "monoGame", "tutorial"]
description = "In the world of game development, creating visually appealing and smooth-running games is a top priority. One powerful technique that can significantly contribute to achieving both of these goals is the use of spritesheets. Working with the MonoGame framework, integrating spritesheets into your project can bring a number of benefits that can improove performance and aesthetics."
showFullContent = false
readingTime = true
hideComments = false
toc = true

+++

Spritesheets are essentially large images that contain many smaller images, often representing individual frames of animation or different game elements. By storing several images in a single file, you reduce the number of individual texture files that need to be loaded into memory. This efficiency can lead to better performance and faster load times, especially in games with a lot of objects or animations.

## Benefits of Using Spritesheets in MonoGame

1. **Improved Performance:** In games built using MonoGame, every texture loaded into memory has a performance cost. By using a spritesheet, you can minimize this cost by loading a single texture into memory rather than a lot smaller ones. This results in reduced memory usage and faster rendering times, leading to a smoother gaming experience.

1. **Reduced Texture Switching:** When elements of a game are spread across multiple textures, switching between these textures during rendering can be resource-intensive. Spritesheets eliminate the need for frequent texture switches since all the necessary images are packed into a single texture. This contributes to a more seamless gameplay experience, as there are fewer interruptions in rendering.

1. **Efficient Memory Usage:** Games often feature recurring objects or characters that require multiple frames of animation. By arranging these frames in a spritesheet, you eliminate redundant memory usage caused by duplicated images. This optimization leaves more room for other game assets and reduces the overall memory footprint of your game.

1. **Simplified Asset Management:** Managing individual texture files can be cumbersome, especially in larger projects. Spritesheets consolidate your graphical assets into fewer files, making it easier to organize, locate, and update game elements. This streamlined asset management process enhances your workflow and saves time during development.

1. **Smooth Animations:** Spritesheets are particularly advantageous for creating fluid animations. Since all frames are contained within a single image, transitioning between frames is faster and more consistent. This results in smoother animations without the potential for hiccups that can occur when loading individual textures on the fly.

Incorporating spritesheets into your MonoGame project is a strategy that can yield impressive dividends in terms of performance optimization and visual finesse. By reducing texture-related overhead and simplifying asset management, you're able to create games that run more smoothly, load faster, and look better. Whether you're a solo indie developer or part of a larger team, leveraging the power of spritesheets can be a game-changer in your game development journey.

And with that I give you:-

## sprpack

[***sprpack** on github*](https://github.com/mort8088/sprpack) is my command line sprite sheet packer, a fully featured tool for combining multiple images into a single, efficiently laid out spriteSheet.

It supports reading PNG, JPG, BMP, and GIF images and produces a single PNG image with all the images embedded inside. The resulting image is transparent anywhere an image is not drawn. The tool also produces an accompanying file that maps the image file names with their rectangles, for use in your game to find the regions of the image you are interested in. The tool supports options for setting the maximum resulting image size, padding (added to the size of the individual images), as well as options for requiring a power-of-two sized output and a square output.

Here is a sample output created from 720 individual images found at [blogoscoped.com](http://blogoscoped.com/archive/2006-08-08-n51.html):

![Example SpriteSheet](https://github.com/mort8088/sprpack/raw/master/images/Sheet1.png)

> sprpack was originaly written by [Kelly Gravelyn](https://ohthesetrees.com/) except the code for computing the efficient placement of the rectangles which was taken from the [Nuclex Framework](http://blog.nuclex-games.com/tags/nuclex-framework/). I removed the windows forms GUI, replaced the Command line argument paser and the use of System.Draw has been replaced with SixLabors.ImageSharp in an effort to make it work in Linux.

## Using the sprite Sheet

Using the sprite sheet is simple enough adding the files to the content pipeline is as simple as adding the following block to your .mgcb file, just update where the files are and what they are called:-

``` txt
#begin Textures/Atlas_map.xml
/importer:XmlImporter
/processor:PassThroughProcessor
/build:Textures/Atlas_map.xml

#begin Textures/Atlas.png
/importer:TextureImporter
/processor:TextureProcessor
/processorParam:ColorKeyColor=255,0,255,255
/processorParam:ColorKeyEnabled=True
/processorParam:GenerateMipmaps=False
/processorParam:PremultiplyAlpha=True
/processorParam:ResizeToPowerOfTwo=False
/processorParam:MakeSquare=False
/processorParam:TextureFormat=Color
/build:Textures/Atlas.png
```

Then you can load the image into a Texture2D and the map into a Dictionary<string, Rectangle> then make a normal(ish) call to Spritebatch.Draw :-

``` C#
protected override void LoadContent()
{
    // Create a new SpriteBatch, which can be used to draw textures.
    _spriteBatch = new SpriteBatch(GraphicsDevice);

    // TODO: use this.Content to load your game content here
    Texture2D texture = Content.Load<Texture2D>("Textures/Atlas");
    Dictionary<string, Rectangle> sprPos = Content.Load<Dictionary<string, Rectangle>>("Textures/Atlas_map");
}

protected override void Draw(GameTime gameTime)
{
    Rectangle destinationRectangle = new Rectangle(xPos, yPos, sprPos["NameOfSprite"].Width, sprPos["NameOfSprite"].Height)
    Rectangle? sourceRectangle = sprPos["NameOfSprite"];

    graphics.GraphicsDevice.Clear(Color.CornflowerBlue);

    // TODO: Add your drawing code here
    _spriteBatch.Begin();
    _spriteBatch.Draw(texture, destinationRectangle, sourceRectangle, Color.White);
    _spriteBatch.End();

    base.Draw(gameTime);
}
```

## You *still* don't need the MGCB Editor

If you missed my post on Fonts and adding content to your games in Linux without needing to use the MGCB Editor.

- select **Configure User Snippets** under **File** > **Preferences** (**Code** > **Preferences** on macOS), and then type mgcb and select mgcb (MonoGame Content Builder)
- Add the following code segment to the mgcb.json file:-

```JavaScript
  "SpriteSheet Files": {
    "prefix": "sprites",
    "body": [
      "#begin ${1:fileName}_map.xml",
      "/importer:XmlImporter",
      "/processor:PassThroughProcessor",
      "/build:${1:fileName}_map.xml",
      "",
      "#begin ${1:fileName}.png",
      "/importer:TextureImporter",
      "/processor:TextureProcessor",
      "/processorParam:ColorKeyColor=255,0,255,255",
      "/processorParam:ColorKeyEnabled=True",
      "/processorParam:GenerateMipmaps=False",
      "/processorParam:PremultiplyAlpha=True",
      "/processorParam:ResizeToPowerOfTwo=False",
      "/processorParam:MakeSquare=False",
      "/processorParam:TextureFormat=Color",
      "/build:${1:fileName}.png",
      "$0"
    ],
    "description": "A sprite sheet block."
  }
```

- Close any open files.

Now when you want to add a new spriteSheet to the project.

- Open the **Content.mgcb** file for editing.
- move to the end of the file and type _sprites_ when IntelliSense offers you the **SpriteSheet Files** {{< image src="images/IntelliSense-sprite.webp" alt="IntelliSense SpriteSheet Files" style="margin: auto;" >}}
- hit tab and type the name of the file you created, the default from sprpack is Atlas, followed by tab.

That's it for today's installment. Next I'll make a nuget package that contains my full spritesheet support class'. If you have any questions or feedback, feel free to leave a comment below. And don't forget to subscribe to the [RSS](/index.xml) so you'll know when I post the next engrossing installment.
