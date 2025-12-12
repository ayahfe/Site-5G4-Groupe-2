+++
title = "Atelier"
weight = 1
+++

### 1 Création des murs

J’ai utilisé des cubes et j’ai changé les dimensions afin de créer les “murs” de la table. J’en ai ajouté 4 pour les 4 coins de la table.

Un autre cube, plus large et plus mince, a été utilisé pour créer la table. Sa hauteur a été réduite afin d'imiter l'effet d'une vraie table de billard.

---

## 2. Caméra de jeu

Il faut ajouter une caméra sur le haut de la table afin de voir pendant le jeu le haut de la table. Cependant, il faut choisir le bon angle afin que les balles ne soient pas déformées.

---

## 3. Les balles

### 3.1 Disposition des balles

J’ai disposé les balles comme un vrai jeu de billard (avec une sphère par balle).

### 3.2 Différenciation des joueurs

J’ai mis certaines balles en roses et certaines balles en bleues afin de différencier les balles des deux joueurs. J'en ai aussi mis une noire pour la "8e balle" qu'ils devront rentrer après avoir rentré toute leurs balles.

---

## 4. Création des trous (pockets)

J’ai mis 6 balles noires sur le côté en guise de trous.
Il a faluu activé le isTrigger afin qu'ils puissent détecter lorsqu'une balle touche le trou et puisse disparaitre (il a fallu ajouter un component: le rigidbody).

---

## 5. Contraintes physiques des balles

Pour les autres balles j’ai mis ces contraintes dans le rigidbody afin qu’elles roulent sur la table et non qu’elles sautent.

- Freeze Position Y
- Freeze Rotation X
- Freeze Rotation Z

---

## 6. Respawn de la balle blanche

J’ai aussi créé un CueBallSpawn pour que la balle blanche réapparaisse après qu’elle tombe dans un trou.

## 7. Codes

Il a fallu créer un script: CueBallAim.cs afin de créer la ligne pour viser les balles selon la position du curesuer.

```csharp
using UnityEngine;

public class CueBallAim : MonoBehaviour
{
    public LineRenderer line;
    public float maxDistance = 5f;

    private void Update()
    {
        line.SetPosition(0, transform.position);
        Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
        if(Physics.Raycast(ray, out RaycastHit hit))
        {
            Vector3 direction = (hit.point - transform.position).normalized;
            line.SetPosition(1, transform.position + direction*maxDistance);
        }
    }
}
```

Ensuite j'ai du créer CueBallShoot.cs pour changer la force du tir selon le nombre de temps appuyer sur la souris.

```csharp
using UnityEngine;

public class CueBallShoot : MonoBehaviour
{
    public float maxForce = 50f;
    public float chargeSpeed = 40f;

    private float currentForce = 0f;
    private bool charging = false;
    private Rigidbody rb;

    private GameManager gm;

    void Start()
    {
        rb = GetComponent<Rigidbody>();
        gm = GameObject.Find("GameManager").GetComponent<GameManager>();
    }

    void Update()
    {

        if (rb.linearVelocity.magnitude > 0.1f)
            return;

        // Début du tir
        if (Input.GetMouseButtonDown(0))
        {
            charging = true;
            currentForce = 0f;
        }

        // Charge la force
        if (charging && Input.GetMouseButton(0))
        {
            currentForce += chargeSpeed * Time.deltaTime;
            currentForce = Mathf.Clamp(currentForce, 0, maxForce);
        }

        // Tire quand on lâche
        if (charging && Input.GetMouseButtonUp(0))
        {
            charging = false;

            Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
            if (Physics.Raycast(ray, out RaycastHit hit))
            {
                // Direction horizontale propre
                Vector3 dir = (hit.point - transform.position);
                dir.y = 0;
                dir = dir.normalized;

                rb.AddForce(dir * currentForce, ForceMode.Impulse);

                gm.Invoke("EndOfTurn", 1f);
            }

            currentForce = 0f;
        }
    }
}
```

