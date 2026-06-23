# 🗺️ GeoPortal - Módulo de Incidentes

Este módulo gestiona la captura y el envío de reportes geolocalizados utilizando una arquitectura limpia y moderna en Android.

---

### 📂 Estructura del Proyecto (Arquitectura MVVM)

El proyecto está estructurado por capas para separar las responsabilidades, facilitar las pruebas unitarias y garantizar la escalabilidad:

```text
app/src/main/java/com/tuempresa/geoportal/
│
├── data/                            # 🗄️ CAPA DE DATOS (Gestión de fuentes de información)
│   ├── model/
│   │   └── IncidenteDto.kt          # Objetos de transferencia de datos (Data Transfer Objects)
│   ├── network/
│   │   ├── RetrofitClient.kt        # Instancia única de la red (Singleton / DRY)
│   │   └── IncidenteApiService.kt   # Definición de Endpoints de Retrofit
│   └── repository/
│       └── IncidenteRepository.kt   # Única fuente de verdad (Decide el flujo de datos)
│
├── domain/                          # 🧠 CAPA DE NEGOCIO (Reglas puras, omitida en demo para evitar sobreingeniería)
│
└── ui/                              # 🎨 CAPA DE INTERFAZ (Renderizado visual y eventos)
    ├── viewmodel/
    │   └── IncidenteViewModel.kt    # Cerebro de la pantalla. Gestiona el estado de Compose
    └── screen/
        └── FormularioScreen.kt      # Interfaz declarativa en Jetpack Compose
```

---

### 🛠️ Tecnologías Utilizadas

* **Jetpack Compose**: Interfaz de usuario nativa y declarativa.
* **Retrofit 2**: Cliente HTTP con seguridad de tipos para la comunicación con la API.
* **Kotlin Coroutines / Flow**: Gestión de tareas asíncronas y flujos de datos reactivos sin bloquear el hilo principal.
* **Moshi / Gson**: Serialización y deserialización de datos JSON a objetos Kotlin.
* **ViewModel (Jetpack)**: Persistencia y gestión del estado de la pantalla frente a cambios de configuración.

---

### 🔄 Flujo de Datos (Data Flow)

El flujo de información sigue un patrón unidireccional estricto para evitar efectos secundarios:

1. **Usuario**: Interactúa con `FormularioScreen.kt` e introduce los datos del incidente.
2. **UI a ViewModel**: La pantalla notifica el evento al `IncidenteViewModel.kt`.
3. **Procesamiento de Estado**: El ViewModel cambia su estado a `Cargando` y ejecuta una Corrutina.
4. **ViewModel a Repositorio**: Se invoca la función de envío en `IncidenteRepository.kt`.
5. **Repositorio a API**: El repositorio coordina con `IncidenteApiService.kt` mediante Retrofit para enviar el `IncidenteDto.kt` al servidor.
6. **Respuesta**: El resultado regresa en sentido inverso, actualizando el estado de la UI a `Éxito` o `Error`.

---

### 🚀 Configuración y Ejecución

#### Prerrequisitos
* Android Studio (versión reciente con soporte para Jetpack Compose).
* JDK 17 o superior.

