# CourseWork2021-SecondTerm

## 3/3/2021
Today I have made an attempt to create low poly water by code in unity, I started this by making a script(WaterPlaneGen) to create a plane that we can use to control the vertices and I also created a script called MakeSomeNoise, this script is the script that actually control the water and malipulate the vertices. When I was working on the first script which is the WaterPlaneGen I had encouter a problem that stopped my script from working. The problem was that in the GenerateMesh function, instead of writing "Vector3", I wrote "vector3" for all of them. Solve this problem I just went back and change the "vector3" to Vector3. I also change the line "var uvs = List<Vector3>();" to "var uvs = List<Vector2>();" this because uvs is a flat plane and it only has 2 values(x, z).
    
    private Mesh GenerateMesh()
    {
        Mesh m = new Mesh();
        var verticies = new List<Vector3>(); //stores x, y, z
        var normals = new List<Vector3>();
        var uvs = new List<Vector2>();  //only has 2 values (x, z)


## 5/3/2021
I have tried to generate a plane using code in unity, it was hard to learn and understand all the logic and math behind it. But at the end I managed to get it to work, I did this by splitting the plane into 2 seperate triangles. 

        var triangles = new List<int>();
        var vertCount = gridSize + 1;
        for (int i = 0; i < vertCount * vertCount - vertCount; i++)
        {
            if ((i + 1) % vertCount == 0)
            {
                continue;
            }
            triangles.AddRange(new List<int>() {
                i+1+vertCount, i+vertCount, i,
                i, i+1, i+vertCount+1
            });
            
            

## 9/3/2021
Today I tried to make the MakeSomeNoise script, I had a problem where I couldn't call the CalculateHeight function which was very confusing. This was because instead of CalculateHeight, I had it as claculateHeight. I fixed this problem by going back and change the line "float claculateHeight(float x, float y) to "float CalculateHeight(float x, float y). 

    float CalculateHeight(float x, float y)
    {
    

## 13/3/2021
I managed to finish both scripts(MakeSomeNoise,WaterPlaneGen), I tried to put in work together in unity, but it didn't work at first and it gave me a red error. This was because I didn't add a Mesh Filter component and a Mesh Renderer component. I managed to fix this error by adding these 2 components in my empty object.



## 15/3/2021
I had a problem when I was making my first package which was the Character Movement package. The problem was that I made a variable for the Character Controller but I didn't add a Character Controller to the object and it didn't let me start the game when I clicked play. I fixed this problem by adding a Character Controller to the game object.

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerMovement : MonoBehaviour
{

    Vector3 MoveVector = Vector3.zero;
    CharacterController characterController;

    public float moveSpeed;
    
   

## 19/3/2021
The script of my character still didn't work the way that I want it to work, I wanted my character to be able to jump so I added the jumpSpeed and the gravity variables. I also added this line of code to the update function: 

if (characterController.isGrounded && Input.GetButton("Jump"))
            MoveVector.y = jumpSpeed;    
            
This line of code allows the character to jump whenever the CharacterController touches the ground.
            

## 22/3/2021
I attemped to make my Moving Platform/Elevator package, but it didn't go very well for me. This is because when I tried to make the platform to move, it didn't move at all. this is because in my update function I failed to find reference the T and Y keys in the if statement. I wrote if (Input.GetKey (KeyCode.T) && hasRider) instead of if (Input.GetKeyDown (KeyCode.T) && hasRider). I fixed this problem by replacing GetKey with GetKeyDown.


void Update()
    {
    
        if (Input.GetKeyDown (KeyCode.T) && hasRider)
        {
            states = EleStates.goUP;
        }

        if (Input.GetKeyDown(KeyCode.Y) && hasRider)
        {
            states = EleStates.goDown;
        }





## 24/3/2021
Today I was still working on my Moving Platform/Elevator package. I encoutered another problem where the player didn't stay on the moving platform when it moved. This was because if the second collider of the moving platform wasn't set to is Trigger and I didn't reference the player corretly because I said "if (coll.tag == "Object")" instead of "if (coll.tag == "Player")". I fixed the problem by setting the collider to is Trigger and corrected the code in both OnTriggerEnter and OntriggerExit.

    void OnTriggerEnter(Collider coll)
    {
        if (coll.tag == "Player")
        {
            platformPanel.SetActive(true);
            coll.transform.parent = gameObject.transform;
            hasRider = true;
        }
    }

    void OntriggerExit(Collider coll)
    {
        if (coll.tag == "Player")
        {
            platformPanel.SetActive(false);
            coll.transform.parent = null;
            hasRider = false;
        }
    }


    
## 24/11/20
The problem that I encountered while making the script for my Shooting Enemy AI tutorial was that I couldn't get the enemy to shoot projectiles at the player, the enemy was creating projectiles but it didn't shoot at the player so what I did was making a new script for the projectile and told it to target the player by "player = GameObject.FindGameObjectWithTag("Player").transform;" 

## 26/11/20
I had another problem where the projectile doesn't disappear when it collides with the player. I fixed this by saying that when whenever the projectile touches the object with tag "Player", it will destrory itself.

void OntriggerEnter2D(Collider2D other) {

    if (other.CompareTag("Player")){
        DestroyProjectile();
    }
}

## 28/11/2020
I had an issue with the game for the coding model where I couldn't get my character to jump. I made this work by adding a game public called "groundcheck" and say that whenever the character reaches the ground it will be able to jump again.


grounded = Physics.Raycast(groundcheck.transform.position, Vector2.down, 0.1f, ground);

        if (Input.GetAxis("Jump") != 0 && grounded == true)
        {
            rb.velocity = new Vector3(rb.velocity.x, JumpSpeed, rb.velocity.z);
            
        }
    }


## 30/11/2020
I had another problem with my game where the camera was going through the wall whenever the character is near the wall, this caused the wall to block the view of the player. I managed to fix the problem by using raycast method and said that whenever the raycast hit the wall, the mesh of the wall would get disabled which allowed the player to see through the wall.

RaycastHit hit;

        if (Physics.Raycast(transform.position, Target.position - transform.position, out hit, 4.5f))
        {
            if (hit.collider.gameObject.tag != "Player")
            {
                Obstruction = hit.transform;
                Obstruction.gameObject.GetComponent<MeshRenderer>().shadowCastingMode = UnityEngine.Rendering.ShadowCastingMode.ShadowsOnly;

                if (Vector3.Distance(Obstruction.position, transform.position) >= 3f && Vector3.Distance(transform.position, Target.position) >= 1.5f)
                {
                    transform.Translate(Vector3.forward * zoomSpeed * Time.deltaTime);
                }

            }
            else
            {
                Obstruction.gameObject.GetComponent<MeshRenderer>().shadowCastingMode = UnityEngine.Rendering.ShadowCastingMode.On;
                if (Vector3.Distance(transform.position, Target.position) < 4.5f)
                {
                    transform.Translate(Vector3.back * zoomSpeed * Time.deltaTime);
                }
            }
        }
    }

## 2/12/2020
I encountered another problem where the player would clip through objects whenever they tried to fly toward the object, this problem is in my grappling hook/flying script. I fixed this problem by problem by going in my script and change the value of of the grapplespeed variable from 10.0f to 6.8f, this stopped the character from going through the wall or any other objects.

using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class GrappleHook : MonoBehaviour
{

     private Rigidbody rb;
     public float speed;
     private PlayerController movementScript;
     public float grappleDistance;
     public float grappleSpeed = 6.8f;

## 3/11/2020
I had another problem where the enemy didn't chase and attack the player when it was closed to the player instead it was just patrolling around that area. I fixed it by saying that whenever the playerDistance is smalled than 2f, the enemy will chase the player but if the playerDistance is more than 2f, the enemy will move to the next point.

playerDistance = Vector3.Distance(player.position, transform.position);

        if (playerDistance < awareAI)
        {
            LookAtPlayer();
            Debug.Log("Seen");
        }

        if (playerDistance < awareAI)
        {
            if (playerDistance > 2f)
            chase();
                else
            GotoNextPoint();
        }


## 4/12/2020
I tried to fix the problem with the UI of coins where the UI doesn't change even if the character had collected all the coins in the map, the number of coins would just stay at 1 and it wouldn't go to 0. I managed to fix this problem by going in my gameManagerScript and say that if the number of coins is less than or equal to 0 the UI will change.

public void UpdateUI() 
    {
        
        if (cur_coins > 0) 
        {
            coinsLeft.text = "coin Left: " + cur_coins.ToString("D1") + "/" + max_coins.ToString("D1");
        } else if (cur_coins <= 0) {
            coinsLeft.text = "coin Left: " + cur_coins.ToString("D1") + "/" + max_coins.ToString("D1");
            Door.SetActive (true);
        }
       
    }
 
## 6/12/2020
I tried to use true or false function to make my animation worked and after sometimes of trying to make it work, I managed to make the animation played whenever a key is pressed. It is better to map out which animation is supposed to play after each key is pressed before trying to make the script for the animation.
