# Aplicación de Consulta del Clima 🌦️

Una aplicación móvil desarrollada en Android Studio que permite obtener información sobre el clima de cualquier ciudad ingresada por el usuario. Utiliza Retrofit para consumir una API REST de clima en tiempo real.

---

## 🛠️ Funcionalidades
- **Búsqueda de Clima:**
  - Permite al usuario ingresar el nombre de una ciudad.
  - Muestra la temperatura actual.
  - Proporciona una descripción breve del estado del clima (por ejemplo: "Soleado", "Nublado").

---

## 📂 Estructura de la Aplicación
- **Pantalla Principal:**
  - Campo de texto para ingresar el nombre de la ciudad.
  - Botón para obtener la información del clima.
  - Etiquetas para mostrar la temperatura y la descripción del clima.

- **Lógica del Backend:**
  - Se utiliza Retrofit para realizar peticiones a una API REST.
  - El endpoint utilizado es proporcionado por una API externa, como OpenWeatherMap.

---

## 🔧 Tecnologías Utilizadas
- Lenguaje: **Kotlin**.
- Framework: **Android Studio**.
- API Externa: **OpenWeatherMap**.
- Librerías:
  - **Retrofit**: Para consumir la API REST.
  - **Gson**: Para procesar el JSON de la respuesta.

---

## 🚀 Configuración del Proyecto
1. Descarga y descomprime el archivo desde el enlace proporcionado.
2. Abre el proyecto en **Android Studio**.
3. Configura la clave de la API:
   - Ve a [OpenWeatherMap](https://openweathermap.org/) y crea una cuenta para obtener una clave API.
   - Sustituye la clave en la línea correspondiente de `MainActivity`:
     ```kotlin
     private val apiKey = "TU_CLAVE_API"
     ```
4. Ejecuta la aplicación en un emulador o dispositivo físico.

---

## 🖼️ Capturas de Pantalla
### Pantalla Principal:
![image](https://github.com/user-attachments/assets/c62559c1-2d38-407e-8d62-1541065953e0)
![image](https://github.com/user-attachments/assets/f9a14b25-f721-4ff6-b9e1-7f66769b39ea)

---

## 🌟 Código Relevante
```kotlin
private fun fetchWeather(city: String) {
    binding.tvTemperature.text = "Cargando..."
    binding.tvDescription.text = ""

    val call = RetrofitClient.instance.getWeather(city, apiKey)
    call.enqueue(object : Callback<WeatherResponse> {
        override fun onResponse(call: Call<WeatherResponse>, response: Response<WeatherResponse>) {
            if (response.isSuccessful) {
                val weatherResponse = response.body()
                if (weatherResponse != null) {
                    val temperature = weatherResponse.main.temp
                    val description = weatherResponse.weather[0].description

                    binding.tvTemperature.text = "Temperatura: $temperature°C"
                    binding.tvDescription.text = "Descripción: $description"
                }
            } else {
                binding.tvTemperature.text = "Error al obtener datos"
                Log.e("WeatherApp", "Error: ${response.code()}")
            }
        }

        override fun onFailure(call: Call<WeatherResponse>, t: Throwable) {
            binding.tvTemperature.text = "Error de conexión"
            Toast.makeText(this@MainActivity, "Error: ${t.message}", Toast.LENGTH_SHORT).show()
            Log.e("WeatherApp", "Error: ${t.message}")
        }
    })
}

