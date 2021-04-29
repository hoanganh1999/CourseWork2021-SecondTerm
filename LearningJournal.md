# Learning Journal-SecondTerm

## 3/3/2021
Today I have made an attempt to create low poly water by code in unity, I started this by making a script(WaterPlaneGen) to create a plane that we can use to control the vertices and I also created a script called MakeSomeNoise, this script is the script that controls the water and manipulates the vertices. When I was working on the first script which is the WaterPlaneGen I had encountered a problem that stopped my script from working. The problem was that in the GenerateMesh function, instead of writing "Vector3", I wrote "vector3" for all of them. Solve this problem I just went back and change the "vector3" to Vector3. I also change the line "var uvs = List<Vector3>();" to "var uvs = List<Vector2>();" this because uvs is a flat plane and it only has 2 values(x, z).
    
    private Mesh GenerateMesh()
    {
        Mesh m = new Mesh();
        var verticies = new List<Vector3>(); //stores x, y, z
        var normals = new List<Vector3>();
        var uvs = new List<Vector2>();  //only has 2 values (x, z)


## 5/3/2021
I have tried to generate a plane using code in unity, it was hard to learn and understand all the logic and math behind it. But in the end, I managed to get it to work, I did this by splitting the plane into 2 separate triangles. 

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
I managed to finish both scripts(MakeSomeNoise,WaterPlaneGen), I tried to put in work together in unity, but it didn't work at first and it gave me a red error. This was because I didn't add a Mesh Filter component and a Mesh Renderer component. I managed to fix this error by adding these 2 components to my empty object.

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
I attempted to make my Moving Platform/Elevator package, but it didn't go very well for me. This is because when I tried to make the platform move, it didn't move at all. this is because in my update function I failed to find reference the T and Y keys in the if statement. I wrote if (Input.GetKey (KeyCode.T) && hasRider) instead of if (Input.GetKeyDown (KeyCode.T) && hasRider). I fixed this problem by replacing GetKey with GetKeyDown.


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
Today I was still working on my Moving Platform/Elevator package. I encountered another problem where the player didn't stay on the moving platform when it moved. This was because if the second collider of the moving platform wasn't set to is Trigger and I didn't reference the player correctly because I said "if (coll.tag == "Object")" instead of "if (coll.tag == "Player")". I fixed the problem by setting the collider on my moving platform object to is Trigger and corrected the code in both OnTriggerEnter and OntriggerExit functions. I have learnt that tagging is very important in coding.

Corrected code:

    void OnTriggerEnter(Collider coll)
    {
        if (coll.tag == "Player")
        {
           
            coll.transform.parent = gameObject.transform;
            hasRider = true;
        }
    }

    void OntriggerExit(Collider coll)
    {
        if (coll.tag == "Player")
        {
            
            coll.transform.parent = null;
            hasRider = false;
        }
    }


    
## 26/3/2021
I wanted to have a canvas that contained the instruction for the Moving Platform/Elevator that show up when the player touched the platform. I had some problems with trying to make it work but after using the true and false statement I managed to make it work. I was pretty happy with the result.

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

## 1/4/2021
I had another problem while creating a jump pad. The problem was that the player wasn't able to bounce on the jump pad. This is because I added a Rigidbody in the jump pad object but I didn't reference the Rigidbody in the code. I fixed this problem by adding Rigidbody rb = bouncer.GetComponent<Rigidbody>(); in the OncollisionEnter function. 

    
    private void OncollisionEnter(Collision collision)
    {
        GameObject bouncer = collision.gameObject;
        
        Rigidbody rb = bouncer.GetComponent<Rigidbody>();
        
        rb.AddForce(Vector3.up * bounceDistance);
    }


## 5/4/2021
Today I encountered a problem where my PickUp script didn't reset when the player died. This issue was very problematic because when the player died and the game restarted, the player wouldn't be able to press E to pick up the object or Q to drop the object again. I figured out a solution and managed to fix it by adding  "equipped = false;" and "slotFull = false;" at the Start function, this was to make sure that the code reset every time the game restart.


    private void Start()
    {
        equipped = false;
        slotFull = false;

        if (!equipped)
        {
            rb.isKinematic = false;
            col.isTrigger = false;
        }

        if (equipped)
        {
            rb.isKinematic = true;
            col.isTrigger = true;
            slotFull = true;

        }
    }


## 7/4/2021
I have tried to figure out a way to make the pickup object as a child of the container object. It took me a long time but I made it work using the transform.SetParent, transform.localposition and trasnform.localScale.   

    private void PickUp()
    {
        equipped = true;
        slotFull = true;

        transform.SetParent(container);
        transform.localPosition = Vector3.zero;
        transform.localRotation = Quaternion.Euler(Vector3.zero);
        transform.localScale = Vector3.one;


        rb.isKinematic = true;

        col.isTrigger = true;

    }

## 12/4/2021
I also wanted to make the character to be able to drop the object as well, I made this work by adding a Drop function. I used the rb.AddForce method to make the player throw the object when pressed Q. I also used "float random = Random.Range(-1f, 1f);" to make the player throw the object at random range as well.

    private void Drop()
    {
        equipped = false;
        slotFull = false;

        transform.SetParent(null);


        rb.isKinematic = false;

        col.isTrigger = false;

        rb.velocity = player.GetComponent<Rigidbody>().velocity;

        rb.AddForce(cam.forward * dropForwardForce, ForceMode.Impulse);
        rb.AddForce(cam.up * dropUpwardForce, ForceMode.Impulse);

        float random = Random.Range(-1f, 1f);
        rb.AddTorque(new Vector3(random, random, random) * 10);
    }


## 15/4/2021
I wanted to get the Tornado to pull the player in slowly so I added an IEnumerator pullObject function. In this IEnumerator pullObject function, there was an if statement that when objects with tag player come close, it would pull that object in.  

    IEnumerator pullObject(Collider x, bool shouldPull)
    {
        if (shouldPull)
        {
            Vector3 ForeDir = tornadoCenter.position - x.transform.position;
            x.GetComponent<Rigidbody>().AddForce(ForeDir.normalized * pullforce * Time.deltaTime);
            yield return refreshRate;
            StartCoroutine(pullObject(x, shouldPull));
        }
    }



## 18/4/2021
Today I attempted to instantiate a particle effect whenever a bullet touched the target object. I did this by making a HitTarget function and in this function I added GameObject effectIns = (GameObject)Instantiate(impactEffect, transform.position, transform.rotation); to make this work.

    void HitTarget()
    {
        GameObject effectIns = (GameObject)Instantiate(impactEffect, transform.position, transform.rotation);
        Destroy(effectIns, 2f);

        Destroy(target.gameObject);

        MyEnemies = GameObject.FindGameObjectWithTag("Soul");
        DestroyObject(MyEnemies);

        Destroy(this.gameObject);


        Debug.Log("HITTING!");
        Destroy(gameObject);
    }