```csharp
Il y a aussi GameManager.cs qui sert à gérer la logique du jeu avec les tours des joueurs, l'attribution des couleurs, valider si le joueur à entrer la bonne ou la mauvaise balle, le changement de joueur, etc..
using UnityEngine;
using UnityEngine.SceneManagement;
using TMPro;

public class GameManager : MonoBehaviour
{
    public Transform cueBallSpawnPoint;
    public GameObject cueBallPrefab;

    public TextMeshProUGUI playerTurnText;
    public TextMeshProUGUI messageText;
    public TextMeshProUGUI typesText;

    public int currentPlayer = 1;

    public string player1Color = "";
    public string player2Color = "";
    public bool colorsAssigned = false;

    private bool ballPocketedThisTurn = false;
    public bool HadBallPocketedThisTurn()
    {
        return ballPocketedThisTurn;
    }

    //

    void Start()
    {
        SpawnCueBall();
        UpdatePlayerTurnUI();
        UpdateTypesUI();
    }

    void SpawnCueBall()
    {
        GameObject oldCue = GameObject.FindWithTag("CueBall");
        if (oldCue != null) Destroy(oldCue);

        Instantiate(cueBallPrefab, cueBallSpawnPoint.position, Quaternion.identity);


        ballPocketedThisTurn = false;
    }

    public void BallPocketed(GameObject ball)
    {
        string name = ball.name;
        Destroy(ball);


        ballPocketedThisTurn = true;

        // Faute si balle blanche tombe
        if (name.Contains("Cue"))
        {
            messageText.text = "Faute ! Joueur " + currentPlayer + " perd son tour.";
            SwitchPlayer();
            Invoke(nameof(SpawnCueBall), 1f);
            return;
        }

        // Faute si balle noire tombe
        if (name.Contains("8"))
        {
            if (!colorsAssigned)
            {
                messageText.text = "La noire trop tôt ! Joueur " + currentPlayer + " perd.";
                Win(Opponent(currentPlayer));
                return;
            }

            if (PlayerClearedAll(currentPlayer))
            {
                Win(currentPlayer);
            }
            else
            {
                messageText.text = "La noire trop tôt ! Joueur " + currentPlayer + " perd.";
                Win(Opponent(currentPlayer));
            }
            return;
        }


        string color = ExtractColor(name);
        if (color == "") return;


        if (!colorsAssigned)
        {
            AssignColors(currentPlayer, color);
            messageText.text = "J" + currentPlayer + " = " + color;
            UpdateTypesUI();
        }



        if (!IsCorrectBall(currentPlayer, color))
        {
            messageText.text = "Mauvaise balle → changement de joueur";
            SwitchPlayer();
        }
        else
        {
            messageText.text = "Bonne balle ! Rejoue Joueur " + currentPlayer;
        }
    }


    public void EndOfTurn()
    {
        if (!ballPocketedThisTurn)
        {
            messageText.text = "Aucune balle rentrée → Joueur " + currentPlayer + " perd son tour";
            SwitchPlayer();
        }

        ballPocketedThisTurn = false;
    }
    void AssignColors(int player, string color)
    {
        colorsAssigned = true;

        if (player == 1)
        {
            player1Color = color;
            player2Color = (color == "pink") ? "blue" : "pink";
        }
        else
        {
            player2Color = color;
            player1Color = (color == "pink") ? "blue" : "pink";
        }
    }

    string ExtractColor(string name)
    {
        string lower = name.ToLower();

        if (lower.Contains("pink")) return "pink";
        if (lower.Contains("blue")) return "blue";

        return "";
    }

    bool IsCorrectBall(int player, string color)
    {
        if (!colorsAssigned) return true;

        return (player == 1 && color == player1Color)
            || (player == 2 && color == player2Color);
    }

    bool PlayerClearedAll(int player)
    {
        string neededColor = (player == 1) ? player1Color : player2Color;

        foreach (GameObject ball in GameObject.FindGameObjectsWithTag("Ball"))
        {
            string name = ball.name.ToLower();

            if (name.Contains("8")) continue;

            if (neededColor == "pink" && name.Contains("pink"))
                return false;

            if (neededColor == "blue" && name.Contains("blue"))
                return false;
        }

        return true;
    }

    int Opponent(int player) => (player == 1 ? 2 : 1);

    void SwitchPlayer()
    {
        currentPlayer = Opponent(currentPlayer);
        UpdatePlayerTurnUI();
        UpdateTypesUI();
    }

    void UpdatePlayerTurnUI()
    {
        playerTurnText.text = "Joueur : " + currentPlayer;
    }

    void UpdateTypesUI()
    {
        if (!colorsAssigned)
        {
            typesText.text = "Couleurs : Non assignées";
            return;
        }

        typesText.text = "J1 : " + player1Color + "    J2 : " + player2Color;
    }

    void Win(int player)
    {
        messageText.text = "Victoire de Joueur " + player + " !";
        Invoke(nameof(RestartGame), 2f);
    }

    void RestartGame()
    {
        SceneManager.LoadScene(SceneManager.GetActiveScene().name);
    }
}
```

Rotator.cs, lui sert à pouvoir faire tourner l'objet:

```csharp
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class Rotator : MonoBehaviour
{

    void Update()
    {
        transform.Rotate(new Vector3(15, 30, 45)*Time.deltaTime);
    }
}
```

Enfin, Trous.cs détecte lorsqu'une balle est dans un trou et si c'est une balle normale ou la balle blanche :

```csharp
using UnityEngine;

public class Trous : MonoBehaviour
{
    private void OnTriggerEnter(Collider other)
    {
        if (other.CompareTag("Ball") || other.CompareTag("CueBall"))

        {
            GameObject gm = GameObject.Find("GameManager");
            gm.GetComponent<GameManager>().BallPocketed(other.gameObject);
            Destroy(other.gameObject);
        }
    }
}
```

[ Télécharger le projet Unity / Billard (ZIP)](https://gofile.io/d/VWZefJ)
