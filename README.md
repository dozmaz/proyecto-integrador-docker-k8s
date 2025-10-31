# Proyecto Final - Docker & Kubernetes

**Alumno:** GUIDO CUTIPA YUJRA
**Fecha:** 2025-10-31
**Curso:** Docker & Kubernetes - i-Quattro

## Links de Docker Hub
- Backend v2.1: https://hub.docker.com/r/guidocutipa/springboot-api/tags
- Frontend v2.2: https://hub.docker.com/r/guidocutipa/angular-frontend/tags

## Parte 1: Setup del Ambiente

**Ambiente utilizado:**
- [VirtualBox]
- Nombre de VM/Instancia: [guido-cutipa-yujra-k8s]
- Sistema operativo: Ubuntu 24.04 LTS
- Recursos: 12GB RAM, 3 CPU cores
- Red configurada: [Bridged]
- Rango MetalLB: [192.168.100.100-192.168.100.110]

### Screenshots

- Screenshot de `microk8s status` mostrando todos los addons habilitados (debe verse el hostname con tu nombre)

![img.png](screenshots/parte1-microk8s-status.png)

- Screenshot de `kubectl get all -n proyecto-integrador` mostrando todos los pods Running (terminal con hostname visible)

![img_1.png](screenshots/parte1-kubectl-get-all.png)

- Screenshot del navegador accediendo al frontend via IP de MetalLB

![img_2.png](screenshots/parte1-web-1.png)

![img_3.png](screenshots/parte1-web-2.png)

![img_4.png](screenshots/parte1-web-3.png)

![img_5.png](screenshots/parte1-web-4.png)

- Screenshot de la configuración de la VM (VirtualBox) mostrando el nombre con tu nombre completo

![img_6.png](screenshots/parte1-virtualbox.png)

## Parte 2: Backend v2.1

*Como la instrucción decía agregar y no sustituir el endpoint* he modificado el nombre del endpoint debido a que estaba duplicado con otro existente

mi nuevo endpoint se llama "/api/*getinfo*"

### Código Agregado

![img_7.png](screenshots/parte2-Codigo-Agregado.png)

### Screenshots

- Screenshot de `docker images` mostrando la imagen v2.1

![img_8.png](screenshots/parte2-img_8.png)

- Link a tu imagen en Docker Hub:

[https://hub.docker.com/r/guidocutipa/springboot-api/tags](https://hub.docker.com/r/guidocutipa/springboot-api/tags)

- Screenshot de `kubectl rollout status` durante la actualización

![img_11.png](screenshots/parte2-img_11.png)

- Screenshot de `kubectl get pods` mostrando los pods con la nueva versión

![img_12.png](screenshots/parte2-img_12.png)

- Screenshot o output de `curl http://127.0.0.1/api/getinfo` mostrando la respuesta JSON

*Tuve que realizar varios intentos hasta que por fin se recreo el pod y respondió el nuevo enpoint creado*

![img_13.png](screenshots/parte2-img_13.png)

![img_14.png](screenshots/parte2-img_14.png)

## Parte 3: Frontend v2.2

Código modificado de Angular (screenshots de .html y .ts)

### Screenshots

- Código modificado de Angular (screenshots de .html y .ts)

![img_15.png](screenshots/parte3-img_15.png)

![img_16.png](screenshots/parte3-img_16.png)

![img_17.png](screenshots/parte3-img_17.png)

- Link a tu imagen en Docker Hub: `https://hub.docker.com/r/guidocutipa/angular-frontend/tags`

![img_18.png](screenshots/parte3-img_18.png)

- Screenshot de `kubectl get pods -w` durante el rolling update del frontend

![img_19.png](screenshots/parte3-img_19.png)

![img_20.png](screenshots/parte3-img_20.png)

- Screenshot del navegador mostrando el botón "Ver Info del Sistema"

![img_21.png](screenshots/parte3-img_21.png)

- Screenshot del navegador mostrando la información del sistema cargada

![img_22.png](screenshots/parte3-img_22.png)

## Parte 4: Gestión de Versiones

### ¿Qué hace kubectl rollout undo?

Revierte un Deployment al ReplicaSet anterior registrado en su historial, deshaciendo el último rollout.

### Screenshots

- Screenshot de `kubectl rollout history` del backend

![img_23.png](screenshots/parte4-img_23.png)

- Screenshot de `kubectl rollout history` del frontend

![img_24.png](screenshots/parte4-img_24.png)

- Screenshot del proceso de rollback (undo)

![img_26.png](screenshots/parte4-img_26.png)

![img_25.png](screenshots/parte4-img_25.png)

- Screenshot verificando que `/api/info` dejó de funcionar después del rollback

![img_28.png](screenshots/parte4-img_28.png)

- Screenshot del rollforward (undo --to-revision=2)

la revision 2 se elimino en el anterior paso por lo que no se pudo hacer rollback a la revision 2

![img_27.png](screenshots/parte4-img_27.png)

en su lugar se hizo rollback a la revisión 4

![img_29.png](screenshots/parte4-img_29.png)

- Screenshot verificando que `/api/info` volvió a funcionar

![img_30.png](screenshots/parte4-img_30.png)

## Parte 5: Ingress + MetalLB

**IP del Ingress:** 192.168.100.71

### Screenshots

- Screenshot de `kubectl get ingress` mostrando la IP asignada

![img_31.png](screenshots/parte5-img_31.png)

- Screenshot de `kubectl describe ingress` mostrando las rutas configuradas

![img_32.png](screenshots/parte5-img_32.png)

- Screenshot del navegador accediendo a `http://192.168.100.71/` (frontend)

![img_33.png](screenshots/parte5-img_33.png)

- Screenshot de curl a `/api/getinfo` desde la IP de MetalLB

![img_34.png](screenshots/parte5-img_34.png)

- Screenshot de curl a `/actuator/health` mostrando status UP

![img_35.png](screenshots/parte5-img_35.png)


## Conclusiones

### Aprendizajes principales
- *Parte 1*

1. La configuración de red (rango MetalLB y modo de la VM) es crítica para acceso externo.
2. Documentar con screenshots y hostname es imprescindible para validación.
3. Verificación básica (status, describe, logs) resuelve la mayoría de fallos iniciales.

- *Parte 2*

1. Usar tags únicos para imágenes y automatizar versionado.
2. Ver logs y describe para diagnosticar pull/CrashLoop.
3. Probar localmente el endpoint antes de construir la imagen.
4. Entender caché de imágenes en microk8s y cómo forzar actualización.


### Dificultades encontradas
- no pude compilar el proyecto base desde el inicio en ubuntu desktop por lo que tuve que instalar ubuntu server en una maquina virtual y ahi si pude compilar el proyecto sin errores.
- la imagen del frontend no se actualizaba en kubernetes a pesar de hacer build y push de la nueva version, tuve que eliminar manualmente la imagen y forzar la descarga 

### Reflexión
[¿Cómo aplicarías esto en un proyecto real?]