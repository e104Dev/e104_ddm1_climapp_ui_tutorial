# e104_ddm1_climapp_ui_tutorial

# ClimApp Tutorial
##Parte I - Esta versão não utiliza a localização do usuário
Esta primeira versão do App iremos consumir a previsão de clima da HGBrasil sem utilizar GPS.

<img src="" alt="ScreenShot ClimaApp" height="480px">

### OPCIONAL - Tutorial ClimApp completo com GPS
Neste tutorial voces podem fazer o App completo, utilizem de o preferirem [Link GiLab](https://gitlab.com/esdrasilva/climapphowto). Porem nesta versão não é utilizado RecyclerViews e também não foi utilizado o Jackson para conversão de dados de JSON para POJO.

#### Step I - Dependencias
Vá ao arquivo gradle.build (Module App) e adicione as dependencias abaixo:
```javascript
    implementation 'com.android.volley:volley:1.2.1'
    implementation 'com.fasterxml.jackson.core:jackson-databind:2.13.4'
    implementation "com.fasterxml.jackson.module:jackson-module-kotlin:2.13.4"
```
#### Setp II - Layout
1. Atualize o arquivo `main_activity.xml` com o código abaixo:
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:background="@drawable/gradient_animation"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/root_layout"
        tools:context=".MainActivity">
    <TextView
            android:text="Araras"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/textViewCidade"
            android:textColor="@android:color/white"
            android:textSize="32sp"
            app:layout_constraintTop_toTopOf="parent"
            android:layout_marginTop="8dp"
            app:layout_constraintStart_toStartOf="parent"
            android:layout_marginStart="8dp"/>
    <TextView
            android:text='60"'
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/textViewTemperatura"
            android:textSize="120sp"
            android:textColor="@android:color/white"
            app:layout_constraintStart_toStartOf="parent"
            android:layout_marginStart="8dp"
            app:layout_constraintTop_toBottomOf="@+id/textViewData"/>
    <TextView
            android:text="12:57"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/textViewHora"
            android:textSize="24sp"
            android:textColor="@android:color/white"
            app:layout_constraintEnd_toEndOf="parent"
            android:layout_marginEnd="8dp"
            android:layout_marginTop="8dp"
            app:layout_constraintTop_toTopOf="@+id/textViewCidade"
            app:layout_constraintBottom_toBottomOf="@+id/textViewCidade"/>
    <ImageView
            android:layout_width="100dp"
            android:layout_height="100dp"
            android:id="@+id/imageViewIcon"
            app:layout_constraintTop_toTopOf="@+id/linearLayout"
            app:layout_constraintBottom_toBottomOf="@+id/linearLayout"
            app:layout_constraintStart_toEndOf="@+id/linearLayout"
            android:layout_marginStart="24dp"
            app:srcCompat="@drawable/cloud_day" android:backgroundTintMode="src_in"
            app:tint="@android:color/white"
            android:backgroundTint="@android:color/white"/>
    <LinearLayout
            android:orientation="vertical"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/linearLayout"
            app:layout_constraintTop_toTopOf="@+id/textViewTemperatura"
            app:layout_constraintBottom_toBottomOf="@+id/textViewTemperatura"
            android:layout_marginStart="8dp"
            app:layout_constraintStart_toEndOf="@+id/textViewTemperatura">
        <TextView
                android:text="Máx"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:id="@+id/textView3"
                android:textColor="@android:color/white"
                android:layout_weight="1"
                android:textAlignment="viewStart"
        />
        <TextView
                android:text="20"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:id="@+id/textViewMaxima"
                android:textColor="@android:color/white"
                android:layout_weight="1"
                android:textAlignment="viewStart"
                android:textStyle="bold"
                android:textSize="20sp"/>
        <TextView
                android:text="Mín"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:id="@+id/textView8"
                android:textColor="@android:color/white"
                android:layout_weight="1"
                android:textAlignment="viewStart"/>
        <TextView
                android:text="20"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:id="@+id/textViewMinima"
                android:textColor="@android:color/white"
                android:layout_weight="1"
                android:textAlignment="viewStart"
                android:textSize="20sp"
                android:textStyle="bold"/>
    </LinearLayout>
    <TextView
            android:text="Nublado"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/textViewTempoCelula"
            android:textColor="@android:color/white"
            android:textSize="32sp"
            app:layout_constraintTop_toBottomOf="@+id/textViewTemperatura"
            app:layout_constraintStart_toStartOf="parent"
            android:layout_marginStart="8dp"/>
    <TextView
            android:text="05:07"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/textViewNascerDoSol"
            android:textColor="@android:color/white"
            android:layout_weight="1"
            android:textAlignment="viewStart"
            app:layout_constraintStart_toEndOf="@+id/textView11"
            android:layout_marginStart="5dp"
            app:layout_constraintTop_toTopOf="@+id/textView11"
            app:layout_constraintBottom_toBottomOf="@+id/textView11"
            android:textStyle="bold"/>
    <TextView
            android:text="Nascer do Sol: "
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/textView11"
            android:textColor="@android:color/white"
            android:layout_weight="1"
            android:textAlignment="viewStart"
            app:layout_constraintStart_toStartOf="@+id/textViewTempoCelula"
            app:layout_constraintTop_toBottomOf="@+id/textViewTempoCelula"/>
    <TextView
            android:text="17:50"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/textViewPorDoSol"
            android:textColor="@android:color/white"
            android:layout_weight="1"
            android:textAlignment="viewStart"
            app:layout_constraintStart_toEndOf="@+id/textView13"
            android:layout_marginStart="8dp"
            app:layout_constraintTop_toTopOf="@+id/textView13"
            app:layout_constraintBottom_toBottomOf="@+id/textView13"
            android:textStyle="bold"/>
    <TextView
            android:text="Por do Sol: "
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/textView13"
            android:textColor="@android:color/white"
            android:layout_weight="1"
            android:textAlignment="viewStart"
            app:layout_constraintStart_toEndOf="@+id/textViewNascerDoSol"
            android:layout_marginStart="32dp"
            app:layout_constraintTop_toTopOf="@+id/textViewNascerDoSol"
            app:layout_constraintBottom_toBottomOf="@+id/textViewNascerDoSol"/>
    <TextView
            android:text="TextView"
            android:layout_width="226dp"
            android:layout_height="wrap_content"
            android:id="@+id/textViewData"
            android:textColor="@android:color/white"
            android:textAlignment="viewStart"
            app:layout_constraintTop_toBottomOf="@+id/textViewCidade"
            app:layout_constraintStart_toStartOf="@+id/textViewCidade"/>

    <androidx.recyclerview.widget.RecyclerView
            android:id="@+id/recyclerView"
            android:layout_width="match_parent"
            android:layout_height="0dp"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toBottomOf="@+id/textView11" />
</androidx.constraintlayout.widget.ConstraintLayout>
```
2. Crie o arquivo `previsao_cell.xml` e adicione o código abaixo:
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              xmlns:app="http://schemas.android.com/apk/res-auto" android:orientation="horizontal"
              android:layout_width="match_parent"
              android:layout_height="wrap_content" android:layout_margin="5dp">

    <ImageView
            android:layout_width="75dp"
            android:layout_height="wrap_content"
            app:srcCompat="@drawable/cloudy"
            android:id="@+id/imageViewIconWeather"
            android:layout_weight="1"
            android:cropToPadding="true"
            android:adjustViewBounds="true"
            android:padding="5dp"
            android:layout_margin="3dp"
            app:tint="@android:color/white" />
    <LinearLayout
            android:orientation="vertical"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_weight="2"
            android:layout_margin="3dp">
        <TextView
                android:text="TextView"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:id="@+id/textViewTempoCelula"
                android:textSize="18sp"
                android:textColor="@android:color/white"
                android:layout_marginLeft="3dp"/>
        <TextView
                android:text="TextView"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:id="@+id/textViewMaxMinCelula"
                android:textSize="14sp"
                android:textColor="@android:color/white"
                android:layout_marginLeft="3dp"/>
    </LinearLayout>
</LinearLayout>
``` 
3. Arquivos para animação de Gradiente:
3.1. Crie o arquivo `gradiente_start.xml` no diretório `drawable`.
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
       android:shape="rectangle">
    <gradient
            android:type="linear"
            android:angle="90"
            android:startColor="@color/colorGradientStart"
            android:centerColor="@color/colorGradientCenter"
            android:endColor="@color/colorGradientEnd"/>
</shape>
```
3.2. Crie o arquivo `gradiente_center.xml` no diretório `drawable`.
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
       android:shape="rectangle">
    <gradient
            android:type="linear"
            android:angle="90"
            android:startColor="@color/colorGradientCenter"
            android:centerColor="@color/colorGradientEnd"
            android:endColor="@color/colorGradientStart"/>
</shape>
```
3.3. Crie o arquivo `gradiente_end.xml` no diretório `drawable`.
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
       android:shape="rectangle">
    <gradient
            android:type="linear"
            android:angle="90"
            android:startColor="@color/colorGradientEnd"
            android:centerColor="@color/colorGradientStart"
            android:endColor="@color/colorGradientCenter"/>
</shape>
```
3.4. Crie o arquivo `gradiente_animation.xml` no diretório `drawable` que ira ser o container dos demais gradientes.
```xml
<?xml version="1.0" encoding="utf-8"?>
<animation-list xmlns:android="http://schemas.android.com/apk/res/android">

    <item android:duration="5000"
          android:drawable="@drawable/gradient_start"/>

    <item android:duration="5000"
          android:drawable="@drawable/gradient_center"/>

    <item android:duration="5000"
          android:drawable="@drawable/gradient_end"/>

</animation-list>
```
3.5. Atualize o arquivo `res/colors.xml` com o trecho abaixo.

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="colorPrimary">#CA00F9</color>
    <color name="colorPrimaryDark">#4E0DFD</color>
    <color name="colorAccent">#D81B60</color>
    <color name="colorTransparent">#00ffffff</color>

    <color name="colorGradientStart">#4E0DFD</color>
    <color name="colorGradientCenter">#CA00F9</color>
    <color name="colorGradientEnd">#FD0D5D</color>
</resources>
```

3.6. Baixe os icones utilizados e os adicione na pasta `drawable` [Link Icones](https://gitlab.com/esdrasilva/climapp/-/archive/master/climapp-master.zip?path=app/src/main/res/drawable).
