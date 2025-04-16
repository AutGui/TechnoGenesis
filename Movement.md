# Movement

This Movement was inspired by the following video:

[![image](https://github.com/user-attachments/assets/986c7849-0f2d-4467-966c-0df47f2930e9)](https://www.youtube.com/watch?v=ImuCx_XVaEQ)

Even though this video is in an older version of Unity, most of the things still apply to the [version](https://github.com/AutGui/TechnoGenesis/blob/main/README.md#unity) I am using.

<br>

For the movement, we plan to do the following mechanics:

- [WASD to move](#player-movement)
- [Space to jump](#player-movement)

## Player Movement

For movement, I decided to put the new unity version to use by using the Input Actions.

The Input Actions was already available in other versions, but as a package, which needed to
be installed through Unity's Package Manager.
It's good to see Unity bringing some great features into the engine as default.

### Input Actions

I created a new Input Actions named **Player Controller Input**.

The way I did the movement in the Input Actions was very simple:

- Created an Action Map named MainMovement
  
    - Inside I created an Action called **Movement**, this action's type is Pass Through and the control type is Vector 2.<br>
      The reason to use a Vector 2 as control type is because the key maping is 2D, we receive 2D values (Front, Left, Right and Back), there needs to be the previous mentioned, and "Up" or "Down" values to be considered 3D, which would need a Vector3 control type.
      
      ---

      <p float = "left">
        <img align = "top" src = "https://github.com/user-attachments/assets/6b2788dc-87e4-4388-8f02-0ca28a163a72">
        <img src = "https://github.com/user-attachments/assets/83d24000-37f8-4ead-9955-c67eabbcc416">
      </p>
      
      ---
      
      - Inside **Movement** I added an Up\Down\Left\Right Composite named **WASD**, which adds 4 binds following the Up - Down - Left - Right order, I then chose a path to which those 4 binds corresponded and put them in the order W - S - A - D.

        ---

        <img src = "https://github.com/user-attachments/assets/690cc156-7f9b-4ec6-9ab2-64bdcba519f8">

        ---

    - Created an Action called **Jump**, with the path being the Space key.

      ---

      ![image](https://github.com/user-attachments/assets/863867a1-d233-4dad-ba2d-eca8ef9680db)

      ---

    - And Created an Action with the name **Mouse**, the action's type is Pass Through with the control type being Vector 2.<br>
      The reason behind the use of Vector 2 for the mouse's movement is the same reason to why I used the Vector 2 in the **Movement** action, when using the mouse, we receive 2D values (Up, Left, Down and Right).

      ---

      <p float = "left">
        <img align = "top" src = "https://github.com/user-attachments/assets/afa986d3-09f0-441c-a0c6-8fd6b3f7bf1c">
        <img src = "https://github.com/user-attachments/assets/99b02aad-e7b8-466c-b01c-8dbf7bee6c8c">
      </p>

      ---

      - Inside the **Mouse** I put the bind's path as the mouse's Delta, which corresponds to the mouse's position.

        ---

        ![image](https://github.com/user-attachments/assets/dde994e9-3402-4094-a76a-79fe9ced6d9d)

        ---

### Input Manager

After I finished with the Input Actions I switched Generate a C# Class to on and created a new MonoBehaviour Script named **Input Manager**.

The whole purpose of the **Input Manager** is to get the values from the [**Player Controller Input**](#input-actions).

I use the **Awake** method to make sure that, when starting the script, there isn't any other isntance of the InputManager open, and if so, to destroy this one so there are no conflicts.

```cs
    // Starts when being loaded
    void Awake()
    {
        if (_instance != null && _instance != this)
        {
            Destroy(this.gameObject);
        }
        else
        {
            _instance = this;
        }
        
        PlayerController = new PlayerControllerInput();
    }
```

I also create two new methods, one called **OnEnable**, and another one called **OnDisable**, so in case the **Input Manager** is disabled, the [**Player Controller Input**](#input-actions) is also disabled

```cs
    // Called when the script is enabled  //|
    void OnEnable()                       //|
    {                                     //|
        PlayerController.Enable();        //|- Input can be disabled or enabled when the script is disabled or enabled //
    }                                     //|
                                          //|
    // Called when the script is disabled //|
    void OnDisable()
    {
        PlayerController.Disable();
    }
```

### Player Controls

Now that I had the input system completed I created a MonoBehaviour Script named **Player Controls**.

<br>

In this case I decided that the Player's speed would be 2.0f, the Jump height at 1.5f and the Gravity at -9.0f.

```cs
    [SerializeField]
    float PlayerSpeed = 2.0f;

    [SerializeField]
    float JumpHeight = 1.5f;

    [SerializeField]
    float Gravity = -9.0f;
```

<br>

I also use a simple way to check if the player is grounded, since the Character Controller already has an `isGrounded` boolean which checks if the bottom of the character controller is in contact with any collider.

```cs
bool IsGrounded;
```

```cs
IsGrounded = PlayerController.isGrounded;
```

