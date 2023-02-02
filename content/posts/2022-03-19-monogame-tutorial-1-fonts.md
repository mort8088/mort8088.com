---
title: MonoGame – Tutorial 1 – Fonts
author: mort8088
type: post
date: 2022-03-19T00:00:24+00:00
url: /2022/03/19/monogame-tutorial-1-fonts/
featured_image: /wp-content/uploads/2022/02/MonoGame-SquareLogo_256px.png
tags:
  - fonts
  - Game Development
  - mgcb snippets
  - MonoGame
  - tutorial

---
To get started with making games it&#8217;s often a good idea to be able to write out debug messages while you&#8217;re working on your game even if that&#8217;s just the current FPS. MonoGame doesn&#8217;t allow you to use the system fonts on the computer the game is running on you need to provide your own. To achieve this you need to add a font to your game to be used for writing strings to the screen.

## Add fonts to your game.

To be able to use a TrueType font, MonoGame requires the TrueType font file and a .spritefont file.

  1. Start the **MGCB Editor** and open the **Content.mgcb** file from your project Content folder.
  2. Create the .spritefont file by right-clicking the Content node in the Project panel then &#8220;Add -> New Item&#8230;&#8221;, then select **SpriteFont Description** from the list fill in the **Name** and click **Create**.  
<img decoding="async" loading="lazy" src="https://mort8088.com/wp-content/uploads/2022/03/New-Font-File-SystemFont.png" alt="New Font File" class="size-full wp-image-303 aligncenter" width="370" height="332" srcset="https://mort8088.com/wp-content/uploads/2022/03/New-Font-File-SystemFont.png 370w, https://mort8088.com/wp-content/uploads/2022/03/New-Font-File-SystemFont-300x269.png 300w, https://mort8088.com/wp-content/uploads/2022/03/New-Font-File-SystemFont-150x135.png 150w" sizes="(max-width: 370px) 100vw, 370px" /> 
  3. Open the newly created .spritefont file in VS code, find this line and change it to your selected .ttf font. If the font is installed on the system, just type the name of the font. <div class="hcb_wrap">
      <pre class="prism undefined-numbers lang-html" data-lang="HTML"><code>&lt;FontName&gt;Arial&lt;/FontName&gt;</code></pre>
    </div>

## Code Example {#usage-example}

  1. Open the **game1.cs** file
  2. Add a new global variable to hold the font <div class="hcb_wrap">
      <pre class="prism undefined-numbers lang-csharp" data-lang="C#"><code>private SpriteFont systemFont;</code></pre>
    </div>

  3. Next, add the following code to the **LoadContent()** method. <div class="hcb_wrap">
      <pre class="prism undefined-numbers lang-csharp" data-lang="C#"><code>// TODO: use this.Content to load your game content here
systemFont = Content.Load&lt;spritefont>("SystemFont");&lt;/spritefont></code></pre>
    </div>

  4. and finally, add your text output code to the **Draw(GameTime gameTime)** method <div class="hcb_wrap">
      <pre class="prism undefined-numbers lang-csharp" data-lang="C#"><code>// TODO: Add your drawing code here
_spriteBatch.Begin();
_spriteBatch.DrawString(systemFont, "Hello World.", new Vector2(100.0f, 100.0f), Color.Green);
_spriteBatch.End();
base.Draw(gameTime);</code></pre>
    </div>

  5. press F5 to compile and run, job done.

## Getting new fonts

If like me you&#8217;re using Linux Mint(any Debian based Distro) you can install the Microsoft TTF fonts by running the following from the terminal.

<div class="hcb_wrap">
  <pre class="prism undefined-numbers lang-bash" data-lang="Bash"><code>sudo apt-get install ttf-mscorefonts-installer</code></pre>
</div>

This will give you access to fonts like:-

  * Arial
  * Times New Roman
  * Courier New
  * Trebuchet

Another option is to download the <a href="https://github.com/SimonDarksideJ/XNAGameStudio/wiki/Redistributable-Font-Pack" target="_blank" rel="noopener">Redistributable Font Pack from XNAGameStudio</a> by downloading the zip file from the Archived GitHub site.

