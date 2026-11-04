# Informe sobre a tolerancia de fallos con MongoDB

## Parte A.
### 1. Montar o replica set
1. Arrancar os tres nodos
    ```bash
    docker compose up -d
    ```
2. Iniciar o replica set
    ```bash
    docker compose exec mongo1 mongosh /scripts/iniciar_rs.js
    ```
3. Esperar uns segundos e ver o resultado
    ```bash
    docker compose exec mongo1 mongosh --quiet /scripts/estado.js
    ```
#### Preguntas:
- **Cal é o primario e cales os secundarios?**

- **Compara o compose.yaml co da demo de Sentinel:** que servizos hai aquí e cales faltan?

### 2. Carga de datos
1. Cargar as fichas conectando ao replica set:
    ```bash
    docker compose exec mongo1 mongosh "mongodb://mongo1,mongo2,mongo3/?replicaSet=rs0" /scripts/fichas.js
    ```
2. Abrir unha consola conectada ao relica set e cmrpobar cantas fichas hai
    ```bash
    docker compose exec mongo1 mongosh
    use albergues
    ```

#### Preguntas:
- **A cadea de conexión leva os tres nodos. Por que?**

- **Que facía o cliente da demo de Sentinel para saber cal era o primario?**

### 3. Ler e escribir nos secundarios
1. Conectarse a un nodo secundario
    ```bash
    docker compose exec mongo2 mongosh
    ```
    e intentar:
    - inserir unha ficha
    - consultar as fichas
2. Conectarse ao replica set e ver que nodo responde a unha consulta
    ```bash
    use albergues
    db.fichas.find().explain().serverInfo.host
    db.getMongo().setReadPref("secondary")
    db.fichas.find().explain().serverInfo.host
    ```