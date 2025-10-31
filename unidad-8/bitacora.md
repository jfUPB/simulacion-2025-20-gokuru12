# Evidencias de la unidad 8

### Actividad 2 Diseño

__Cancion elegida__: https://youtu.be/gTMWO7ovAXk?si=sVO1BWWxmQJVK90p

__Concepto visual__: La idea es representar las distintas cosas que pueden salir en el cielo que sale en la portada mientras se escucha la canción, unas estrellas moviendose, unas luciernagas queriendo unirse y a lo mejor un viaje más psicodelico, todo es lo que puede ver una persona en el cielo dependiendo de lo que ocurra.

__Inputs__: Esferas de colores azules, grises y blancos, como las estrellas que permitan sentirse explorando, algunas junto al mouse serán un poco rojas, como si te concentrarás en un sol joven

Esferas que actuan de luciernagas subiendo por el cielo que además tienen un deseo que cuando el mouse se acerca a ellos sienten atracción por el, pero solo es un deseo ellas quieren seguir subiendo

Esferas que crecen y bajan multiples veces cambiando de color, se mueven de manera aleatoria y puedes destruirlas para seguirse multiplicando

__técnicas usadas__: random, flocking, físicas, lerp de colores, mouse

### Apply 

<details> 

<summary> Aquí esta el código</summary>

