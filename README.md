using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class HamsterController : MonoBehaviour
{
    public float speed = 5f;
    public float jumpForce = 7f;
    private Rigidbody2D rb;
    private bool isGrounded = true;
    public Transform groundCheck;
    public LayerMask groundLayer;
    public Animator animator;

    void Start()
    {
        rb = GetComponent<Rigidbody2D>();
    }

    void Update()
    {
        Move();
        Jump();
        Attack();
    }

    void Move()
    {
        float moveInput = Input.GetAxis("Horizontal");
        rb.velocity = new Vector2(moveInput * speed, rb.velocity.y);
        
        // Flipping character when moving
        if (moveInput > 0)
        {
            transform.localScale = new Vector3(1, 1, 1);
        }
        else if (moveInput < 0)
        {
            transform.localScale = new Vector3(-1, 1, 1);
        }

        // Animation
        animator.SetFloat("Speed", Mathf.Abs(moveInput));
    }

    void Jump()
    {
        isGrounded = Physics2D.OverlapCircle(groundCheck.position, 0.1f, groundLayer);
        animator.SetBool("isGrounded", isGrounded);

        if (Input.GetKeyDown(KeyCode.Space) && isGrounded)
        {
            rb.velocity = new Vector2(rb.velocity.x, jumpForce);
            animator.SetTrigger("Jump");
        }
    }

    void Attack()
    {
        if (Input.GetKeyDown(KeyCode.Z))
        {
            animator.SetTrigger("Attack");
            // Add attack logic here
        }
    }
}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hamster Kombat</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="game-container">
        <div class="player" id="player1">Player 1</div>
        <div class="player" id="player2">Player 2</div>
    </div>
    <div class="controls">
        <button id="attackBtn1">Player 1 Attack</button>
        <button id="attackBtn2">Player 2 Attack</button>
    </div>
    
    <script src="game.js"></script>
</body>
</html>
public class ComboSystem : MonoBehaviour
{
    public Animator animator;
    private float lastAttackTime;
    private int comboStep = 0;
    public float comboDelay = 1f;

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Z))
        {
            if (Time.time - lastAttackTime < comboDelay)
            {
                comboStep++;
                if (comboStep > 3) comboStep = 0;
            }
            else
            {
                comboStep = 1;
            }
            lastAttackTime = Time.time;

            switch (comboStep)
            {
                case 1:
                    animator.SetTrigger("Attack1");
                    break;
                case 2:
                    animator.SetTrigger("Attack2");
                    break;
                case 3:
                    animator.SetTrigger("Attack3");
                    break;
            }
        }
    }
}


