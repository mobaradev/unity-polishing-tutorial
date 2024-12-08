
# Unity Polishing Tutorial

This tutorial shows how quickly and easily you can polish your game. It takes less than 40 minutes to complete the steps which have an incredibly positive impact on the look of the game.

As an example project, I will use my redesigned implementation of the classic Pong game. We will not change the rules and mechanics of the projects - the gameplay will stay exactly the same but we will add effects, animations and esthetic elements that change the visual aspects of the game.

Before:
![](https://i.imgur.com/sTtnn8T.png)
After:
![ ](https://i.imgur.com/aVohPaI.png)

Clone the following repo:
https://github.com/mobaradev/PongGame

and open it with Unity 2022.3.X (the exact version is 2022.3.48f but it will work for any 2022.3).

Familiarize yourself with the gameplay and begin the tutorial.

# 1. Post-processing effects (5 minutes)

Open the "GameScene". Select "Global Volume" gameobject and click "Add Override" in the "Volume" component.

![ ](https://i.imgur.com/EABKefO.png)

### Bloom effect
Add Post-Processing->Bloom effect.
Enable "Intensity" and set it to 0.1.
![ ](https://i.imgur.com/IlBO0pV.png)

Create a custom material and name it "Player". 
![ ](https://i.imgur.com/2xQRdVd.png)

Apply the material to both PlayerRacket-Top and PlayerRacket-Bottom gameobjects:
![ ](https://i.imgur.com/ep6zKtx.png)

Click on the "Player" material object and enable "Emission" property:
![ ](https://i.imgur.com/cJ3QuR1.png)

Edit the "Emission Map" to your favorite color and increase the color "Intensity" property to a higher value (for example 4 or 5).

You can the scene the player rackets are glowing now:
![ ](https://i.imgur.com/XtgIXP7.png)

Add more materials for different gameobjects:
![ ](https://i.imgur.com/tETXpvY.png)
![ ](https://i.imgur.com/YHviLoQ.png)
### Vignette effect
Select "Global Volume" gameobject and add another effect by clicking "Add Override" -> Post-processing -> Vignette. Use the following values, or change them to your own preference.
![ ](https://i.imgur.com/eD7T1VJ.png)

With just those two effects the game already looks way cooler!
![ ](https://i.imgur.com/jpAMEiy.png)

# 2. Animations (12 minutes)

Create a directory "Animations". Then, go to Prefabs->PlayerRacket and open the prefab.
Open the Animation panel (Window -> Animation):
![ ](https://i.imgur.com/jCXxxxz.png)

Create a new animation clip and save it as "PlayerRacketHit" in the "Animations" directory.
Add property -> Transform -> Scale:
![ ](https://i.imgur.com/9ted1nn.png)

Right click in the middle of the scale and select "Add Key".
![ ](https://i.imgur.com/KfogRei.png)

Slightly increase the Scale.x and Scale.y values:

 - Scale.x: 0.424
 - Scale.y: 2.2366

![ ](https://i.imgur.com/LpCC7pJ.png)

If you click the "Play" button, you can see the animation preview in the "Scene" window.

Add another property - Sprite Renderer -> Sprite Renderer.Material._Emission Color:
![ ](https://i.imgur.com/jqbChK7.png)

Again, select the center of the scale and edit the emission color to, for example Color.x = 50:
![ ](https://i.imgur.com/xHLyzHa.png)

If you play the scene now, you should see the rackets changing size and glow color all the time. However, the plan is to play the animation only when the racket touches a ball.

Open again the PlayerRacket prefab (make sure you edit the actual prefab, not the scene gameobject instance):
![ ](https://i.imgur.com/sjsiswL.png)


Double click the "PlayerRacket" controller in the Animator component:
![ ](https://i.imgur.com/A9NvVdx.png)

It should open the Animator window (alternatively you can open this window by Window -> Animation -> Animator).

Go to "Parameters" tab and create a new Trigger parameter and call it "RacketBallHit".

![ ](https://i.imgur.com/Djsepuk.png)

Right click on the empty space and Create State -> Empty.
![ ](https://i.imgur.com/pCRqP1a.png)

Rename it to "PlayerRacketDefault" and set it as a layer default state.
![ ](https://i.imgur.com/KwIeCYa.png)

Right click "Any State" -> Make transition to the "PlayerRacketHit".
![ ](https://i.imgur.com/VoAiT5j.png)

Select the transition arrow and in the Inspector window, add the condition on the RacketBallHit trigger:
![ ](https://i.imgur.com/F42kpCL.png)

And finally, create one last transition from PlayerRacketHit to PlayerRacketDefault with no conditions:
![ ](https://i.imgur.com/zhw5kel.png)

Click the "PlayerRacketHit" block and in Inspector, increase the speed to 3.
![ ](https://i.imgur.com/aqr0ryN.png)

Tap on the Motion "PlayerRacketHit":
![ ](https://i.imgur.com/QkdB8UL.png)

This should show you the location of this file in your Project window:
![ ](https://i.imgur.com/AlNFzGA.png)

Click on it and in Inspector, disable the "Loop time":
![ ](https://i.imgur.com/qPCLRes.png)

This configuration plays the hit animation on the "RacketBallHit" trigger call and then automatically goes back to the default state. The very last thing to do is to actually call this trigger.
Open the  "PlayerRacket.cs" script and add the following line if the player collides with a "Ball" object:

    this.GetComponent<Animator>().SetTrigger("RacketBallHit");

![ ](https://i.imgur.com/WBNFznQ.png)

Test the game - it should animate the rackets every time they hit a ball!

You can repeat those steps for the Obstacle prefab:
In this case, use higher scale values, default size is 0.7, so for the center use for example 1.1:
![ ](https://i.imgur.com/4gGXjlt.png)

![ ](https://i.imgur.com/CRDWqGk.png)

and use this code in Obstacle.cs:

    public void OnCollisionEnter2D(Collision2D collision)
    {
        if (collision.gameObject.CompareTag("Ball"))
        {
            this.GetComponent<Animator>().SetTrigger("ObstacleBallHit");
        }
    }

You can also repeat those steps for the Ball prefab but for this objects, do not change its size, animate only the color. I used animation speed = 4.
![ ](https://i.imgur.com/Cwisjma.png)

# 3. Fonts (4 minutes)

It's amazing how much difference the right font makes.
Go to https://fonts.google.com/ and find a cool font for your game theme. For this game, I will use a futuristic font called "Audiowide" (https://fonts.google.com/specimen/Audiowide).

Download the font and in Unity project create a folder called "Fonts". Extract the font there.
![ ](https://i.imgur.com/9fDvDDT.png)
It is extracted as a ".ttf" file, which is not supported for the modern TextMeshPro objects. There is a built-in tool that converts .ttf fonts into a new. Click Window -> TextMeshPro -> Font Asset Creator.

![ ](https://i.imgur.com/juTlOnt.png)

Use the downloaded .ttf font as a source, generate font atlas and save it in our "Fonts" directory.

![ ](https://i.imgur.com/yutHGmc.png)

![ ](https://i.imgur.com/CTlNxql.png)

In the Hierarchy window, select Canvas -> "Lives text" and then Canvas -> "Score text".
![ ](https://i.imgur.com/O7K4j99.png)

Drag & drop the font asset!

![ ](https://i.imgur.com/iQFZF2C.png)

# 4. Particles (4 minutes)
Create a new gameobject Particle System (Effects -> Particle System).

![ ](https://i.imgur.com/caRrjOJ.png)

Create a prefab out of it (drag & drop from Hierarchy window to the Project files window).
Delete it from the Hierarchy window and open the prefab to modify the parameters.
![ ](https://i.imgur.com/10ET2h6.png)

![ ](https://i.imgur.com/FUYu2E7.png)

By enabling the "Gravity Modifier", the particles will be affected by the gravity (which in this game is pulling objects to the right). You can play around and set your own parameters, use the built-in particles simulator in the scene window (visible only when you're editing a particle system):
![ ](https://i.imgur.com/vhO2TBE.png)

Let's edit the Ball.cs script.
Define Particles as public GameObject at the beginning of the script:
![ ](https://i.imgur.com/KX8ZMXD.png)

And for every collision, spawn the particle system prefab:

    Instantiate(this.Particles, this.transform.position, Quaternion.identity);

![ ](https://i.imgur.com/3CxbUHE.png)

Open the Ball prefab and drag&drop the Particle System prefab (make sure you use prefabs, not the scene objects):
![ ](https://i.imgur.com/DddcfmZ.png)

Play the game and observe the particles showing up every time any ball hit something!


# 5. External explosion assets (7 minutes)
Go to the Unity AssetStore and find any cool explosion assets! I will be using "Cartoon FX Remaster Free" by Jean Moreno (https://assetstore.unity.com/packages/vfx/particles/cartoon-fx-remaster-free-109565).

Add to My Assets -> Open in Unity -> Download -> Import.
![ ](https://i.imgur.com/bPvQOGM.png)

After all files are imported, explore the prefabs (double click to see the preview of it, or drag&drop to the scene):
![ ](https://i.imgur.com/rdSdGrg.png)

Let's apply the effects to our ball prefab.
Edit the Ball.cs script by adding the following public variables at the beginning (right after the Particles variable we defined in the previous step):

    public List<GameObject> Ball2BallEffects;
    public List<GameObject> Ball2PlayerRacketEffects;
    public List<GameObject> Ball2ObstacleEffects;
    public List<GameObject> Ball2BorderRightEffects;
    public List<GameObject> Ball2BorderLeftEffects;

And then add this code to the OnCollisionEnter2D function:

        if (collision.gameObject.CompareTag("Ball"))
        {
            Instantiate(this.Ball2BallEffects[Random.Range(0, this.Ball2BallEffects.Count)], this.transform.position, Quaternion.identity);
        }
Repeat this step for all the cases:
![ ](https://i.imgur.com/9VtCWdc.png)

Open the Ball prefab and drag&drop your favorite effects to the cases we just defined. Since we used List<GameObject> and Random.Range, you can attach multiple effects to each case and the random one will be chosen every time!

This is my configuration:

![ ](https://i.imgur.com/FOvrKpn.png)



# 6. Camera Shake (2 minutes)

The previous effect package already included camera shake for some effects (you can disable it in the prefab CFXR_Effect component):
![ ](https://i.imgur.com/CpUYD6W.png)

However, if you used other package, or simply just want to add more shakes in our own conditions it's good to implement our own mechanism for it.
Create Scripts->CameraShaker.cs and paste the following code:

    public class CameraShaker : MonoBehaviour
    {
	    private float _timer;
	    private bool _isShaking;
	    private float _shakeAmount;
	    private Vector3 _originalPosition;
	    
	    void Start()
	    {
	        this._isShaking = false;
	    }
	    
	    void Update()
	    {
	        if (this._isShaking)
	        {
	            this._timer -= Time.deltaTime;
	            transform.localPosition = this._originalPosition + Random.insideUnitSphere * this._shakeAmount;

	            if (this._timer <= 0f)
	            {
	                this._isShaking = false;
	                this._timer = 0;
	                this.transform.localPosition = _originalPosition;
	            }
	        }
	    }

	    public void Shake(float amount, float duration)
	    {
	        if (!this._isShaking) this._originalPosition = this.transform.localPosition;
	        this._isShaking = true;
	        this._shakeAmount = amount;
	        this._timer = duration;
	    }
    }

Attach the script to the "Main Camera" gameobject.

Now you can call it from any other script in your game by simply using this line:

    Camera.main.GetComponent<CameraShaker>().Shake(amount, duration);
I attached it to the Ball.cs script for the collision with the PlayerRacket, since the effects I chose for it in the previous step do not create any shakes.

![ ](https://i.imgur.com/LP5QfYs.png)


# 7. Conditional Post-Processing (5 minutes)

Create Scripts -> ConditionalPostProcessing.cs and attach it to the Global Volume object.
Add this public variable:

    public VolumeProfile GameVolume;
and in the Inspector, select "GameSceneProfile" for it.

## Random Split Toning
"Add Override" to the Volume to add another effect - "Split Toning". Set roughly the similar color and parameters as below and notice the scene colors change.
![ ](https://i.imgur.com/RAt7I0n.png)

Add another public variable to the ConditionalPostProcessing.cs:

    public List<Color> SplitToningColors;

and these lines to the Start function:

    if (this.GameVolume.TryGet<SplitToning>(out SplitToning splitToning))
    {
        splitToning.shadows.value = this.SplitToningColors[Random.Range(0, this.SplitToningColors.Count)];
    }

![ ](https://i.imgur.com/aQ5QQcY.png)

Set your favorite colors (test them first in the SplitToning -> Shadows):
I used the following 4 colors:
![ ](https://i.imgur.com/5xth4Mn.png)

 1. #7100FF
 2. #FF001B
 3. #0964FF
 4. #D7698B

When you load the scene, the random color from them is selected. That creates a unique experience for every time the player starts a level!

## Dark red color adjustments when one life left

When the player is close to game over, you can apply a special post-processing color adjustment.
Add Override -> Color Adjustments and set the Color Filter parameter to dark red:
![ ](https://i.imgur.com/X3VtS2e.png)

Uncheck this effect in the inspector:
![ ](https://i.imgur.com/ZOarwtf.png)

    private void Update()
    {
        if (FindObjectOfType<GameManager>().lives == 0)
        {
            if (this.GameVolume.TryGet<ColorAdjustments>(out ColorAdjustments colorAdjustments))
            {
                colorAdjustments.active = true;
            }
        }
    }
***Note:** it is not optimal to use the expensive FindObjectOfType<>() in the Update function but I do it this way for the simplicity of this tutorial.*

Deactive the colorAdjustments every time the script starts (Start function):

        if (this.GameVolume.TryGet<ColorAdjustments>(out ColorAdjustments colorAdjustments))
        {
            colorAdjustments.active = false;
        }

![ ](https://i.imgur.com/VCx4BcI.png)




---
