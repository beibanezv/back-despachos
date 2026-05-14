# Back Despachos

Backend que administra los despachos de las compras.

## Qué hace

- Crea despachos.
- Lista despachos.
- Actualiza intentos y estado de entrega.

## Cómo funciona con EC2

- Este backend corre en su EC2 con Docker.
- Se expone por el puerto `8081`.
- Usa MySQL en contenedor con volumen para no perder datos.

URL principal:

- `http://<IP_EC2_BACK_DESPACHOS>:8081/api/v1/despachos`

## Persistencia

- Volumen usado: `mysql_despachos_data`.
- Los datos se mantienen aunque reinicies contenedores.

## Despliegue automático (CI/CD)

Con push a rama `deploy`:

1. Se construye imagen Docker.
2. Se publica en Docker Hub.
3. Se copia `docker-compose.yml` a EC2.
4. Se actualizan contenedores en EC2.

Workflow: `.github/workflows/deploy.yml`

## Prueba rápida

Crear despacho:

```bash
curl -X POST "http://<IP_EC2_BACK_DESPACHOS>:8081/api/v1/despachos" -H "Content-Type: application/json" -d "{\"fechaDespacho\":\"2026-05-15\",\"patenteCamion\":\"ABC123\",\"intento\":0,\"entregado\":false,\"idCompra\":1,\"direccionCompra\":\"Calle Falsa 123\",\"valorCompra\":12345}"
```

Listar despachos:

```bash
curl "http://<IP_EC2_BACK_DESPACHOS>:8081/api/v1/despachos"
```