<img decoding="async" loading="lazy" src="https://mort8088.com/wp-content/uploads/2022/03/redistfonts.png" alt="" class="aligncenter wp-image-313 size-full" width="801" height="366" srcset="https://mort8088.com/wp-content/uploads/2022/03/redistfonts.png 801w, https://mort8088.com/wp-content/uploads/2022/03/redistfonts-300x137.png 300w, https://mort8088.com/wp-content/uploads/2022/03/redistfonts-150x69.png 150w, https://mort8088.com/wp-content/uploads/2022/03/redistfonts-768x351.png 768w" sizes="(max-width: 801px) 100vw, 801px" /> 

Once you download the zip file unarchive the contents of the zip file to your user font folder (~/.local/share/fonts).

when you add new fonts to this folder then it&#8217;s a good idea to log out and back in again or refresh your font cache with the command:-

<div class="hcb_wrap">
  <pre class="prism undefined-numbers lang-bash" data-lang="Bash"><code>sudo fc-cache -fv</code></pre>
</div>

But remember you need to be sure that you are allowed to use a given font in your game, it&#8217;s always a good idea to only use fonts that you know have a Creative Commons license that allows commercial use.

<p dir="auto">
  If you want to write something on the screen without getting sued? These sites contain, besides other stuff, many free-for-commercial-use fonts (you&#8217;ll have to read the licenses though).
</p>

<ul dir="auto">
  <li>
    <a href="http://www.fontsquirrel.com/" rel="nofollow noopener" target="_blank">http://www.fontsquirrel.com</a>
  </li>
  <li>
    <a href="http://www.1001fonts.com/" rel="nofollow noopener" target="_blank">http://www.1001fonts.com</a>
  </li>
</ul>

## You don&#8217;t need MGCB Editor

If you don&#8217;t like the fact you currently need a windows application to add content to your games in Linux here is an extremely quick overview of how I add spriteFontDescription files to my project without needing to use the MGCB Editor.

  1. Install the following plugin to VS-Code &#8220;**File Templates for VSCode&#8221;** with the command palette (Ctrl+P), <span>paste the following command, and press enter.</span> <div class="hcb_wrap">
      <pre class="prism undefined-numbers lang-plain" data-lang="Plain Text"><code>ext install bam.vscode-file-templates</code></pre>
    </div>

  2. Download [SpriteFont.spritefont Template][1] and save it in _~/.vscode/extensions/bam.vscode-file-templates-{VERSION}/templates_
  3. select **User Snippets** under **File** > **Preferences** (**Code** > **Preferences** on macOS), and then type mgcb and select mgcb (MonoGame Content Builder)
  4. Add the following code segment to the mgcb.json file:- <div class="hcb_wrap">
      <pre class="prism undefined-numbers lang-js" data-lang="JavaScript"><code>"Sprite Font Description": {
	"prefix": "font",
	"body": [
		"#begin ${1:fileName}.spritefont",
	"/importer:FontDescriptionImporter",
	"/processor:FontDescriptionProcessor",
	"/processorParam:PremultiplyAlpha=True",
	"/processorParam:TextureFormat=Compressed",
	"/build:${1:fileName}.spritefont",
	"$0"
	],
	"description": "A sprite font description block."
}</code></pre>
    </div>

  5. Close any open files.

Now when you want to add a new font to the project.

  1. Right-click the Content folder and select &#8220;**New File from Template**&#8220;.
  2. Start typing _SpriteFont_ and hit enter to select and type the name of the file.
  3. Next, open the **Content.mgcb** file for editing.
  4. move to the end of the file and type _font_ when IntelliSense offers you the **sprite font description**<img decoding="async" loading="lazy" src="https://mort8088.com/wp-content/uploads/2022/03/IntelliSense-font.png" alt="IntelliSense font" class="size-full wp-image-309 aligncenter" width="525" height="110" srcset="https://mort8088.com/wp-content/uploads/2022/03/IntelliSense-font.png 525w, https://mort8088.com/wp-content/uploads/2022/03/IntelliSense-font-300x63.png 300w, https://mort8088.com/wp-content/uploads/2022/03/IntelliSense-font-150x31.png 150w" sizes="(max-width: 525px) 100vw, 525px" />
  5. <span>hit tab and type the name of the file you just created followed by tab</span>
  6. you can edit the new font description file as normal.

 [1]: https://github.com/MonoGame/MonoGame/blob/f79c28805103a761e36ca97e16b4cf259010f778/Tools/MonoGame.Content.Builder.Editor/Templates/SpriteFont.spritefont