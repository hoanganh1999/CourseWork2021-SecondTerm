# CourseWork2021-SecondTerm

## 13/11/2020
Today I have made an attempt to create low poly water by code in unity, I started this by making a script(WaterPlaneGen) to create a plane that we can use to control the vertices and I also created a script called MakeSomeNoise, this script is the script that actually control the water and malipulate the vertices. When I was working on the first script which is the WaterPlaneGen I had encouter a problem that stopped my script from working. The problem was that in the GenerateMesh function, instead of writing "Vector3", I wrote "vector3" for all of them. Solve this problem I just went back and change the "vector3" to Vector3. I also change the line "var uvs = List<Vector3>();" to "var uvs = List<Vector2>();" this because uvs is a flat plane and it only has 2 values(x, z).
    
    private Mesh GenerateMesh()
    {
        Mesh m = new Mesh();
        var verticies = new List<Vector3>(); //stores x, y, z
        var normals = new List<Vector3>();
        var uvs = new List<Vector2>();  //only has 2 values (x, z)


## 15/11/2020
I have tried to generate a plane using code in unity, it was hard to learn and understand all the logic behind it. But at the end I managed to get it to work, I did this by splitting the plane into 2 seperate triangles. 

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
            
            

## 16/11/2020
I tried to implement the raycast method in my grappling hook/flying script and after a few tries, I manage to make it worked.  

IEnumerator Grapple1()
    {
        
        RaycastHit hit;
        Physics.Raycast(Camera.main.transform.position, Camera.main.transform.forward, out hit, grappleDistance);
        if (hit.transform)
        {
            rb.useGravity = false;
            Vector3 target = hit.transform.position;
            movementScript.enabled = false;
            rb.velocity = (target - transform.position) * grappleSpeed;
        }
        else
        {
            rb.useGravity = true;
        }
        yield return null;
    }

## 18/11/2020
I tried to use raycast on my camera to make sure whenever the camera is behind the wall, the mesh of the wall would be turned off. it didn't work out because I put "payer" instead of "Player" in the code "if (hit.collider.gameObject.tag != "Player")".

## 20/11/2020
I had a problem when I was making my first tutorial which was the 2D Top Down Player Control Tutorial. The problem was that I made a variable for the rigidbody 2D but I didn't put a rigidbody on the player character and this stopped my game from working. I fixed this problem by adding a rigidbody 2D onto the player character.

using System.Collections;

using System.Collections.Generic;

using System.Runtime.CompilerServices;

using UnityEngine;

public class PlayController : MonoBehaviour {

     public float speed;

     private Rigidbody2D rb;

     private Vector2 moveVelocity;

## 23/11/2020
I had a problem with my second tutorial which was my 2D Enemy AI Follow tutorial. The problem that I encountered was that the enemy wouldn't follow the player even when I tagged the player with the "Player" tag. The enemy didn't follow the player because in the script where it said target = "GameObject.FindGameObjectWithTag("Player").GetComponent<Transform>();" I wrote "player" instead of "Player". I fixed this by putting "Play" in the line "GameObject.FindGameObjectWithTag("Player").GetComponent<Transform>();"
    
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
