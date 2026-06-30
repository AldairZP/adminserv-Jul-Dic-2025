# Ruta de aprendizaje de volúmenes en Kubernetes

## Introducción

En entornos reales de Kubernetes (empresas, DevOps, SRE y Cloud), no es necesario aprender todos los tipos de volúmenes existentes. Aproximadamente el **90% de los casos** utilizan un conjunto reducido de ellos.

La siguiente ruta está ordenada de los más utilizados a los menos utilizados.

---

# Frecuencia de uso

| Orden | Volumen | Frecuencia | Prioridad |
|--------|----------|------------|------------|
| 1 | `PersistentVolumeClaim (PVC)` | Muy alta | Imprescindible |
| 2 | `emptyDir` | Muy alta | Imprescindible |
| 3 | `ConfigMap` | Muy alta | Imprescindible |
| 4 | `Secret` | Muy alta | Imprescindible |
| 5 | `projected` | Alta | Importante |
| 6 | `CSI` | Alta | Importante |
| 7 | `hostPath` | Media | Conocerlo |
| 8 | `downwardAPI` | Baja-Media | Conocerlo |
| 9 | `ephemeral` | Baja | Opcional |

Los demás (`nfs`, `iscsi`, `cephfs`, `fc`, etc.) normalmente solo se estudian si la infraestructura de la empresa los utiliza.

---

# Ruta de aprendizaje

## Nivel 1 — Conceptos básicos (70% de los casos)

### 1. emptyDir

### ¿Qué aprender?

- Qué es un `emptyDir`.
- Cuándo se crea.
- Cuándo se elimina.
- Cómo compartir archivos entre contenedores del mismo Pod.
- Cuándo utilizar almacenamiento temporal.

### Debes poder responder

- ¿Qué sucede si un contenedor se reinicia?
- ¿Qué sucede si el Pod es eliminado?

---

### 2. ConfigMap

### ¿Qué aprender?

- Crear ConfigMaps.
- Montarlos como archivos.
- Utilizarlos como variables de entorno.
- Actualizar ConfigMaps.
- Diferencias entre montar un ConfigMap y usar variables de entorno.

Debes comprender perfectamente la diferencia entre:

```yaml
env:
```

y

```yaml
volumeMounts:
```

---

### 3. Secret

Aprende prácticamente lo mismo que con ConfigMap.

### Además debes conocer

- Base64.
- `secretKeyRef`.
- Montar Secrets como archivos.
- Montar Secrets como variables de entorno.

Debes comprender la diferencia entre:

```yaml
configMapKeyRef
```

y

```yaml
secretKeyRef
```

---

# Nivel 2 — Persistencia (20% restante)

Este es probablemente el tema más importante del almacenamiento en Kubernetes.

## 1. PersistentVolume (PV)

Aprende:

- Qué es.
- Quién lo crea.
- Dónde existe.
- Qué representa físicamente.

---

## 2. PersistentVolumeClaim (PVC)

Aprende:

- Cómo un Pod solicita almacenamiento.
- Cómo se enlaza con un PV.
- Cómo se monta dentro del contenedor.
- Qué ocurre cuando un Pod es eliminado.

---

## 3. StorageClass

Aprende:

- Qué es un StorageClass.
- Provisionamiento dinámico.
- Cómo crea automáticamente un PersistentVolume.

---

## Debes comprender este flujo

```text
Deployment
      │
      ▼
PVC
      │
      ▼
StorageClass
      │
      ▼
CSI Driver
      │
      ▼
Disco físico o almacenamiento en la nube
```

Una vez entendido este flujo, comprenderás cómo Kubernetes administra el almacenamiento persistente.

---

# Nivel 3 — Casos especiales

## projected

Aprende:

- Combinar varios orígenes en un único volumen.
- ConfigMap.
- Secret.
- Downward API.
- ServiceAccount Token.

Es común utilizarlo para reunir toda la configuración de una aplicación en un solo directorio.

---

## CSI (Container Storage Interface)

No es necesario aprender a desarrollar un CSI Driver.

Solo debes comprender cómo participa dentro de la arquitectura.

```text
Pod
 ↓
PVC
 ↓
StorageClass
 ↓
CSI Driver
 ↓
Proveedor de almacenamiento

    AWS EBS
    Azure Disk
    Google Persistent Disk
    Ceph
    NetApp
```

---

# Nivel 4 — Administración y casos específicos

## hostPath

Aprende:

- Qué hace.
- Cuándo utilizarlo.
- Riesgos de seguridad.
- Dependencia del nodo donde se ejecuta el Pod.

Generalmente se utiliza para:

- Agentes de monitoreo.
- Recolección de logs.
- DaemonSets.
- Entornos de laboratorio.

---

## downwardAPI

Aprende que permite obtener información del propio Pod.

Por ejemplo:

- Nombre del Pod.
- Namespace.
- Labels.
- Annotations.
- Dirección IP.

No suele utilizarse en aplicaciones comunes, pero es útil conocerlo.

---

## ephemeral

Solo es necesario saber que existe.

Se utiliza en escenarios muy específicos y rara vez aparece en aplicaciones de negocio.

---

# Ruta completa de aprendizaje

```text
1. emptyDir
        ↓
2. ConfigMap
        ↓
3. Secret
        ↓
4. PersistentVolume (PV)
        ↓
5. PersistentVolumeClaim (PVC)
        ↓
6. StorageClass
        ↓
7. CSI
        ↓
8. projected
        ↓
9. hostPath
        ↓
10. downwardAPI
        ↓
11. ephemeral
```

---

# Objetivo final

Al finalizar esta ruta deberías comprender completamente:

- Almacenamiento temporal.
- Configuración mediante ConfigMaps.
- Manejo seguro de credenciales con Secrets.
- Persistencia mediante PV y PVC.
- Provisionamiento dinámico con StorageClass.
- Integración con proveedores de almacenamiento mediante CSI.
- Casos especiales de montaje de volúmenes.

Con estos conocimientos cubrirás aproximadamente el **90% de los escenarios reales** relacionados con volúmenes en Kubernetes.