# Nodo 1 - Primario

Imagen: bitnami/postgresql:16
Rol: Primario (acepta lecturas y escrituras)
Configuracion: variables de entorno en docker-compose.yml (seccion db-node1)
Puerto interno: 5432 (expuesto solo a traves de HAProxy, no directo al host)
