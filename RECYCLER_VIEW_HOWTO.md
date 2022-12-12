# Implementação do Adapter

Com base na documentação oficial, iremos implementar uma RecyclerView.

1. Na classe `WeatherAdapter`, crie uma classe com o trecho abaixo.

```kotlin
class WeatherViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {
    val imageView = itemView.findViewById<ImageView>(R.id.imageViewIconWeather)
    val textViewTempoCelula = itemView.findViewById<TextView>(R.id.textViewTempoCelula)
    val textViewTempMaxMin = itemView.findViewById<TextView>(R.id.textViewMaxMinCelula)
}
```

> Esta classe será a representação de uma das linhas da RecyclerView. Ela conterá exatamente os Widgets que exibirão os dados da requisição à HGBrasil.

2. Ainda a classe `WeatherAdapter`, faça a extenção de `RecyclerView.Adapter`passando o tipo do ViewHolder criado no passo 2.

```kotlin
class WeatherAdapter(var forcasts: List<Forecast>) : RecyclerView.Adapter<WeatherViewHolder>() {
  //... Restante do código
```

3. Implemente os métodos obrigatórios do Adapter, dentro de `WeatherAdapter`.

```kotlin
override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): WeatherViewHolder {

        val view = LayoutInflater.from(parent.context)
            .inflate(R.layout.previsao_cell, parent, false)

        return WeatherViewHolder(view)
    }

    override fun onBindViewHolder(holder: WeatherViewHolder, position: Int) {
        val forecast = forcasts.get(position)
        holder.textViewTempoCelula.text = "${forecast?.weekday} - ${forecast?.description}"
        holder.textViewTempMaxMin.text = "Máx: ${forecast?.max} - Mín: ${forecast.min}"
        holder.imageView.setImageResource(setDrawableResource(forecast?.condition!!))
    }

    override fun getItemCount(): Int {
        return forcasts.size
    }
```

4. Crie um metodo para recuperar o id resource de um recurso em `drawable` com base no retorno da requisição; também dentro de `WeatherAdapter`

```kotlin
    fun setDrawableResource(condition: String) : Int {
        return when(condition) {
            "storm" -> R.drawable.storm
            "snow" -> R.drawable.snow
            "rain" -> R.drawable.rain
            "fog" -> R.drawable.fog
            "clear_day" -> R.drawable.sun
            "clear_night" -> R.drawable.moon
            "cloud" -> R.drawable.cloudy
            "cloudly_day" -> R.drawable.cloud_day
            "cloudly_night" -> R.drawable.cloudy_night
            else -> R.drawable.sun
        }
    }
```

5. Atualize a url da requisição para o ID de Cosmópolis no método main de `ActivityMain.kt`.

```kotlin
val url = "https://api.hgbrasil.com/weather?woeid=456158"
```

4. Após atualizar o Adapter, teste o App econfira a previsão do tempo para sua cidade agora; e na RecyclerView as previsões resumidas para os próximos 10 dias.

# Concluímos! Parabens por chegar até aqui!