```csharp
using UnityEngine;
using System.Collections.Generic;

public enum GameMode { Estrellas, Sondeo, Bombeo }
public enum Tempo { Lento, Medio, Rapido }

public class ParticleSystemManager : MonoBehaviour
{
    [Header("Configuración General")]
    public GameMode currentMode = GameMode.Estrellas;
    public Tempo currentTempo = Tempo.Medio;

    [Header("Referencias")]
    public Camera mainCamera;
    public GameObject particlePrefab;

    [Header("Colores de Fondo")]
    public Color[] backgroundColors = new Color[2] {
        new Color(0.2f, 0.1f, 0.3f, 1f),  // Morado oscuro
        new Color(0.1f, 0.1f, 0.4f, 1f)   // Azul oscuro
    };

    private List<GameObject> activeParticles = new List<GameObject>();
    private SpriteRenderer backgroundRenderer;
    private float colorLerpTime = 0f;
    private Vector3 lastMousePosition;
    private bool isRotatingStars = false;
    private float starSpawnTimer = 0f;
    private float probeSpawnTimer = 0f;

    // Velocidades según tempo
    private Dictionary<Tempo, float> tempoSpeeds = new Dictionary<Tempo, float>()
    {
        { Tempo.Lento, 0.5f },
        { Tempo.Medio, 1.0f },
        { Tempo.Rapido, 2.0f }
    };

    void Start()
    {
        SetupBackground();
        InitializeParticles();
    }

    void Update()
    {
        UpdateBackground();
        HandleInput();
        UpdateParticles();
        HandleParticleSpawning();
        lastMousePosition = Input.mousePosition;
    }

    void SetupBackground()
    {
        GameObject bgObject = new GameObject("Background");
        bgObject.transform.SetParent(transform);

        backgroundRenderer = bgObject.AddComponent<SpriteRenderer>();

        Texture2D tex = new Texture2D(1, 1);
        tex.SetPixel(0, 0, Color.white);
        tex.Apply();

        Sprite sprite = Sprite.Create(tex, new Rect(0, 0, 1, 1), new Vector2(0.5f, 0.5f));
        backgroundRenderer.sprite = sprite;
        backgroundRenderer.drawMode = SpriteDrawMode.Sliced;
        backgroundRenderer.sortingOrder = -10;

        // 🔹 Centrar el fondo en la cámara
        bgObject.transform.position = mainCamera.transform.position;
        bgObject.transform.position = new Vector3(bgObject.transform.position.x, bgObject.transform.position.y, 0f);

        // 🔹 Ajustar el tamaño al tamaño visible de la cámara
        float height = 2f * mainCamera.orthographicSize;
        float width = height * mainCamera.aspect;
        backgroundRenderer.size = new Vector2(width, height);
    }


    void UpdateBackground()
    {
        colorLerpTime += Time.deltaTime * 0.3f; // Más rápido el cambio
        float lerpValue = (Mathf.Sin(colorLerpTime) + 1f) / 2f; // Suave transición
        Color currentColor = Color.Lerp(backgroundColors[0], backgroundColors[1], lerpValue);
        backgroundRenderer.color = currentColor;
    }

    void HandleInput()
    {
        if (Input.GetKeyDown(KeyCode.Alpha1)) currentTempo = Tempo.Lento;
        if (Input.GetKeyDown(KeyCode.Alpha2)) currentTempo = Tempo.Medio;
        if (Input.GetKeyDown(KeyCode.Alpha3)) currentTempo = Tempo.Rapido;

        if (Input.GetKeyDown(KeyCode.I)) ChangeMode(GameMode.Estrellas);
        if (Input.GetKeyDown(KeyCode.O)) ChangeMode(GameMode.Sondeo);
        if (Input.GetKeyDown(KeyCode.P)) ChangeMode(GameMode.Bombeo);

        if (currentMode == GameMode.Estrellas && Input.GetMouseButtonDown(0))
        {
            isRotatingStars = true;
        }

        if (Input.GetMouseButtonUp(0))
        {
            isRotatingStars = false;
        }
    }

    void HandleParticleSpawning()
    {
        float speedMultiplier = tempoSpeeds[currentTempo];

        switch (currentMode)
        {
            case GameMode.Estrellas:
                starSpawnTimer += Time.deltaTime * speedMultiplier;
                if (starSpawnTimer >= 1f / 6f) // 6 estrellas por segundo
                {
                    CreateStarParticle();
                    starSpawnTimer = 0f;
                }
                break;

            case GameMode.Sondeo:
                probeSpawnTimer += Time.deltaTime * speedMultiplier;
                if (probeSpawnTimer >= Random.Range(0.1f, 0.5f)) // Aleatorio entre 0.1-0.5 segundos
                {
                    CreateProbeParticle();
                    probeSpawnTimer = 0f;
                }
                break;
        }
    }

    void ChangeMode(GameMode newMode)
    {
        if (currentMode == newMode) return;
        currentMode = newMode;
        ClearAllParticles();
        starSpawnTimer = 0f;
        probeSpawnTimer = 0f;

        if (currentMode == GameMode.Bombeo)
            InitializeParticles(); // 🔹 Añadir esta línea
    }

    void InitializeParticles()
    {
        // Solo inicializar algunas partículas para el modo bombeo
        if (currentMode == GameMode.Bombeo)
        {
            for (int i = 0; i < 15; i++) CreatePumpParticle();
        }
    }

    void UpdateParticles()
    {
        float speedMultiplier = tempoSpeeds[currentTempo];
        Vector3 mouseWorldPos = mainCamera.ScreenToWorldPoint(Input.mousePosition);
        mouseWorldPos.z = 0;

        for (int i = activeParticles.Count - 1; i >= 0; i--)
        {
            if (activeParticles[i] == null)
            {
                activeParticles.RemoveAt(i);
                continue;
            }

            GameObject particle = activeParticles[i];

            switch (currentMode)
            {
                case GameMode.Estrellas:
                    StarParticle star = particle.GetComponent<StarParticle>();
                    if (star != null)
                    {
                        star.UpdateParticle(speedMultiplier, mouseWorldPos);
                        if (isRotatingStars && Input.GetMouseButton(0))
                        {
                            Vector3 mouseDelta = Input.mousePosition - lastMousePosition;
                            star.ApplyRotation(mouseDelta.x * 0.02f); // Rotación más suave
                        }
                    }
                    break;

                case GameMode.Sondeo:
                    ProbeParticle probe = particle.GetComponent<ProbeParticle>();
                    if (probe != null)
                    {
                        probe.UpdateParticle(speedMultiplier, mouseWorldPos);
                        // Remover si está fuera de pantalla
                        if (probe.transform.position.y > 10f)
                        {
                            Destroy(particle);
                            activeParticles.RemoveAt(i);
                        }
                    }
                    break;

                case GameMode.Bombeo:
                    PumpParticle pump = particle.GetComponent<PumpParticle>();
                    if (pump != null) pump.UpdateParticle(speedMultiplier);
                    break;
            }
        }
    }

    void CreateStarParticle()
    {
        GameObject particle = Instantiate(particlePrefab, Vector3.zero, Quaternion.identity);
        particle.AddComponent<StarParticle>();
        activeParticles.Add(particle);
    }

    void CreateProbeParticle()
    {
        // Salir desde diferentes posiciones X en el borde inferior
        Vector3 startPos = new Vector3(Random.Range(-9f, 9f), -6f, 0f);
        GameObject particle = Instantiate(particlePrefab, startPos, Quaternion.identity);
        particle.AddComponent<ProbeParticle>();
        activeParticles.Add(particle);
    }

    void CreatePumpParticle()
    {
        Vector3 randomPos = new Vector3(Random.Range(-8f, 8f), Random.Range(-4f, 4f), 0f);
        GameObject particle = Instantiate(particlePrefab, randomPos, Quaternion.identity);
        particle.AddComponent<PumpParticle>();
        activeParticles.Add(particle);
    }

    void ClearAllParticles()
    {
        foreach (var particle in activeParticles)
        {
            if (particle != null) Destroy(particle);
        }
        activeParticles.Clear();
    }

    public void OnPumpParticleClicked(GameObject pumpParticle)
    {
        if (currentMode == GameMode.Bombeo)
        {
            activeParticles.Remove(pumpParticle);
            Destroy(pumpParticle);
            CreatePumpParticle();
            CreatePumpParticle();
        }
    }
}

using UnityEngine;
using System.Collections.Generic;

public class ProbeParticle : MonoBehaviour
{
    public float riseSpeed = 3f;
    public float mouseAttractionRadius = 3f;
    public float attractionForce = 2f;

    private SpriteRenderer spriteRenderer;
    private List<Vector3> trailPositions = new List<Vector3>();
    private LineRenderer trailRenderer;
    private Color originalColor;
    private float lifeTime = 0f;

    void Start()
    {
        spriteRenderer = GetComponent<SpriteRenderer>();

        trailRenderer = gameObject.AddComponent<LineRenderer>();
        trailRenderer.material = new Material(Shader.Find("Sprites/Default"));
        trailRenderer.startColor = Color.yellow;
        trailRenderer.endColor = Color.clear;
        trailRenderer.startWidth = 0.1f;
        trailRenderer.endWidth = 0f;
        trailRenderer.positionCount = 0;

        originalColor = new Color(
            Random.Range(0.8f, 1f),
            Random.Range(0.6f, 0.8f),
            Random.Range(0.1f, 0.3f),
            1f
        );
        spriteRenderer.color = originalColor;

        float scale = Random.Range(0.2f, 0.4f);
        transform.localScale = Vector3.one * scale;
    }

    public void UpdateParticle(float speedMultiplier, Vector3 mousePosition)
    {
        lifeTime += Time.deltaTime;

        Vector3 newPosition = transform.position + Vector3.up * riseSpeed * speedMultiplier * Time.deltaTime;

        // Atracción hacia el mouse más suave
        float distanceToMouse = Vector3.Distance(transform.position, mousePosition);
        if (distanceToMouse < mouseAttractionRadius)
        {
            Vector3 directionToMouse = (mousePosition - transform.position).normalized;
            // Solo afectar ligeramente la dirección horizontal
            newPosition.x += directionToMouse.x * attractionForce * speedMultiplier * Time.deltaTime * 0.3f;
        }

        transform.position = newPosition;
        UpdateTrail();
    }

    void UpdateTrail()
    {
        trailPositions.Add(transform.position);
        if (trailPositions.Count > 15) trailPositions.RemoveAt(0);

        trailRenderer.positionCount = trailPositions.Count;
        trailRenderer.SetPositions(trailPositions.ToArray());

        // Actualizar colores del trail
        for (int i = 0; i < trailPositions.Count; i++)
        {
            float alpha = (float)i / trailPositions.Count;
            Color trailColor = new Color(originalColor.r, originalColor.g, originalColor.b, alpha * 0.3f);

            if (i == 0) trailRenderer.startColor = trailColor;
            if (i == trailPositions.Count - 1) trailRenderer.endColor = trailColor;
        }
    }
}

using UnityEngine;

public class StarParticle : MonoBehaviour
{
    public float moveSpeed = 2f;
    public float rotationSpeed = 30f;
    public float mouseDetectionRadius = 2f;

    private Vector3 moveDirection;
    private SpriteRenderer spriteRenderer;
    private Color originalColor;
    private float currentRotation = 0f;
    private float lifeTime = 0f;
    private float maxLifeTime = 8f;

    void Start()
    {
        spriteRenderer = GetComponent<SpriteRenderer>();

        // Dirección completamente aleatoria
        moveDirection = new Vector3(
            Random.Range(-1f, 1f),
            Random.Range(-1f, 1f),
            0
        ).normalized;

        Color[] starColors = {
            new Color(0.4f, 0.6f, 1f, 1f),    // Azul
            new Color(0.7f, 0.7f, 0.7f, 1f),  // Gris
            new Color(1f, 1f, 1f, 1f)         // Blanco
        };

        originalColor = starColors[Random.Range(0, starColors.Length)];
        spriteRenderer.color = originalColor;

        float scale = Random.Range(0.1f, 0.3f);
        transform.localScale = Vector3.one * scale;
    }

    public void UpdateParticle(float speedMultiplier, Vector3 mousePosition)
    {
        lifeTime += Time.deltaTime;

        // Movimiento radial desde el centro
        transform.position += moveDirection * moveSpeed * speedMultiplier * Time.deltaTime;

        // Rotación suave
        transform.Rotate(0, 0, rotationSpeed * speedMultiplier * Time.deltaTime);

        // Efecto de mouse más sutil
        float distanceToMouse = Vector3.Distance(transform.position, mousePosition);
        if (distanceToMouse < mouseDetectionRadius)
        {
            float intensity = (mouseDetectionRadius - distanceToMouse) / mouseDetectionRadius;
            spriteRenderer.color = Color.Lerp(originalColor, new Color(1f, 0.5f, 0.5f, 1f), intensity * 0.3f);
        }
        else
        {
            spriteRenderer.color = originalColor;
        }

        // Reposicionar si sale de la pantalla o después de mucho tiempo
        if (transform.position.magnitude > 12f || lifeTime > maxLifeTime)
        {
            Destroy(gameObject);
        }
    }

    public void ApplyRotation(float rotationForce)
    {
        // Rotación más suave alrededor del centro
        currentRotation += rotationForce * 0.5f;
        float angle = currentRotation * Mathf.Deg2Rad;

        Vector3 currentPos = transform.position;
        float radius = currentPos.magnitude;
        float currentAngle = Mathf.Atan2(currentPos.y, currentPos.x);

        float newAngle = currentAngle + angle;
        float newX = Mathf.Cos(newAngle) * radius;
        float newY = Mathf.Sin(newAngle) * radius;

        transform.position = new Vector3(newX, newY, 0);

        // Actualizar dirección de movimiento
        moveDirection = new Vector3(Mathf.Cos(newAngle), Mathf.Sin(newAngle), 0);
    }
}

using UnityEngine;

public class PumpParticle : MonoBehaviour
{
    public float minSize = 0.3f;
    public float maxSize = 0.6f;
    public float pulseSpeed = 3f;
    public float moveSpeed = 1f;

    private SpriteRenderer spriteRenderer;
    private Vector3 moveDirection;
    private float pulseTimer = 0f;
    private float colorTimer = 0f;
    private GameObject border;

    void Start()
    {
        spriteRenderer = GetComponent<SpriteRenderer>();
        moveDirection = Random.insideUnitCircle.normalized;

        SetupWhiteBorder();

        CircleCollider2D collider = gameObject.AddComponent<CircleCollider2D>();
        collider.isTrigger = true;
    }

    void SetupWhiteBorder()
    {
        border = new GameObject("Border");
        border.transform.SetParent(transform);
        border.transform.localPosition = Vector3.zero;
        border.transform.localScale = Vector3.one * 1.2f;

        SpriteRenderer borderRenderer = border.AddComponent<SpriteRenderer>();
        borderRenderer.sprite = spriteRenderer.sprite;
        borderRenderer.color = Color.white;
        borderRenderer.sortingOrder = -1;
    }

    public void UpdateParticle(float speedMultiplier)
    {
        // Movimiento aleatorio
        transform.position += moveDirection * moveSpeed * speedMultiplier * Time.deltaTime;

        // Efecto de pulso/vibración
        pulseTimer += Time.deltaTime * pulseSpeed * speedMultiplier;
        float scale = Mathf.Lerp(minSize, maxSize, (Mathf.Sin(pulseTimer) + 1f) / 2f);
        transform.localScale = Vector3.one * scale;

        // Cambio de color arcoíris suave
        colorTimer += Time.deltaTime * speedMultiplier;
        UpdateRainbowColor();

        // Cambiar dirección ocasionalmente
        if (Random.Range(0f, 1f) < 0.01f * speedMultiplier)
        {
            moveDirection = Random.insideUnitCircle.normalized;
        }

        // Mantener dentro de la pantalla
        Vector3 viewportPos = Camera.main.WorldToViewportPoint(transform.position);
        if (viewportPos.x < 0 || viewportPos.x > 1 || viewportPos.y < 0 || viewportPos.y > 1)
        {
            moveDirection = -moveDirection;
        }
    }

    void UpdateRainbowColor()
    {
        // Colores del arcoíris en transición suave
        float hue = (colorTimer * 0.3f) % 1f; // Cambio lento de color

        Color rainbowColor = Color.HSVToRGB(hue, 0.8f, 1f);
        spriteRenderer.color = rainbowColor;

        // Actualizar borde también
        if (border != null)
        {
            SpriteRenderer borderRenderer = border.GetComponent<SpriteRenderer>();
            borderRenderer.color = new Color(1f, 1f, 1f, 0.8f); // Borde blanco semi-transparente
        }
    }



    void OnMouseDown()
    {
        ParticleSystemManager manager = FindObjectOfType<ParticleSystemManager>();
        if (manager != null) manager.OnPumpParticleClicked(gameObject);
    }
}


```

</details>



<img width="974" height="547" alt="image" src="https://github.com/user-attachments/assets/9b00c14b-d259-457f-9d8d-5da1f60ff86b" />



### Autoevaluación

Cumpliendo lo que dice la rubrica, complete dos actividades ya que no hice la primera en la bitacora, las otras 2 si las hice completas por lo que mi nota final sería 

__3.0__
